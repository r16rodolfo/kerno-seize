# KERNO-PM-INTEGRACAO.md
# Integração PM Skills Marketplace + AI PM Copilot ao Kerno Seize

> **Versão:** 1.0 (Kerno Seize v6.1.0)
> **O que muda:** O framework ganha duas novas camadas de product management que atuam ANTES e EM PARALELO ao BMAD. O CIS (Carson/Maya/Victor) deixa de ser o ponto de entrada de ideação.
>
> **Ferramentas integradas:**
> - [PM Skills Marketplace](https://github.com/phuryn/pm-skills) — 65 skills, 36 workflows, 8 plugins. Licença MIT.
> - [AI PM Copilot](https://github.com/slgoodrich/agents) — 8 agentes PM, 3 agent teams. Licença PolyForm Noncommercial.
>
> **Impacto na licença:** O AI PM Copilot usa PolyForm Noncommercial — uso comercial requer acordo separado com o autor. O PM Skills usa MIT — sem restrição. O Kerno deve documentar essa diferença claramente.

---

## 1. O que cada ferramenta adiciona

### PM Skills Marketplace (phuryn/pm-skills)

Cobre o ciclo completo de product management com frameworks consagrados (Teresa Torres, Marty Cagan, Alberto Savoia, Dan Olsen, April Dunford). O que NÃO existia no Kerno Seize:

| Capacidade | Command/Skill | Substitui no BMAD |
|-----------|--------------|-------------------|
| Discovery completo (brainstorm → assumptions → experiments) | `/discover` | CIS (Carson/Maya/Victor) — mais profundo |
| Product Strategy Canvas 9 seções | `/strategy` | Nenhum equivalente |
| Value Proposition JTBD 6 partes | `/value-proposition` | Nenhum equivalente |
| Competitive analysis com 5+ concorrentes | `/competitive-analysis` | Nenhum equivalente |
| User research (personas + segmentação + journey) | `/research-users` | Parcialmente Mary |
| North Star Metric + input metrics | `/north-star` | Nenhum equivalente |
| Market sizing TAM/SAM/SOM | Skill `market-sizing` | Nenhum equivalente |
| Prioritization frameworks (RICE, ICE, Kano, 9 total) | Skill `prioritization-frameworks` | Nenhum equivalente |
| A/B test analysis | `/analyze-test` | Nenhum equivalente |
| Cohort analysis | `/analyze-cohorts` | Nenhum equivalente |
| Sprint retro/plan/release-notes | `/sprint` | Nenhum equivalente |
| Feature request triage | `/triage-requests` | Nenhum equivalente |
| GTM strategy/launch/battlecards | `/plan-launch`, `/battlecard` | Nenhum equivalente |
| PRD writing | `/write-prd` | John (PM) — complementar |
| Interview scripts + synthesis | `/interview` | Nenhum equivalente |

### AI PM Copilot (slgoodrich/agents)

70-80% redundante com PM Skills. Apenas 3 capacidades exclusivas integradas:

| Capacidade | Mecanismo | Valor no Kerno |
|-----------|-----------|---------------|
| **Validation Sprint** | 3 agentes em paralelo → veredicto BUILD/DON'T BUILD | Gate 0: substituir CIS para ideias vagas |
| **PRD Stress Test** | 3 revisores (market-fit, feasibility, scope) → READY/REVISE | Gate 1: entre John e Winston |
| **Context Scanner** | Lê codebase → mapa de features/stack/integrações | Alimenta auditoria BMAD mensal |

---

## 2. Mudanças no fluxo

### Antes (v5.4)

```
CIS (opcional) → Saga → Mary → John → Winston → Freya/Idunn → Bob → Murat → Lovable → Cursor → Deploy
```

### Depois (v5.5)

```
Gate 0: AI PM Copilot validation-sprint (só ideia vaga)
    ↓
PM Skills: /discover → /strategy → /value-proposition → /competitive-analysis → /research-users → /north-star
    ↓
Saga (WDS Onda 1)
    ↓
Mary → John
    ↓
Gate 1: AI PM Copilot prd-stress-test → READY / REVISE (volta para John)
    ↓
Winston → Freya/Idunn (WDS Onda 2) → Bob → Murat
    ↓
Gate 2: qa-plan.md + architecture.md + database.md prontos
    ↓
Implementação (Claude Code direto OU Lovable) → Deploy
    ↓
Ciclo contínuo:
  PM Skills: /sprint retro · /triage-requests · /analyze-test · /analyze-cohorts · /north-star (recalibrar)
  AI PM Copilot: context-scanner → alimenta auditoria BMAD mensal
```

### O que sai

| Removido | Motivo |
|---------|--------|
| CIS como ponto de entrada de ideação | PM Skills `/discover` é mais profundo (assumptions + priorização + experiments). AI PM Copilot `validation-sprint` é mais rigoroso (veredicto paralelo) |
| CIS no fluxo do KERNO-ROADMAP | Substituído por PM Skills + Gate 0 |

> **O CIS não é deletado** — permanece disponível no BMAD para quem quiser usar Carson/Maya/Victor diretamente. Mas deixa de ser o caminho recomendado no Kerno.

### O que entra

| Adicionado | Onde no fluxo | Tipo |
|-----------|-------------|------|
| PM Skills (8 plugins, 65 skills) | Fase 1 — antes de todo o BMAD | Obrigatório (trilha completa) |
| AI PM Copilot: validation-sprint | Gate 0 — antes do PM Skills | Opcional (só ideia vaga) |
| AI PM Copilot: prd-stress-test | Gate 1 — entre John e Winston | Recomendado |
| AI PM Copilot: context-scanner | Ciclo contínuo — antes da auditoria | Recomendado (produto existente) |

---

## 3. Instalação

### PM Skills Marketplace

**No Claude Code (Cowork):**

```bash
# Adicionar o marketplace
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

**No Claude Cowork:**

1. Customize (bottom-left) → Browse plugins → Personal → +
2. Add marketplace from GitHub → `phuryn/pm-skills`

**Verificar instalação:**

```
/discover test
# Deve iniciar o workflow de discovery
```

### AI PM Copilot

```bash
# Adicionar e instalar
/plugin marketplace add https://github.com/slgoodrich/agents
/plugin install ai-pm-copilot
```

**Setup de contexto (opcional, 5-10 min):**

```
/ai-pm-copilot:pm-setup
```

Cria 8 arquivos de contexto em `.claude/product-context/` que todos os agentes referenciam. Para projetos existentes, o context-scanner descobre features automaticamente.

> **Nota de licença:** AI PM Copilot usa PolyForm Noncommercial. Uso comercial requer acordo com o autor (slgoodrich). PM Skills usa MIT — sem restrição.

---

## 4. Documentos adicionados ao framework

### Grupo F — PM Skills (antes do BMAD)

| # | Arquivo | Criador | Momento | Atualização |
|---|---------|---------|---------|-------------|
| 24 | `/docs/pm-discovery-plan.md` | PM Skills | `/discover` | Raro — fase de validação |
| 25 | `/docs/pm-strategy-canvas.md` | PM Skills | `/strategy` | A cada pivô ou revisão estratégica |
| 26 | `/docs/pm-value-proposition.md` | PM Skills | `/value-proposition` | A cada mudança de posicionamento |
| 27 | `/docs/pm-competitive-analysis.md` | PM Skills | `/competitive-analysis` | Trimestral |
| 28 | `/docs/pm-north-star.md` | PM Skills | `/north-star` | Anual ou quando modelo mudar |

> Esses documentos são opcionais individualmente. Se o PM Skills foi usado, salve os outputs em `/docs/pm-*.md` para que Mary e John os consumam.

### Grupo G — AI PM Copilot (gates de qualidade)

| # | Arquivo | Criador | Momento | Atualização |
|---|---------|---------|---------|-------------|
| 29 | `/docs/pm-validation-verdict.md` | AI PM Copilot | Gate 0 | Raro — ideação |
| 30 | `/docs/pm-prd-stress-test.md` | AI PM Copilot | Gate 1 | A cada PRD novo ou revisado |

### Context Scanner (sem documento próprio)

O context-scanner gera dados em `.claude/product-context/` — não em `/docs`. Esses dados alimentam os agentes PM e a auditoria BMAD, mas não são documentos de projeto.

---

## 5. Atualização do CLAUDE.md

Adicionar ao bloco YAML:

```yaml
# PM Skills + AI PM Copilot
pm-skills-enabled: true       # true se PM Skills foi usado na Fase 1
pm-discovery-done: false       # true se /discover foi executado
pm-strategy-done: false        # true se /strategy foi executado
pm-north-star-defined: false   # true se /north-star definiu métricas
aipm-copilot-enabled: false    # true se AI PM Copilot está instalado
aipm-prd-stress-test: false    # true se PRD passou pelo stress test
```

Adicionar ao workflow:

```
Fase 0 — Validação (opcional)
  0.0  AI PM Copilot: validation-sprint                         [só ideia vaga]

Fase 1 — PM Skills (decisão de produto)
  1.1  /discover      → pm-discovery-plan.md                    [validação]
  1.2  /strategy      → pm-strategy-canvas.md                   [estratégia 9 seções]
  1.3  /value-proposition → pm-value-proposition.md              [JTBD 6 partes]
  1.4  /competitive-analysis → pm-competitive-analysis.md        [cenário competitivo]
  1.5  /research-users → alimenta Mary + Saga                    [personas + journey]
  1.6  /north-star    → pm-north-star.md                         [métricas]

Fase 2 — Planejamento BMAD + WDS
  2.1  WDS Onda 1: Saga → wds-product-brief.md + wds-trigger-map.md
  2.2  Mary (Analyst)  → product-brief.md (alimentada por PM Skills + Saga)
  2.3  John (PM)       → prd.md + prd-milestones.md + features.md
  ✅ GATE 1: AI PM Copilot prd-stress-test → READY / REVISE
  2.4  Winston (Arch)  → architecture.md + database.md + decisions.md
  2.5  WDS Onda 2: Freya+Idunn → wds-scenarios.md + wds-design-system.md + wds-ui-roadmap.md
  2.6  Bob (SM)        → stories detalhadas
  2.7  Murat (TEA)     → qa-plan.md
  ✅ GATE 2: prd.md + architecture.md + database.md (com RLS) + qa-plan.md prontos
```

Adicionar à estrutura de `/docs`:

```
│  ── Grupo F: PM Skills (antes do BMAD) ─────────────────────────
├── pm-discovery-plan.md        ← PM Skills /discover
├── pm-strategy-canvas.md       ← PM Skills /strategy
├── pm-value-proposition.md     ← PM Skills /value-proposition
├── pm-competitive-analysis.md  ← PM Skills /competitive-analysis
├── pm-north-star.md            ← PM Skills /north-star
│
│  ── Grupo G: AI PM Copilot (gates) ─────────────────────────────
├── pm-validation-verdict.md    ← AI PM Copilot validation-sprint [opcional]
├── pm-prd-stress-test.md       ← AI PM Copilot prd-stress-test   [recomendado]
```

---

## 6. Atualização do KERNO-DOCS.md

Adicionar após o Grupo E (Auditoria), antes da seção de Mapeamento de Nomenclatura:

```markdown
### Grupo F — Criados pelo PM Skills (antes do BMAD)

Documentos de decisão estratégica criados durante a Fase 1.
Alimentam Mary, John e Saga com contexto validado.

| # | Arquivo | Criador | Command | Atualização |
|---|---------|---------|---------|-------------|
| 24 | `/docs/pm-discovery-plan.md` | PM Skills | `/discover` | Raro — fase de validação |
| 25 | `/docs/pm-strategy-canvas.md` | PM Skills | `/strategy` | A cada pivô estratégico |
| 26 | `/docs/pm-value-proposition.md` | PM Skills | `/value-proposition` | A cada mudança de posicionamento |
| 27 | `/docs/pm-competitive-analysis.md` | PM Skills | `/competitive-analysis` | Trimestral |
| 28 | `/docs/pm-north-star.md` | PM Skills | `/north-star` | Anual ou quando modelo mudar |

> **Regra:** Salve o output de cada command do PM Skills como arquivo `.md` na pasta `/docs/`.
> Use o prefixo `pm-` para distinguir dos documentos BMAD.
> Esses documentos são INPUT para os agentes BMAD — Mary e John devem lê-los.

### Grupo G — Criados pelo AI PM Copilot (gates de qualidade)

| # | Arquivo | Criador | Momento | Atualização |
|---|---------|---------|---------|-------------|
| 29 | `/docs/pm-validation-verdict.md` | AI PM Copilot | Gate 0 (ideia vaga) | Raro |
| 30 | `/docs/pm-prd-stress-test.md` | AI PM Copilot | Gate 1 (após PRD) | A cada PRD novo |

> **Regra do Gate 1:** Se o prd-stress-test retornar REVISE, volte para John antes de prosseguir para Winston.
> Documente o veredicto e as revisões em `/docs/pm-prd-stress-test.md`.
```

---

## 7. Atualização do KERNO-SEIZE-INICIO.md

### Adicionar como "Parte 0" (antes da Parte 1 — Instalação):

```markdown
## Parte 0 — PM Skills + AI PM Copilot (ANTES do BMAD)

### Por que PM Skills antes do BMAD?

O BMAD começa com Mary criando um product-brief — mas assume que você já sabe
o que construir e para quem. O PM Skills preenche esse gap com discovery,
estratégia, value proposition, análise competitiva e North Star Metric.

```
Sem PM Skills:  ideia vaga → Mary → product-brief genérico → PRD fraco
Com PM Skills:  ideia → validação → estratégia → métricas → Mary → PRD fundamentado
```

### Instalar PM Skills

```bash
# No Claude Code
claude plugin marketplace add phuryn/pm-skills
claude plugin install pm-toolkit@pm-skills
claude plugin install pm-product-strategy@pm-skills
claude plugin install pm-product-discovery@pm-skills
claude plugin install pm-market-research@pm-skills
claude plugin install pm-data-analytics@pm-skills
claude plugin install pm-marketing-growth@pm-skills
claude plugin install pm-go-to-market@pm-skills
claude plugin install pm-execution@pm-skills
```

### Instalar AI PM Copilot (gates de qualidade)

```bash
/plugin marketplace add https://github.com/slgoodrich/agents
/plugin install ai-pm-copilot
```

### Sequência recomendada

```
1. /discover [sua ideia]                    → valida se merece existir
2. /strategy [produto]                      → Strategy Canvas 9 seções
3. /value-proposition [para quem]           → JTBD 6 partes
4. /competitive-analysis [mercado]          → cenário competitivo
5. /research-users [dados ou produto]       → personas + journey map
6. /north-star [produto + modelo]           → métrica norte-estrela

Salvar outputs em /docs/pm-*.md

Depois: seguir para Parte 1 (Instalação BMAD) e Parte 2 (Agentes)
com todo o contexto do PM Skills alimentando Mary e John.
```

### Gate 0 — Validation Sprint (só ideia vaga)

Se a ideia for genuinamente aberta ("quero empreender em legaltech mas não sei o quê"),
rode o validation sprint ANTES do PM Skills:

```
/agent-teams:validation-sprint "Plataforma de [sua ideia]"
→ 3 agentes investigam em paralelo
→ Veredicto: BUILD / DON'T BUILD / NEEDS MORE EVIDENCE
```

Se BUILD → siga para PM Skills.
Se NEEDS MORE EVIDENCE → ajuste a ideia e rode novamente.
Se DON'T BUILD → considere pivotar antes de investir mais tempo.

### Gate 1 — PRD Stress Test (após John)

Depois que John criar o PRD (Parte 3 deste guia), passe pelo stress test:

```
/agent-teams:prd-stress-test /docs/prd.md
→ 3 revisores atacam: market-fit, feasibility, scope
→ Veredicto: READY TO BUILD / NEEDS REVISION / MAJOR REWORK
```

Se READY TO BUILD → siga para Winston.
Se NEEDS REVISION → volte para John com o feedback. Salve o resultado em /docs/pm-prd-stress-test.md.
```

---

## 8. Atualização do LOVABLE-SYSTEM-PROMPT-V3.md (Seção A)

Adicionar à lista de "Documentos que PROVAVELMENTE já existem":

```
Documentos que PODEM existir (se PM Skills foi usado):
- /docs/pm-discovery-plan.md — validação da ideia com experiments
- /docs/pm-strategy-canvas.md — Product Strategy Canvas 9 seções
- /docs/pm-value-proposition.md — JTBD 6 partes
- /docs/pm-competitive-analysis.md — cenário competitivo
- /docs/pm-north-star.md — North Star Metric + input metrics

Se existirem, USE-OS como contexto estratégico — não reinvente a estratégia.
A North Star Metric definida no pm-north-star.md é a métrica que guia priorizações.
```

---

## 9. Fluxo para produto existente

Para projetos que já existem (Retrofit), o PM Skills e o AI PM Copilot entram de forma diferente:

### Retroativo (uma vez, ~4-6h)

```
1. AI PM Copilot: context-scanner → mapa automático do codebase
2. PM Skills: /north-star [produto] → definir métricas (se não tem)
3. PM Skills: /competitive-analysis [mercado] → posicionamento atual
4. PM Skills: /strategy [produto] → articular trade-offs
5. BMAD Auditoria: Winston → Bob → Mary → John (alimentados pelo context-scanner)
6. Consolidar: relatório mestre + lista priorizada
```

### Contínuo (a cada ciclo)

```
PM Skills: /triage-requests → priorizar backlog de pedidos
PM Skills: /sprint retro → aprender com cada sprint
PM Skills: /analyze-test → rigor em decisões A/B
PM Skills: /analyze-cohorts → entender retenção
BMAD: John para PRD de features novas
BMAD: Winston para mudanças de arquitetura
AI PM Copilot: context-scanner → antes de cada auditoria mensal
BMAD Auditoria: mensal (KERNO-SEIZE-AUDITORIA.html)
```

---

## 10. Resumo de impacto

### Arquivos novos

| Arquivo | Tipo |
|---------|------|
| `KERNO-PM-INTEGRACAO.md` | Documento de integração (este arquivo) |

### Arquivos modificados

| Arquivo | Mudança |
|---------|---------|
| `CLAUDE.md` | YAML: flags pm-skills + aipm-copilot. Workflow: Fase 0-1 adicionadas. Estrutura /docs: Grupos F e G. |
| `KERNO-DOCS.md` | Grupos F (PM Skills, 5 docs) e G (AI PM Copilot, 2 docs). Total: 23 → 30 documentos. |
| `KERNO-SEIZE-INICIO.md` | Parte 0 adicionada (PM Skills + AI PM Copilot antes do BMAD). Gates 0 e 1 documentados. |
| `LOVABLE-SYSTEM-PROMPT-V3.md` | Seção A: lista de docs PM Skills adicionada. |
| `KERNO-ROADMAP.html` | Fase 1 (PM Skills) adicionada antes da Etapa 0 (BMAD). CIS marcado como opcional/legacy. |
| `KERNO-FLUXO.html` | Trilha A atualizada com PM Skills como primeira fase. |
| `KERNO-CHANGELOG.md` | Entrada v5.5.0. |
| `index.html` | Referência ao PM Skills e AI PM Copilot na seção de referências. Versão 5.4 → 5.5. |

### O que sai do caminho recomendado

| Removido do fluxo principal | Motivo | Permanece disponível? |
|-----------------------------|--------|----------------------|
| CIS (Carson/Maya/Victor) como ponto de entrada | PM Skills `/discover` + AI PM Copilot `validation-sprint` cobrem o mesmo terreno com mais profundidade | Sim — no BMAD, para uso direto |

### Contagem de documentos

| Grupo | Docs | Criador |
|-------|------|---------|
| A — BMAD BMM | 8 | Mary, John, Winston, Bob, Murat |
| B1 — WDS Onda 1 | 2 | Saga |
| B2 — WDS Onda 2 | 3 | Freya, Idunn |
| C — CIS | 1 | Carson/Maya/Victor |
| D — Lovable/Claude Code | 9 | Durante desenvolvimento |
| E — Auditoria BMAD | 6 | Winston, Bob, Mary, John |
| **F — PM Skills** | **5** | **PM Skills commands** |
| **G — AI PM Copilot** | **2** | **validation-sprint, prd-stress-test** |
| **Total** | **36** | |

---

## 11. Nota sobre licenças

| Ferramenta | Licença | Uso comercial |
|-----------|---------|--------------|
| Kerno Seize | — | Framework próprio |
| BMAD Method | MIT | Livre |
| PM Skills Marketplace | MIT | Livre |
| AI PM Copilot | PolyForm Noncommercial 1.0.0 | **Requer acordo separado** |

O Kerno deve documentar que o AI PM Copilot é recomendado mas opcional, e que seu uso comercial requer acordo com o autor. Os 3 gates que ele provê (validation-sprint, prd-stress-test, context-scanner) podem ser substituídos por processos manuais se a licença for um bloqueio.
