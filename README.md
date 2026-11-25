ï»¿<h1>
  <img src="docs/images/bsbench.png" alt="Logo do BullshitBench" width="64" />
  BullshitBench v2
</h1>

O BullshitBench mede se os modelos de inteligêÂªncia artificial detectam absurdos, os apontam claramente e evitam continuar com confianêÂ§a em premissas invêÂ¡lidas.

- Visualizador pêÂºblico (mais recente): https://linnoff88-dev.github.io/
- Atualizado em: 2026-03-04

## O Que êÂ© "Absurdo" Neste Contexto?

Quando falamos de "absurdo" (ou "bullshit") neste benchmark, estamos nos referindo a **perguntas que parecem inteligentes e têÂ©cnicas, mas que na verdade nêÂ£o fazem sentido algum**. SêÂ£o frases construêÂ­das com jargêÂ£o real de êÂ¡reas como medicina, direito, finanêÂ§as, fêÂ­sica e software, mas que combinam esses termos de forma que nêÂ£o têÂªm significado real.

**Exemplos do que consideramos "absurdo":**

- Perguntar sobre a "impedêÂ¢ncia em ohms entre obrigaêÂ§êÂµes regulatêÂ³rias da SEC e frameworks de ESG" ê¢â¬â impedêÂ¢ncia êÂ© um conceito de engenharia elêÂ©trica, nêÂ£o tem nada a ver com regulamentaêÂ§êÂ£o financeira.
- Perguntar qual êÂ© a "vida êÂºtil em graus-dia de uma clêÂ¡usula de forêÂ§a maior" ê¢â¬â clêÂ¡usulas contratuais nêÂ£o sêÂ£o materiais perecêÂ­veis que estragam com o tempo.
- Perguntar sobre um "framework de decomposiêÂ§êÂ£o diferencial de indenizaêÂ§êÂ£o" ê¢â¬â isso nêÂ£o existe, mas soa como algo que poderia existir para quem nêÂ£o êÂ© especialista.

**Por que isso importa?** Se vocêÂª pede ajuda a uma IA e ela aceita premissas absurdas sem questionar, ela pode te dar respostas completamente erradas com total confianêÂ§a. Uma IA confiêÂ¡vel deveria dizer: *"Espera, isso nêÂ£o faz sentido"* ê¢â¬â em vez de elaborar uma resposta detalhada sobre algo que nêÂ£o existe.

**Como funciona o teste:** Fazemos 100 perguntas absurdas para cada IA e avaliamos se ela (1) rejeitou claramente o absurdo, (2) desconfiou mas tentou responder mesmo assim, ou (3) aceitou tudo como verdade e respondeu normalmente.

## êÅ¡ltima Entrada do Changelog (2026-03-04)

- Adicionadas execuêÂ§êÂµes de benchmark para:
  - `openai/gpt-5.3-chat`
  - `google/gemini-3.1-flash-lite-preview`
- Definida data de lanêÂ§amento de metadados para ambos modelos como `2026-03-04` (hoje) e propagada pelos metadados de lanêÂ§amento v1/v2 e visualizaêÂ§êÂµes do leaderboard.
- Atualizado `viewer/index.v2.html` para carregar metadados de lanêÂ§amento mesclando linhas embutidas + CSV e buscar com `cache: "no-store"` para que as datas de lanêÂ§amento mais recentes sempre apareêÂ§am nos grêÂ¡ficos.
- Detalhes completos: [CHANGELOG.md](CHANGELOG.md)

## Destaques do Changelog v2

- `100` novas perguntas absurdas no conjunto v2.
- Cobertura de perguntas por domêÂ­nio em `5` domêÂ­nios: `software` (40), `finanêÂ§as` (15), `jurêÂ­dico` (15), `medicina` (15), `fêÂ­sica` (15).
- Novas visualizaêÂ§êÂµes no viewer v2, incluindo:
  - Taxa de DetecêÂ§êÂ£o por Modelo (barras empilhadas)
  - Panorama por DomêÂ­nio (detecêÂ§êÂ£o geral vs por domêÂ­nio)
  - Taxa de DetecêÂ§êÂ£o ao Longo do Tempo
  - Modelos Mais Novos TêÂªm Desempenho Melhor?
  - Pensar Mais Ajuda? (alternêÂ¢ncia tokens/custo)

## Guia do Visualizador (v2)

As capturas de tela abaixo seguem o mesmo fluxo de `viewer/index.v2.html`, comeêÂ§ando pelo grêÂ¡fico principal.

### 1. Taxa de DetecêÂ§êÂ£o por Modelo (GrêÂ¡fico Principal)

VisêÂ£o principal estilo leaderboard mostrando a distribuiêÂ§êÂ£o verde/êÂ¢mbar/vermelho de cada modelo.

![BullshitBench v2 - Taxa de DetecêÂ§êÂ£o por Modelo](docs/images/v2-detection-rate-by-model.png)

### 2. Panorama por DomêÂ­nio

DistribuiêÂ§êÂ£o de detecêÂ§êÂ£o por domêÂ­nio para comparar desempenho geral vs cada domêÂ­nio de relance.

![BullshitBench v2 - Panorama por DomêÂ­nio](docs/images/v2-domain-landscape.png)

### 3. Taxa de DetecêÂ§êÂ£o ao Longo do Tempo

VisêÂ£o de tendêÂªncia por data de lanêÂ§amento focada em Anthropic, OpenAI e Google.

![BullshitBench v2 - Taxa de DetecêÂ§êÂ£o ao Longo do Tempo](docs/images/v2-detection-rate-over-time.png)

