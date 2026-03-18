# KERNO-SEIZE-INICIO.md
# Tutorial de Onboarding BMAD — Do zero ao primeiro prompt no Lovable

> **Para quem é este guia:** Vibe coder que nunca usou o BMAD Method e quer entender como usar os agentes de planejamento antes de abrir o Lovable.
>
> **O que você vai aprender:** Como instalar, qual agente invocar, em que ordem, com qual prompt, o que esperar de cada um e como entregar os resultados para o Lovable.
>
> **Tempo estimado:** 1h30–3h para um projeto típico (mais se você quiser ir fundo em cada etapa).

---

## Por que planejar antes de construir?

Quando você vai direto ao Lovable com uma ideia na cabeça, o Lovable decide sozinho:
- Como estruturar o banco de dados
- Quais funcionalidades entram no MVP
- Como a arquitetura vai crescer

Esse improviso gera retrabalho. O BMAD inverte essa lógica: você chega ao Lovable com documentos prontos, e ele implementa o que foi planejado — não reinventa.

```
Sem BMAD:  ideia → Lovable → retrabalho → retrabalho → retrabalho
Com BMAD:  ideia → planejamento (1-3h) → Lovable → produto organizado
Com PM Skills + BMAD:  ideia → validação → estratégia → planejamento → produto fundamentado
```

---

## Parte 0 — PM Skills + AI PM Copilot (ANTES do BMAD)

> **Novidade v5.5:** Antes de invocar os agentes BMAD, use o PM Skills Marketplace para validar a ideia, definir estratégia, articular value proposition, mapear concorrentes e definir métricas. Isso garante que Mary e John recebam contexto validado — não improvisem do zero.

### Por que PM Skills antes do BMAD?

O BMAD começa com Mary criando um product-brief — mas assume que você já sabe o que construir e para quem. O PM Skills preenche esse gap com discovery, estratégia, value proposition, análise competitiva e North Star Metric.

```
Sem PM Skills:  ideia vaga → Mary → product-brief genérico → PRD fraco
Com PM Skills:  ideia → validação → estratégia → métricas → Mary → PRD fundamentado
```

### Instalar PM Skills

```bash
# No Claude Code
claude plugin marketplace add phuryn/pm-skills

# Instalar todos os 8 plugins
claude plugin install pm-toolkit@pm-skills
claude plugin install pm-product-strategy@pm-skills
claude plugin install pm-product-discovery@pm-skills
claude plugin install pm-market-research@pm-skills
claude plugin install pm-data-analytics@pm-skills
claude plugin install pm-marketing-growth@pm-skills
claude plugin install pm-go-to-market@pm-skills
claude plugin install pm-execution@pm-skills
```

**Verificar:** `/discover test` — deve iniciar o workflow de discovery.

### Instalar AI PM Copilot (gates de qualidade)

```bash
/plugin marketplace add https://github.com/slgoodrich/agents
/plugin install ai-pm-copilot
```

> **Nota de licença:** AI PM Copilot usa PolyForm Noncommercial — uso comercial requer acordo com o autor. PM Skills usa MIT — sem restrição. Os gates do AI PM Copilot são recomendados mas podem ser substituídos por revisão manual.

### Sequência recomendada (Fase 1)

Execute os commands abaixo na ordem. Salve cada output em `/docs/pm-*.md`:

```
1. /discover [sua ideia]                    → /docs/pm-discovery-plan.md
2. /strategy [produto]                      → /docs/pm-strategy-canvas.md
3. /value-proposition [para quem]           → /docs/pm-value-proposition.md
4. /competitive-analysis [mercado]          → /docs/pm-competitive-analysis.md
5. /research-users [dados ou produto]       → alimenta Mary + Saga (não precisa salvar separado)
6. /north-star [produto + modelo]           → /docs/pm-north-star.md
```

**Tempo estimado:** ~2h para os 6 commands.

Depois: siga para a Parte 1 (Instalação BMAD) e Parte 2 (Agentes) com todo o contexto do PM Skills alimentando Mary e John.

