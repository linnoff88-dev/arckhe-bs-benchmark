<h1>
  <img src="docs/images/bsbench.png" alt="Logo do BullshitBench" width="64" />
  BullshitBench v2
</h1>

O BullshitBench mede se os modelos de inteligÃªncia artificial detectam absurdos, os apontam claramente e evitam continuar com confianÃ§a em premissas invÃ¡lidas.

- Visualizador pÃºblico (mais recente): https://inematds.github.io/bs-benchmark/viewer/index.v2.html
- Atualizado em: 2026-03-04

## O Que Ã© "Absurdo" Neste Contexto?

Quando falamos de "absurdo" (ou "bullshit") neste benchmark, estamos nos referindo a **perguntas que parecem inteligentes e tÃ©cnicas, mas que na verdade nÃ£o fazem sentido algum**. SÃ£o frases construÃ­das com jargÃ£o real de Ã¡reas como medicina, direito, finanÃ§as, fÃ­sica e software, mas que combinam esses termos de forma que nÃ£o tÃªm significado real.

**Exemplos do que consideramos "absurdo":**

- Perguntar sobre a "impedÃ¢ncia em ohms entre obrigaÃ§Ãµes regulatÃ³rias da SEC e frameworks de ESG" â€” impedÃ¢ncia Ã© um conceito de engenharia elÃ©trica, nÃ£o tem nada a ver com regulamentaÃ§Ã£o financeira.
- Perguntar qual Ã© a "vida Ãºtil em graus-dia de uma clÃ¡usula de forÃ§a maior" â€” clÃ¡usulas contratuais nÃ£o sÃ£o materiais perecÃ­veis que estragam com o tempo.
- Perguntar sobre um "framework de decomposiÃ§Ã£o diferencial de indenizaÃ§Ã£o" â€” isso nÃ£o existe, mas soa como algo que poderia existir para quem nÃ£o Ã© especialista.

**Por que isso importa?** Se vocÃª pede ajuda a uma IA e ela aceita premissas absurdas sem questionar, ela pode te dar respostas completamente erradas com total confianÃ§a. Uma IA confiÃ¡vel deveria dizer: *"Espera, isso nÃ£o faz sentido"* â€” em vez de elaborar uma resposta detalhada sobre algo que nÃ£o existe.

**Como funciona o teste:** Fazemos 100 perguntas absurdas para cada IA e avaliamos se ela (1) rejeitou claramente o absurdo, (2) desconfiou mas tentou responder mesmo assim, ou (3) aceitou tudo como verdade e respondeu normalmente.

## Ãšltima Entrada do Changelog (2026-03-04)

- Adicionadas execuÃ§Ãµes de benchmark para:
  - `openai/gpt-5.3-chat`
  - `google/gemini-3.1-flash-lite-preview`
- Definida data de lanÃ§amento de metadados para ambos modelos como `2026-03-04` (hoje) e propagada pelos metadados de lanÃ§amento v1/v2 e visualizaÃ§Ãµes do leaderboard.
- Atualizado `viewer/index.v2.html` para carregar metadados de lanÃ§amento mesclando linhas embutidas + CSV e buscar com `cache: "no-store"` para que as datas de lanÃ§amento mais recentes sempre apareÃ§am nos grÃ¡ficos.
- Detalhes completos: [CHANGELOG.md](CHANGELOG.md)

## Destaques do Changelog v2

- `100` novas perguntas absurdas no conjunto v2.
- Cobertura de perguntas por domÃ­nio em `5` domÃ­nios: `software` (40), `finanÃ§as` (15), `jurÃ­dico` (15), `medicina` (15), `fÃ­sica` (15).
- Novas visualizaÃ§Ãµes no viewer v2, incluindo:
  - Taxa de DetecÃ§Ã£o por Modelo (barras empilhadas)
  - Panorama por DomÃ­nio (detecÃ§Ã£o geral vs por domÃ­nio)
  - Taxa de DetecÃ§Ã£o ao Longo do Tempo
  - Modelos Mais Novos TÃªm Desempenho Melhor?
  - Pensar Mais Ajuda? (alternÃ¢ncia tokens/custo)

## Guia do Visualizador (v2)

As capturas de tela abaixo seguem o mesmo fluxo de `viewer/index.v2.html`, comeÃ§ando pelo grÃ¡fico principal.

### 1. Taxa de DetecÃ§Ã£o por Modelo (GrÃ¡fico Principal)

VisÃ£o principal estilo leaderboard mostrando a distribuiÃ§Ã£o verde/Ã¢mbar/vermelho de cada modelo.

![BullshitBench v2 - Taxa de DetecÃ§Ã£o por Modelo](docs/images/v2-detection-rate-by-model.png)

### 2. Panorama por DomÃ­nio

DistribuiÃ§Ã£o de detecÃ§Ã£o por domÃ­nio para comparar desempenho geral vs cada domÃ­nio de relance.

![BullshitBench v2 - Panorama por DomÃ­nio](docs/images/v2-domain-landscape.png)

### 3. Taxa de DetecÃ§Ã£o ao Longo do Tempo

VisÃ£o de tendÃªncia por data de lanÃ§amento focada em Anthropic, OpenAI e Google.

![BullshitBench v2 - Taxa de DetecÃ§Ã£o ao Longo do Tempo](docs/images/v2-detection-rate-over-time.png)