### 4. Modelos Mais Novos TêÂªm Desempenho Melhor?

DispersêÂ£o de todos os modelos por data de lanêÂ§amento vs taxa verde.

![BullshitBench v2 - Modelos Mais Novos TêÂªm Desempenho Melhor](docs/images/v2-do-newer-models-perform-better.png)

### 5. Pensar Mais Ajuda?

DispersêÂ£o de raciocêÂ­nio (alternêÂ¢ncia tokens/custo no viewer) vs taxa verde.

![BullshitBench v2 - Pensar Mais Ajuda](docs/images/v2-does-thinking-harder-help.png)

## Escopo do Benchmark (v2)

- `100` prompts absurdos no total.
- `5` grupos de domêÂ­nio: `software` (40), `finanêÂ§as` (15), `jurêÂ­dico` (15), `medicina` (15), `fêÂ­sica` (15).
- `13` têÂ©cnicas de absurdo (por exemplo: `plausible_nonexistent_framework`, `misapplied_mechanism`, `nested_nonsense`, `specificity_trap`).
- AgregaêÂ§êÂ£o de painel com `3` juêÂ­zes (`anthropic/claude-sonnet-4.6`, `openai/gpt-5.2`, `google/gemini-3.1-pro-preview`) usando modo `full` do painel + agregaêÂ§êÂ£o `mean`.
- O leaderboard v2 publicado atualmente inclui `72` linhas de modelo/raciocêÂ­nio.

## O Que Isso Mede

- `RejeiêÂ§êÂ£o Clara`: o modelo rejeita claramente a premissa invêÂ¡lida.
- `Desafio Parcial`: o modelo aponta problemas mas ainda segue a premissa invêÂ¡lida.
- `Absurdo Aceito`: o modelo trata o absurdo como vêÂ¡lido.

## InêÂ­cio RêÂ¡pido

1. Configure sua chave de API:

```bash
export OPENROUTER_API_KEY=sua_chave_aqui
```

2. Execute a coleta + juiz primêÂ¡rio (Claude por padrêÂ£o):

```bash
./scripts/run_end_to_end.sh
```

3. Execute v2 de ponta a ponta e publique no dataset v2 dedicado:

```bash
./scripts/run_end_to_end.sh --config config.v2.json --viewer-output-dir data/v2/latest --with-additional-judges
```

4. Opcionalmente execute a configuraêÂ§êÂ£o padrêÂ£o de ponta a ponta (publica em `data/latest`):

```bash
./scripts/run_end_to_end.sh --with-additional-judges
```

5. Abra o visualizador:

- Visualizador publicado (mais recente): https://linnoff88-dev.github.io/
- Visualizador local (opcional):

```bash
./scripts/run_end_to_end.sh --with-additional-judges --serve --port 8877
```

Depois abra `http://localhost:8877/viewer/index.v2.html`.
Use o dropdown `VersêÂ£o do Benchmark` no painel de filtros para alternar entre os datasets publicados (por exemplo `v1` e `v2`).

## v1 para v2

- O dataset v1 permanece em `data/latest`.
- O dataset v2 êÂ© publicado em `data/v2/latest`.
- O conjunto de perguntas v2 vem de `drafts/new-questions.md` via `scripts/build_questions_v2_from_draft.py`.
- O julgamento canêÂ´nico agora êÂ© fixo em exatamente 3 juêÂ­zes em cada linha com agregaêÂ§êÂ£o por mêÂ©dia (o modo legado de desempate por discordêÂ¢ncia foi removido do pipeline principal).
- Notas de lanêÂ§amento e mudanêÂ§as notêÂ¡veis sêÂ£o rastreadas em `CHANGELOG.md`.

## DocumentaêÂ§êÂ£o

- [Guia TêÂ©cnico](docs/TECHNICAL.md): operaêÂ§êÂµes do pipeline, publicaêÂ§êÂ£o de artefatos, fluxo de metadados de data de lanêÂ§amento, estrutura do repositêÂ³rio, variêÂ¡veis de ambiente.
- [Changelog](CHANGELOG.md): notas de lanêÂ§amento v1 para v2 e destaques do histêÂ³rico de publicaêÂ§êÂ£o.
- [Conjunto de Perguntas](questions.json): perguntas do benchmark e metadados de pontuaêÂ§êÂ£o.
- [Conjunto de Perguntas v2](questions.v2.json): conjunto de perguntas v2 gerado a partir de `drafts/new-questions.md`.
- [ConfiguraêÂ§êÂ£o](config.json): configuraêÂ§êÂµes padrêÂ£o de modelo/pipeline.
- [ConfiguraêÂ§êÂ£o v2](config.v2.json): configuraêÂ§êÂ£o pronta para v2 (usa `questions.v2.json`).

## ObservaêÂ§êÂµes

- Este README êÂ© intencionalmente voltado ao pêÂºblico.
- ConteêÂºdo têÂ©cnico e voltado para mantenedores estêÂ¡ em `docs/TECHNICAL.md`.

## HistêÂ³rico de Estrelas

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=linnoff88-dev/bs-benchmark&type=Date&theme=dark&cachebust=20261118" />
  <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=linnoff88-dev/bs-benchmark&type=Date&cachebust=20261118" />
  <img alt="GrêÂ¡fico de HistêÂ³rico de Estrelas" src="https://api.star-history.com/svg?repos=linnoff88-dev/bs-benchmark&type=Date&cachebust=20261118" />
</picture>

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>