### Gate 0 — Validation Sprint (só ideia vaga)

Se a ideia for genuinamente aberta ("quero empreender em legaltech mas não sei o quê"), rode o validation sprint ANTES do PM Skills:

```
/agent-teams:validation-sprint "Plataforma de [sua ideia]"
→ 3 agentes investigam em paralelo
→ Veredicto: BUILD / DON'T BUILD / NEEDS MORE EVIDENCE
```

Se BUILD → siga para PM Skills (Fase 1 acima).
Se NEEDS MORE EVIDENCE → ajuste a ideia e rode novamente.
Se DON'T BUILD → considere pivotar antes de investir mais tempo.

Salve o resultado em `/docs/pm-validation-verdict.md`.

### Gate 1 — PRD Stress Test (após John, antes de Winston)

Depois que John criar o PRD (Parte 3 deste guia), passe pelo stress test:

```
/agent-teams:prd-stress-test /docs/prd.md
→ 3 revisores atacam: market-fit, feasibility, scope
→ Veredicto: READY TO BUILD / NEEDS REVISION / MAJOR REWORK
```

Se READY TO BUILD → siga para Winston.
Se NEEDS REVISION → volte para John com o feedback.
Salve o resultado em `/docs/pm-prd-stress-test.md`.

---

## Parte 1 — Instalação

### Pré-requisitos

Você precisa ter instalado antes de continuar:

