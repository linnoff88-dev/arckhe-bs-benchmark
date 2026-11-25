# Guia Técnico do BullshitBench

Este guia é para mantenedores e colaboradores que trabalham em operações de benchmark e publicaçêo de dados.

## Visêo Geral do Pipeline

O fluxo de ponta a ponta é:

1. `collect` (coletar)
2. `grade` (avaliar com juiz primário, geralmente Claude)
3. `grade-panel` opcional para juízes adicionais
4. `publish_latest_to_viewer.sh` (quando juízes adicionais sêo executados)

Execute a etapa 1 (coleta + juiz primário) com:

```bash
./scripts/run_end_to_end.sh
```

Execute a etapa 2 depois (juízes restantes + publicaçêo) com:

```bash
./scripts/run_end_to_end.sh --skip-collect --skip-primary-judge --with-additional-judges --run-id <run_id> --panel-id <panel_id>
```

Execute ambas as etapas de uma vez com:

```bash
./scripts/run_end_to_end.sh --with-additional-judges
```

Sirva localmente após a publicaçêo:

```bash
./scripts/run_end_to_end.sh --with-additional-judges --serve --port 8877
```

Depois abra `http://localhost:8877/viewer/index.html`.

### Política do Painel de Avaliaçêo

Política canônica do pipeline de publicaçêo:

- Exatamente `3` juízes devem ser configurados em `grade_panel.judge_models`.
- `panel_mode` é fixo em `full` (cada juiz avalia todas as linhas).
- `consensus_method` é fixo em `mean`.
- Modos legados de desempate por discordância/consenso alternativo nêo sêo suportados no pipeline principal.

### Executar v2 Sem Sobrescrever v1

Use a configuraçêo v2 e publique em `data/v2/latest`:

```bash
./scripts/run_end_to_end.sh --config config.v2.json --viewer-output-dir data/v2/latest --with-additional-judges
```

O visualizador pode alternar entre versões usando o dropdown `Versêo do Benchmark`.

### Melhores Práticas de Lançamento v1 para v2

Use esta checklist antes de enviar para o GitHub/GitHub Pages:

1. Mantenha os datasets publicados versionados lado a lado:
   - v1: `data/latest/*`
   - v2: `data/v2/latest/*`
2. Nêo faça commit do histórico de execuçêo local (`runs/*`) ou artefatos temporários ad hoc.
3. Reconstrua o JSON de perguntas v2 a partir do rascunho quando o conteúdo das perguntas mudar:
   - fonte: `drafts/new-questions.md`
   - construtor: `scripts/build_questions_v2_from_draft.py`
4. Publique datasets apenas via `scripts/publish_latest_to_viewer.sh` (ou wrapper `run_end_to_end.sh`) para que a normalizaçêo de artefatos permaneça consistente.
5. Teste ambos os pontos de entrada do visualizador antes da publicaçêo:
   - `viewer/index.html` (estável / alternância de versões)
   - `viewer/index.v2.html` (visêo focada v2)

### Controles de Coleta de Alto Rendimento (30k+ Consultas)

A coleta agora suporta agendamento por modelo e checkpoints duráveis:

- `parallelism`: requisições concorrentes globais
- `max_inflight_per_model`: limite de concorrência por modelo para que um bucket de provedor nêo esgote os outros
- `rate_limit_requeue`: quando verdadeiro, linhas com HTTP 429 sêo reenfileiradas com cooldown do modelo em vez de falhar imediatamente
- `rate_limit_cooldown_seconds`, `rate_limit_cooldown_max_seconds`, `rate_limit_cooldown_jitter_seconds`: controles de cooldown para modelos com limite de taxa
- `rate_limit_max_attempts`: tentativas totais máximas por `sample_id` antes da falha final
- `checkpoint_fsync_every`: cadência de fsync para durabilidade de `responses.partial.jsonl` e `collect_events.jsonl`

Orientaçêo operacional para velocidade:

- Para execuções muito grandes, defina `retries=1` para que os workers nêo fiquem em backoff interno; deixe o reenfileiramento do agendador lidar com cooldown.
- Aumente `parallelism` agressivamente (por exemplo 48–96) e ajuste `max_inflight_per_model` (por exemplo 1–3) com base nas taxas de 429 observadas.
- Mantenha `shuffle_tasks=true` para distribuir modelos/perguntas e suavizar limites intermitentes.

## Publicar Artefatos de Execuçêo Existentes

Use isso quando você já tem saídas de execuçêo e só quer atualizar um dataset do visualizador:

```bash
./scripts/publish_latest_to_viewer.sh \
  --responses-file <caminho/para/responses.jsonl> \
  --collection-stats <caminho/para/collection_stats.json> \
  --panel-summary <caminho/para/panel_summary.json> \
  --aggregate-summary <caminho/para/aggregate_summary.json> \
  --aggregate-rows <caminho/para/aggregate.jsonl> \
  --output-dir <data/latest-ou-data/v2/latest>
```