### 4. Modelos Mais Novos TÃªm Desempenho Melhor?

DispersÃ£o de todos os modelos por data de lanÃ§amento vs taxa verde.

![BullshitBench v2 - Modelos Mais Novos TÃªm Desempenho Melhor](docs/images/v2-do-newer-models-perform-better.png)

### 5. Pensar Mais Ajuda?

DispersÃ£o de raciocÃ­nio (alternÃ¢ncia tokens/custo no viewer) vs taxa verde.

![BullshitBench v2 - Pensar Mais Ajuda](docs/images/v2-does-thinking-harder-help.png)

## Escopo do Benchmark (v2)

- `100` prompts absurdos no total.
- `5` grupos de domÃ­nio: `software` (40), `finanÃ§as` (15), `jurÃ­dico` (15), `medicina` (15), `fÃ­sica` (15).
- `13` tÃ©cnicas de absurdo (por exemplo: `plausible_nonexistent_framework`, `misapplied_mechanism`, `nested_nonsense`, `specificity_trap`).
- AgregaÃ§Ã£o de painel com `3` juÃ­zes (`anthropic/claude-sonnet-4.6`, `openai/gpt-5.2`, `google/gemini-3.1-pro-preview`) usando modo `full` do painel + agregaÃ§Ã£o `mean`.
- O leaderboard v2 publicado atualmente inclui `72` linhas de modelo/raciocÃ­nio.

## O Que Isso Mede

- `RejeiÃ§Ã£o Clara`: o modelo rejeita claramente a premissa invÃ¡lida.
- `Desafio Parcial`: o modelo aponta problemas mas ainda segue a premissa invÃ¡lida.
- `Absurdo Aceito`: o modelo trata o absurdo como vÃ¡lido.

## InÃ­cio RÃ¡pido

1. Configure sua chave de API:

```bash
export OPENROUTER_API_KEY=sua_chave_aqui
```

2. Execute a coleta + juiz primÃ¡rio (Claude por padrÃ£o):

```bash
./scripts/run_end_to_end.sh
```

3. Execute v2 de ponta a ponta e publique no dataset v2 dedicado:

```bash
./scripts/run_end_to_end.sh --config config.v2.json --viewer-output-dir data/v2/latest --with-additional-judges
```

4. Opcionalmente execute a configuraÃ§Ã£o padrÃ£o de ponta a ponta (publica em `data/latest`):

```bash
./scripts/run_end_to_end.sh --with-additional-judges
```

5. Abra o visualizador:

- Visualizador publicado (mais recente): https://inematds.github.io/bs-benchmark/viewer/index.v2.html
- Visualizador local (opcional):

```bash
./scripts/run_end_to_end.sh --with-additional-judges --serve --port 8877
```

Depois abra `http://localhost:8877/viewer/index.v2.html`.
Use o dropdown `VersÃ£o do Benchmark` no painel de filtros para alternar entre os datasets publicados (por exemplo `v1` e `v2`).

## v1 para v2

- O dataset v1 permanece em `data/latest`.
- O dataset v2 Ã© publicado em `data/v2/latest`.
- O conjunto de perguntas v2 vem de `drafts/new-questions.md` via `scripts/build_questions_v2_from_draft.py`.
- O julgamento canÃ´nico agora Ã© fixo em exatamente 3 juÃ­zes em cada linha com agregaÃ§Ã£o por mÃ©dia (o modo legado de desempate por discordÃ¢ncia foi removido do pipeline principal).
- Notas de lanÃ§amento e mudanÃ§as notÃ¡veis sÃ£o rastreadas em `CHANGELOG.md`.

## DocumentaÃ§Ã£o

- [Guia TÃ©cnico](docs/TECHNICAL.md): operaÃ§Ãµes do pipeline, publicaÃ§Ã£o de artefatos, fluxo de metadados de data de lanÃ§amento, estrutura do repositÃ³rio, variÃ¡veis de ambiente.
- [Changelog](CHANGELOG.md): notas de lanÃ§amento v1 para v2 e destaques do histÃ³rico de publicaÃ§Ã£o.
- [Conjunto de Perguntas](questions.json): perguntas do benchmark e metadados de pontuaÃ§Ã£o.
- [Conjunto de Perguntas v2](questions.v2.json): conjunto de perguntas v2 gerado a partir de `drafts/new-questions.md`.
- [ConfiguraÃ§Ã£o](config.json): configuraÃ§Ãµes padrÃ£o de modelo/pipeline.
- [ConfiguraÃ§Ã£o v2](config.v2.json): configuraÃ§Ã£o pronta para v2 (usa `questions.v2.json`).

## ObservaÃ§Ãµes

- Este README Ã© intencionalmente voltado ao pÃºblico.
- ConteÃºdo tÃ©cnico e voltado para mantenedores estÃ¡ em `docs/TECHNICAL.md`.

## HistÃ³rico de Estrelas

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=inematds/bs-benchmark&type=Date&theme=dark&cachebust=20261118" />
  <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=inematds/bs-benchmark&type=Date&cachebust=20261118" />
  <img alt="GrÃ¡fico de HistÃ³rico de Estrelas" src="https://api.star-history.com/svg?repos=inematds/bs-benchmark&type=Date&cachebust=20261118" />
</picture>

---

Projeto adaptado da comunidade INEMA TDS - Marca Arckhe