| Ferramenta | Como verificar | Como instalar |
|------------|---------------|---------------|
| Node.js (versão LTS) | `node --version` | [nodejs.org](https://nodejs.org) |
| Git | `git --version` | [git-scm.com](https://git-scm.com) |
| Claude Code | `claude --version` | Mac: `brew install claude-code` · Win: `winget install Anthropic.ClaudeCode` |
| Plano Claude Pro ou Max | — | [claude.ai/settings](https://claude.ai/settings) |

> **Claude Code requer plano pago.** Sem ele, você não consegue invocar os agentes BMAD. Alternativa gratuita: use os prompts da Parte 3 diretamente no Claude.ai — você perde a automação, mas os outputs são similares.

---

### Instalar o BMAD Method

Abra o terminal na **pasta do seu projeto** (ou crie uma pasta nova):

```bash
# Crie a pasta do projeto se ainda não existe
mkdir meu-projeto
cd meu-projeto

# Instale o BMAD Method (vai perguntar quais módulos incluir)
npx bmad-method install
```

Durante a instalação, você vai ver um menu de módulos. **Selecione:**

- ✅ **BMM** (BMAD Method core) — obrigatório: agentes Mary, John, Winston, Bob
- ✅ **TEA** (Test Architect) — recomendado: agente Murat
- ✅ **CIS** (Creative Innovation Suite) — recomendado: agentes Carson, Maya, Victor
- ✅ **WDS** (Whiteport Design Studio) — recomendado se produto B2C: agentes Saga, Freya, Idunn, Mimir

Após a instalação, verifique que a pasta `.bmad/` foi criada:

```bash
ls -la
# Você deve ver: .bmad/  docs/  (e outros arquivos)
```

---

### Criar a estrutura de docs

O BMAD vai salvar os documentos na pasta `/docs`. Crie-a se ainda não existir:

```bash
mkdir -p docs
```

---

## Parte 2 — Os agentes e o que cada um faz

### Visão geral: quem faz o quê

```
ORDEM DE INVOCAÇÃO (projeto B2C)

  CIS ──────────→ SAGA ──────────→ MARY ──────────→ JOHN
  (brainstorm)   (estratégia UX)  (product brief)   (PRD)
                 [Onda 1 WDS]                         ↓
                                               WINSTON ──→ FREYA + IDUNN
                                               (arquit.)  [Onda 2 WDS]
                                                     ↓
                                               BOB ──────→ MURAT
                                               (stories)   (QA Plan)
                                                     ↓
                                            → Lovable (implementar)


ORDEM DE INVOCAÇÃO (SaaS técnico)

  CIS ──────────→ MARY ──────────→ JOHN ──────────→ WINSTON
  (brainstorm)   (product brief)   (PRD)             (arquit.)
       ↓                                               ↓
  (opcional)                                    FREYA + IDUNN
                                                [WDS Onda 2, opcional]
                                                       ↓
                                               BOB → MURAT → Lovable
```

---

### Tabela de agentes

| Agente | Suite | Papel | Entra quando | Documenta |
|--------|-------|-------|-------------|-----------|
| **Carson** | CIS | Brainstorm Coach — gera ideias com SCAMPER e técnicas criativas | Ideia vaga, não sabe por onde começar | `brainstorming-report.md` |
| **Maya** | CIS | Design Thinking — entende o usuário antes de construir | Precisa validar para quem é o produto | (incorporado no brainstorming-report) |
| **Victor** | CIS | Innovation Strategist — analisa mercado e oportunidades | Quer saber se a ideia tem viabilidade | (incorporado no brainstorming-report) |
| **Saga** | WDS | Analista de Negócio — estratégia de produto, pesquisa de usuário, Trigger Map | Produto B2C, antes do PRD | `wds-product-brief.md`, `wds-trigger-map.md` |
| **Mary** | BMM | Analyst — visão do produto, personas, contexto de mercado | Após CIS ou Saga, antes de John | `product-brief.md` |
| **John** | BMM | Product Manager — PRD completo, milestones, features priorizadas | Após product-brief aprovado | `prd.md`, `prd-milestones.md`, `features.md` |
| **Winston** | BMM | Architect — stack, arquitetura, schema de banco, ADRs | Após PRD aprovado | `architecture.md`, `database.md`, `decisions.md` |
| **Freya** | WDS | Designer UX/UI — cenários de uso, specs de página, Design System | Produto B2C, após PRD e Arquitetura | `wds-scenarios.md`, `wds-design-system.md` |
| **Idunn** | WDS | Product Manager de Design — handoff design → desenvolvimento, UI Roadmap | Após Freya | `wds-ui-roadmap.md` |
| **Bob** | BMM | Scrum Master — user stories detalhadas com critérios de aceite | Após arquitetura (e Freya, se B2C) | stories detalhadas (incorporadas ou separadas) |
| **Murat** | TEA | Test Architect — estratégia de testes por risco (P0 a P3) | Após arquitetura, antes do Lovable | `qa-plan.md` |

> **Mimir** (WDS) não produz documentos — é o orquestrador que guia o processo WDS. Invoque Mimir primeiro quando for usar qualquer agente WDS e ele vai te dizer o que fazer.

---

## Parte 3 — Prompts de invocação

Para invocar um agente no Claude Code, use o formato:

```bash
# No terminal, dentro da pasta do projeto, com Claude Code ativo:
claude
# Depois, no prompt do Claude Code:
@<nome-do-agente> <instrução>

# Exemplo:
@mary Crie o product brief para o projeto descrito a seguir: [sua descrição]
```

Alternativamente, você pode iniciar pelo agente orquestrador:

```bash
@bmad-master Quero planejar um novo produto. Por onde começar?
```

---

### CIS — Carson, Maya, Victor

**Quando usar:** Sua ideia ainda é vaga. Você sabe que quer "um app para X" mas não sabe exatamente o quê, para quem ou por quê.

**Prompt para Carson (brainstorm):**
```
@carson Tenho uma ideia para [descreva em 1-2 frases]. 
Me ajude a explorar e expandir essa ideia usando técnicas criativas.
Quero entender: o problema real que resolve, para quem é, 
e quais variações da ideia valem considerar.
```

**Prompt para Maya (validação de usuário):**
```
@maya Baseado no brainstorming sobre [produto], 
me ajude a entender melhor o usuário.
Quem são as pessoas que vão usar isso? Qual dor real têm?
Como esse produto se encaixa no dia a dia delas?
```

**Prompt para Victor (análise de mercado):**
```
@victor Analise a viabilidade de [produto] no mercado atual.
Quais concorrentes existem? Qual é o diferencial?
Existe espaço para esse produto? Quais riscos?
```

**Output esperado:** Um `brainstorming-report.md` com exploração de ideias, personas iniciais, análise de mercado e recomendação de direção. Este documento alimenta o Mary (product brief).

---

### WDS Onda 1 — Saga (produto B2C)

**Quando usar:** Você tem produto B2C onde a experiência do usuário é o diferencial. Saga deve entrar **antes do John fechar o PRD** para que a pesquisa de usuário informe o produto — não decore o que já foi decidido.

**Passo 1 — Invocar Mimir primeiro:**
```
@wds-mimir Quero iniciar o processo WDS para [produto].
É um produto B2C voltado para [tipo de usuário].
Me oriente sobre por onde começar e como aproveitar melhor o WDS.
```

**Passo 2 — Product Exploration com Saga:**
```
@saga Fase 1 — Product Exploration.
Produto: [nome e descrição breve]
Contexto: [saída do CIS ou breve descrição da ideia]

Me ajude a definir a visão estratégica do produto:
- Qual problema central resolve?
- Quem são os usuários prioritários?
- Qual é o posicionamento no mercado?
- Quais são os critérios de sucesso?
```

**Passo 3 — Trigger Map com Saga:**
```
@saga Fase 2 — User Research e Trigger Map.
Com base no Product Brief que desenvolvemos:

Mapeie os gatilhos psicológicos e comportamentais que conectam
as metas de negócio com as motivações do usuário.
Para cada persona identificada: quais são seus gatilhos de ação,
barreiras de adoção e momentos de decisão?
```

**Output esperado:**
- `wds-product-brief.md` — visão, posicionamento, personas, critérios de sucesso
- `wds-trigger-map.md` — mapeamento de gatilhos psicológicos por persona

Esses documentos entram como input para Mary e John.

---

### BMM — Mary (Analyst)

**Quando usar:** Após CIS (e Saga, se B2C), antes de John. Mary formaliza a visão do produto em um document estruturado.

**Prompt:**
```
@mary Crie o Product Brief para o projeto abaixo.

[Se veio do CIS, cole aqui o resumo do brainstorming-report]
[Se veio do WDS, cole aqui o resumo do wds-product-brief]

Produto: [nome]
Visão: [o que é e para quem]
Problema que resolve: [descrição]
Usuários-alvo: [quem vai usar]
Funcionalidades principais desejadas: [lista]
Restrições ou requisitos não-negociáveis: [ex: precisa funcionar offline, LGPD, etc.]

Produza um product-brief.md completo seguindo o padrão BMAD.
Salve em /docs/product-brief.md
```

**Output esperado:** `docs/product-brief.md` com visão do produto, personas detalhadas, contexto de mercado, critérios de sucesso e restrições.

**Como saber se está bom:**
- O product brief descreve para quem é o produto em termos concretos (não "para todos")
- Tem pelo menos 2-3 personas com contexto real
- Lista restrições que o John precisa respeitar ao criar o PRD

---

### BMM — John (PM)

**Quando usar:** Após product-brief.md aprovado. John cria o PRD completo com milestones e features priorizadas.

**Prompt:**
```
@john Crie o PRD completo para o projeto.

Leia /docs/product-brief.md (já salvo) para entender a visão do produto.
[Se B2C com WDS, adicione:] Leia também /docs/wds-product-brief.md e /docs/wds-trigger-map.md para informar as features com a perspectiva do usuário.

Produza:
1. /docs/prd.md — requisitos funcionais e não-funcionais completos
2. /docs/prd-milestones.md — milestones seguindo a Árvore de Dependências:
   - Milestone 1: Fundação (auth + layout + estrutura)
   - Milestone 2: Dados (schema + RLS + functions)
   - Milestone 3: Features Core (CRUD principal)
   - Milestone 4+: Features secundárias, integrações, polish
3. /docs/features.md — lista de features priorizadas por valor/esforço

Cada milestone deve ter: features incluídas, arquivos afetados, dependências e critérios de aceite mensuráveis.
```

**Output esperado:**
- `docs/prd.md` — escopo completo, requisitos, user stories em nível macro, restrições técnicas
- `docs/prd-milestones.md` — 4+ milestones com tudo que o Lovable precisa saber
- `docs/features.md` — lista priorizada com tag MVP/futuro

**Como saber se está bom:**
- Os milestones seguem a ordem auth → layout → dados → features (nunca ao contrário)
- Cada feature tem critério de aceite objetivo ("o usuário consegue fazer X e ver Y")
- O PRD menciona o que **não** está no escopo (isso é tão importante quanto o que está)

---

### BMM — Winston (Architect)

**Quando usar:** Após PRD aprovado. Winston define a stack, arquitetura e schema antes do banco existir — schema é difícil de reverter no Supabase.

**Prompt:**
```
@winston Crie a documentação de arquitetura do projeto.

Leia /docs/prd.md para entender o escopo e as funcionalidades.
Stack preferida (ajuste se necessário):
- Frontend: React + TypeScript + Vite
- Estilização: Tailwind CSS + shadcn/ui
- Backend: Supabase (Auth, Database, RLS, Edge Functions)
- Deploy: Vercel
- CI/CD: GitHub Actions

Produza:
1. /docs/architecture.md — visão geral da arquitetura, padrões de código, estrutura de pastas
2. /docs/database.md — schema completo do banco com TODAS as tabelas, colunas, tipos, 
   relacionamentos e políticas RLS. Inclua os SQL de criação.
3. /docs/decisions.md — ADRs (Architecture Decision Records) para as 3-5 decisões 
   técnicas mais importantes

Atenção especial em database.md: o schema precisa estar correto desde o início.
RLS é obrigatório em todas as tabelas que contêm dados de usuário.
```

**Output esperado:**
- `docs/architecture.md` — stack, estrutura de pastas, padrões, fluxo de dados
- `docs/database.md` — schema completo com SQL, relacionamentos e políticas RLS
- `docs/decisions.md` — registro das principais decisões e por que foram tomadas

**Como saber se está bom:**
- `database.md` tem políticas RLS explícitas para cada tabela (não "a definir")
- `architecture.md` não contradiz o PRD (ex: se PRD diz multi-tenant, a arquitetura reflete isso)
- `decisions.md` documenta por que escolheu Supabase vs Firebase, por exemplo

---

### WDS Onda 2 — Freya + Idunn (produto B2C)

**Quando usar:** Produto B2C, após PRD e Arquitetura fechados. Freya especifica como o produto funciona e como parece. Idunn traduz isso para specs técnicas que o Lovable entende.

> O Design System visual (cores, tipografia, tokens) pode começar a qualquer momento, em paralelo com qualquer outra etapa.

**Prompt para Freya — Cenários UX:**
```
@freya Fase 4 — UX Design.
Leia /docs/prd.md e /docs/wds-product-brief.md.

Crie os cenários de uso detalhados para as features do Milestone 1 e 2:
- Para cada feature principal: descreva o fluxo completo do usuário
- Especifique cada página/tela: objetivo, elementos obrigatórios, hierarquia de conteúdo
- Identifique estados da interface: vazio, carregando, erro, sucesso
- Documente as interações críticas (o que acontece ao clicar em X?)

Salve em /docs/wds-scenarios.md
```

**Prompt para Freya — Design System:**
```
@freya Fase 5 — Visual Design.
Crie o Design System para [nome do produto].

Baseado no posicionamento do produto (leia /docs/wds-product-brief.md):
- Paleta de cores (primária, secundária, feedback, neutros) com hex codes
- Tipografia (fonte, tamanhos, pesos, line-heights)
- Espaçamento (escala de 4px/8px)
- Componentes base: botões, inputs, cards, badges, navegação
- Tokens de design (variáveis CSS)

Salve em /docs/wds-design-system.md
```

**Prompt para Idunn — UI Roadmap:**
```
@idunn Fase 6 — Dev Integration.
Leia /docs/wds-scenarios.md, /docs/wds-design-system.md e /docs/prd-milestones.md.

Crie o UI Roadmap de handoff para o Lovable:
- Para cada milestone: lista de componentes a implementar com specs precisas
- Para cada componente: dimensões, cores (usando tokens do design system), comportamentos, variantes
- Especificações de responsividade (mobile-first)
- Fluxos de navegação entre páginas

O Lovable vai usar este documento para implementar sem inventar.
Salve em /docs/wds-ui-roadmap.md
```

**Output esperado:**
- `docs/wds-scenarios.md` — cenários de uso e specs de página
- `docs/wds-design-system.md` — sistema visual completo com tokens
- `docs/wds-ui-roadmap.md` — handoff técnico de design para desenvolvimento

---

### BMM — Bob (Scrum Master)

**Quando usar:** Após Winston (e Freya/Idunn se B2C). Bob detalha as features do John em user stories granulares com critérios de aceite que o Lovable vai usar como checklist.

**Prompt:**
```
@bob Crie as user stories detalhadas para o projeto.

Leia:
- /docs/prd.md (escopo e features)
- /docs/prd-milestones.md (o que entra em cada milestone)
- /docs/architecture.md (restrições técnicas)
[Se B2C:] - /docs/wds-scenarios.md (como cada feature funciona)

Para cada feature da Milestone 1 e 2, escreva user stories no formato:
"Como [persona], quero [ação] para [benefício]"

Cada story deve ter:
- Critérios de aceite objetivos e testáveis (formato Dado/Quando/Então)
- Considerações técnicas relevantes
- Dependências com outras stories
- Estimativa de complexidade (P: pequena, M: média, G: grande)

Agrupe por milestone. Salve no /docs/prd.md (seção de stories) 
ou crie /docs/stories.md separado se preferir.
```

**Output esperado:** User stories granulares para Milestone 1 e 2, com critérios de aceite no formato BDD (Dado/Quando/Então) e estimativas de complexidade.

---

### TEA — Murat (Test Architect)

**Quando usar:** Após Winston. Antes de abrir o Lovable. Esta é a **1ª intervenção** de Murat — ele vai intervir de novo na Etapa 5 (Cursor Review) para validar se o código cobriu o plano.

**Prompt:**
```
@murat Crie a estratégia de testes para o projeto.

Leia:
- /docs/prd.md (funcionalidades)
- /docs/architecture.md (stack e padrões)
- /docs/database.md (schema e RLS)

Produza /docs/qa-plan.md com:
1. Classificação de risco por funcionalidade (P0 a P3):
   - P0: falha causa perda de dados ou acesso não autorizado
   - P1: falha bloqueia funcionalidade core
   - P2: falha degrada experiência mas tem workaround
   - P3: falha é cosmética ou edge case

2. Para cada funcionalidade P0 e P1:
   - Casos de teste manuais obrigatórios
   - Estratégia de teste automatizado recomendada

3. Checklist de segurança específico para este projeto:
   - RLS: quais tabelas têm dados sensíveis?
   - Autenticação: quais rotas precisam estar protegidas?
   - Inputs: quais formulários têm campos críticos?

4. Critério de "pronto para deploy":
   - 100% dos P0 testados e passando
   - 80%+ dos P1 testados e passando
   - Zero vulnerabilidades críticas no SAST
```

**Output esperado:** `docs/qa-plan.md` com classificação P0-P3 de todas as funcionalidades, casos de teste, e critério objetivo de "pronto para deploy".

**Como saber se está bom:**
- Todo P0 tem pelo menos um caso de teste manual descrito
- O checklist de segurança menciona tabelas específicas do `database.md`
- O critério de "pronto para deploy" é objetivo (não "quando eu achar que está bom")

---

## Parte 4 — O que fazer com os documentos prontos

Quando você tiver os documentos do BMAD, o próximo passo é configurar o Lovable. Siga a **Etapa 02 do KERNO-GUIA-PASSO-A-PASSO.html**, com as seguintes adaptações:

### Documentos para fazer upload no Lovable

**Obrigatórios (gerados pelo BMAD):**
- `prd.md`
- `prd-milestones.md`
- `architecture.md`
- `database.md`
- `features.md`

**Se usou WDS (Onda 2):**
- `wds-design-system.md`
- `wds-ui-roadmap.md`

**Do framework Kerno (obrigatório):**
- `KERNO-DOCS.md` (define os 23 documentos em 4 grupos)
- `SECURITY.md` (regras OWASP do Kerno)

### Prompt Seção B (já está correto na Etapa 02 do Guia)

O prompt da Seção B do V3 instrui o Lovable a:
1. Ler e validar os docs BMAD que você fez upload
2. Criar apenas os docs operacionais que faltam (session-close, changelog, etc.)
3. Apresentar o plano da Milestone 1 para aprovação

**Ele NÃO vai recriar o que você planejou.** Esse é o ponto central da integração.

---

## Parte 5 — Referência rápida de documentos

### O que cada documento contém

| Documento | Criado por | Contém |
|-----------|-----------|--------|
| `brainstorming-report.md` | CIS | Exploração de ideias, validação de usuário, análise de mercado |
| `wds-product-brief.md` | Saga | Visão estratégica, posicionamento, personas, critérios de sucesso |
| `wds-trigger-map.md` | Saga | Gatilhos psicológicos por persona, barreiras de adoção |
| `product-brief.md` | Mary | Visão formal do produto, personas detalhadas, restrições |
| `prd.md` | John | Requisitos funcionais e não-funcionais, user stories em nível macro |
| `prd-milestones.md` | John | 4+ milestones com features, dependências e critérios de aceite |
| `features.md` | John/Bob | Lista priorizada MVP vs futuro |
| `architecture.md` | Winston | Stack, estrutura de pastas, padrões de código, fluxo de dados |
| `database.md` | Winston | Schema completo com SQL, relacionamentos, políticas RLS |
| `decisions.md` | Winston | ADRs — decisões técnicas e justificativas |
| `wds-scenarios.md` | Freya | Cenários de uso, specs de página, estados de interface |
| `wds-design-system.md` | Freya | Paleta, tipografia, espaçamento, componentes, tokens CSS |
| `wds-ui-roadmap.md` | Idunn | Handoff design → Lovable: specs precisas por milestone |
| `qa-plan.md` | Murat | Classificação P0-P3, casos de teste, critério de pronto para deploy |

### Documentos criados pelo Lovable (operacionais)

Esses são criados pelo prompt da Seção B do V3 — você não precisa criar:

| Documento | Contém |
|-----------|--------|
| `ui-components.md` | Componentes criados e suas props |
| `flows.md` | Fluxos de navegação implementados |
| `changelog.md` | Histórico de versões |
| `handover.md` | Estado do projeto para continuação |
| `security-rules.md` | Regras de segurança aplicadas |
| `session-close.md` | Estado da última sessão (copiar ao retomar) |
| `delivery-report.md` | Escopo planejado vs entregue por milestone |
| `security-report.md` | Resultado da revisão de segurança |

---

## Parte 6 — Dúvidas frequentes

**Preciso usar todos os agentes?**
Não. Para um MVP simples: Mary → John → Winston → Murat → Lovable é suficiente. CIS só se a ideia for vaga. WDS só se o produto for B2C onde a UX é diferencial. Bob só se você quiser stories muito granulares.

**Posso usar sem Claude Code (só Claude.ai)?**
Sim. Cole os prompts da Parte 3 diretamente no Claude.ai e salve os outputs manualmente em arquivos `.md`. Você perde a automação do `@agente`, mas os documentos gerados são equivalentes.

**Em que ordem devo salvar os arquivos?**
Na pasta `/docs` do seu projeto (na mesma pasta onde vai ficar o código). Quando conectar ao Lovable, você vai fazer upload desses arquivos para o Knowledge do projeto.

**O Lovable vai respeitar os documentos?**
Sim, se você:
1. Colocou o KERNO-KNOWLEDGE.md no Custom Knowledge
2. Fez upload dos docs antes de mandar qualquer prompt
3. Usou o prompt correto da Seção B do V3 (que está no KERNO-GUIA-PASSO-A-PASSO.html)

**Posso pular o Winston e deixar o schema para o Lovable criar?**
Tecnicamente sim, mas não é recomendado. O Supabase é difícil de modificar depois (RLS, relacionamentos, políticas). Se o Lovable criar um schema sem planejamento, você vai gastar tempo corrigindo. Meia hora com o Winston economiza horas de refatoração.

**Quanto tempo demora tudo isso?**
- CIS (se ideia for vaga): 20-30 min
- Saga (WDS Onda 1, se B2C): 30-45 min  
- Mary: 15-20 min
- John: 20-30 min
- Winston: 20-30 min
- Freya + Idunn (WDS Onda 2, se B2C): 30-60 min
- Bob: 15-25 min
- Murat: 15-20 min
- **Total mínimo** (sem WDS): ~1h30 · **Total com WDS**: ~3h

---

## Parte 7 — Checklist de entrega

Antes de abrir o Lovable, confirme:

### Docs BMAD obrigatórios
- [ ] `product-brief.md` — visão clara do produto e personas definidas
- [ ] `prd.md` — requisitos e user stories macro
- [ ] `prd-milestones.md` — 4+ milestones na ordem correta (auth → dados → features)
- [ ] `architecture.md` — stack definida, estrutura de pastas
- [ ] `database.md` — schema com RLS em todas as tabelas de usuário
- [ ] `features.md` — lista priorizada MVP vs futuro
- [ ] `qa-plan.md` — P0 e P1 classificados com critérios de aceite

### Docs WDS (se produto B2C)
- [ ] `wds-product-brief.md` — visão estratégica e personas UX
- [ ] `wds-trigger-map.md` — gatilhos e barreiras do usuário
- [ ] `wds-scenarios.md` — cenários de uso e specs de página
- [ ] `wds-design-system.md` — paleta, tipografia, componentes
- [ ] `wds-ui-roadmap.md` — handoff técnico para o Lovable

### Configuração Lovable
- [ ] Seção A do LOVABLE-SYSTEM-PROMPT-V3.md colada no Custom Knowledge
- [ ] Upload de todos os docs BMAD + KERNO-DOCS.md + SECURITY.md
- [ ] GitHub repo criado e conectado (com pipeline SAST)
- [ ] Prompt da Seção B do V3 pronto para enviar

---

## Referências

- [KERNO-GUIA-PASSO-A-PASSO.html](KERNO-GUIA-PASSO-A-PASSO.html) — guia completo do fluxo Lovable
- [KERNO-INICIO.html](KERNO-INICIO.html) — fluxograma interativo "por onde começar"
- [KERNO-ROADMAP.html](KERNO-ROADMAP.html) — visão geral das 6 etapas do framework
- [KERNO-SEIZE-MAPEAMENTO.html](KERNO-SEIZE-MAPEAMENTO.html) — mapeamento completo BMAD × Kerno
- [LOVABLE-SYSTEM-PROMPT-V3.md](LOVABLE-SYSTEM-PROMPT-V3.md) — system prompt completo (Seções A-G)
- [KERNO-DOCS.md](KERNO-DOCS.md) — os 23 documentos em 4 grupos
- [SECURITY.md](SECURITY.md) — regras OWASP do framework

---

*Kerno Seize v6.0.3 · Março 2026*