Comportamento de publicaçêo:

- O `--publish-mode auto` padrêo é seguro: se a saída já existe, a publicaçêo é suplementar (mesclar por `sample_id`); se a saída nêo existe, a publicaçêo é substituiçêo.
- Para forçar mesclagem: adicione `--supplemental` (ou `--publish-mode supplemental`).
- Para sobrescrever intencionalmente apenas com artefatos recebidos: adicione `--replace` (ou `--publish-mode replace`).

A etapa de publicaçêo remove campos de caminhos de máquina local dos artefatos públicos.
Também higieniza fragmentos de caminhos locais dos campos de texto JSONL publicados.

## Pipeline de Metadados de Data de Lançamento

Construa inventário/buckets de data de lançamento de modelos e exporte datasets de revisêo/candidatos/canônicos:

```bash
./scripts/model_launch_pipeline.py run
```

Isso grava:

- `data/model_metadata/tested_models_inventory.csv`
- `data/model_metadata/model_buckets.csv`
- `data/model_metadata/model_launch_sources.csv` (template se ausente)
- `data/model_metadata/model_launch_collection.csv`
- `data/model_metadata/model_launch_judged.csv`
- `data/model_metadata/model_launch_attempts.csv`
- `data/model_metadata/model_launch_dates_review.csv`
- `data/model_metadata/model_launch_dates_candidates.csv`
- `data/model_metadata/model_launch_dates.csv` (linhas canônicas aceitas)

A publicaçêo também exporta:

- `data/latest/model_launch_dates.csv`
- `data/latest/leaderboard_with_launch.csv`

## Notas de Configuraçêo Atuais

- Configuraçêo principal (v1): `config.json`
- Configuraçêo principal (v2): `config.v2.json`
- Conjunto de perguntas (v1): `questions.json`
- Conjunto de perguntas (v2): `questions.v2.json` (gerado de `drafts/new-questions.md` via `scripts/build_questions_v2_from_draft.py`)
- As configurações incluem `openai/gpt-5.2-codex` e `openai/gpt-5.3-codex` com variações de raciocínio (`low`, `high`, `xhigh`).
- As listas de modelos das configurações sêo alinhadas ao histórico de execuçêo de `data/model_metadata/tested_models_inventory.csv`, incluindo IDs legados da OpenAI (`openai/gpt-4.1`, `openai/gpt-4o*`, `openai/o3`).

## Estrutura do Repositório

- `scripts/openrouter_benchmark.py`: CLI principal (`collect`, `grade`, `grade-panel`, `aggregate`, `report`)
- `scripts/run_end_to_end.sh`: executor de pipeline em um comando
- `scripts/publish_latest_to_viewer.sh`: publicar saídas de execuçêo em um diretório de dataset do visualizador selecionado (`data/latest` ou `data/v2/latest`)
- `scripts/build_questions_v2_from_draft.py`: construir `questions.v2.json` a partir de rascunho em markdown
- `scripts/cleanup_generated_outputs.sh`: remover artefatos locais gerados
- `scripts/model_launch_pipeline.py`: pipeline de coleta/julgamento de datas de lançamento
- `viewer/index.html`: visualizador interativo canônico
- `viewer/index.v2.html`: visualizador interativo focado em v2
- `data/latest/*`: dataset v1 publicado do benchmark
- `data/v2/latest/*`: dataset v2 publicado do benchmark
- `runs/*`: histórico de execuçêo local

## Arquivos do Dataset Publicado

`data/latest` contém:

- `responses.jsonl`
- `collection_stats.json`
- `panel_summary.json`
- `aggregate_summary.json`
- `aggregate.jsonl`
- `leaderboard.csv`
- `leaderboard_with_launch.csv`
- `model_launch_dates.csv`
- `manifest.json`

Artefatos de execuçêo de coleta em `runs/<run_id>/` agora também incluem métricas de uso achatadas por linha em `responses.jsonl` e `responses_review.csv`:

- contagem de tokens (`response_prompt_tokens`, `response_completion_tokens`, `response_total_tokens`, `response_reasoning_tokens`)
- detalhes de cache (`response_cached_prompt_tokens`, `response_cache_write_tokens`)
- campos de custo (`response_cost_usd` e campos de detalhamento de custo upstream)
- métricas derivadas (`response_char_count`, `response_tokens_per_second`)

E `collection_stats.json` inclui `usage_summary` com totais/médias gerais e por modelo.

## Ambiente

Obrigatório:

- `OPENROUTER_API_KEY`

Opcional:

- `OPENROUTER_REFERER`
- `OPENROUTER_APP_NAME`
