# CLAUDE.md — Kerno Seize Context

> Este arquivo fornece contexto automático para Claude Code em projetos Kerno Seize.
> Coloque na raiz do repositório. O Claude Code lê automaticamente ao abrir o projeto.

## Projeto

Este projeto segue o **Kerno Seize v6.2.1** — um framework de desenvolvimento completo que integra PM Skills (decisão de produto), BMAD Method (planejamento técnico), WDS (design), e Kerno (governança, segurança e documentação). Implementação via Claude Code direto ou Lovable.

```yaml
kerno-version: 6.2.1
bmad-version: 6.x

# PM Skills + AI PM Copilot
pm-skills-enabled: false      # true se PM Skills foi usado na Fase 1
pm-discovery-done: false       # true se /discover foi executado
pm-strategy-done: false        # true se /strategy foi executado
pm-north-star-defined: false   # true se /north-star definiu métricas
aipm-copilot-enabled: false    # true se AI PM Copilot está instalado
aipm-prd-stress-test: false    # true se PRD passou pelo stress test

# BMAD + WDS
wds-enabled: false             # altere para true se agentes WDS foram usados
wds-onda1: false               # true se Saga criou wds-product-brief.md + wds-trigger-map.md
wds-onda2: false               # true se Freya/Idunn criaram wds-scenarios.md + wds-ui-roadmap.md
cis-enabled: false             # true se CIS criou brainstorming-report.md (legacy — PM Skills substitui)
tea-enabled: true              # true se Murat criou qa-plan.md
```

> **Para Claude Code:** ao abrir este projeto, leia os campos acima para entender quais módulos foram usados.
> - Se `pm-skills-enabled: true`, os arquivos `pm-*.md` são fonte de verdade para decisões estratégicas.
> - Se `pm-north-star-defined: true`, use a métrica definida em `pm-north-star.md` para guiar priorizações.
> - Se `wds-enabled: true`, os arquivos `wds-*.md` são fonte de verdade para decisões de UI.
> - Se `tea-enabled: true`, valide mudanças estruturais contra `qa-plan.md`.
> - Se `aipm-prd-stress-test: true`, o PRD já foi validado — respeite o escopo aprovado.

## Nomenclatura

Todos os documentos em `/docs` seguem **kebab-case** (padrão BMAD):
- ✅ `architecture.md`, `prd-milestones.md`, `qa-plan.md`, `pm-strategy-canvas.md`
- ❌ `ARCHITECTURE.md`, `PRD_SYSTEM_MILESTONES.md`, `QA_PLAN.md`

## Estrutura de Documentação

```
/docs/
│
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
│
│  ── Grupo A: BMAD BMM (criados antes da implementação) ─────────
├── product-brief.md            ← Mary (Analyst) — alimentada por PM Skills + Saga
├── prd.md                      ← John (PM) — alimentado por PM Skills + Saga
├── prd-milestones.md           ← John (PM)
├── architecture.md             ← Winston (Architect)
├── decisions.md                ← Winston (Architect)
├── database.md                 ← Winston → implementação mantém
├── features.md                 ← John/Bob (PM/SM)
├── qa-plan.md                  ← Murat (TEA) — 1ª intervenção
│
│  ── Grupo B1: WDS Onda 1 (ANTES do PRD, produto B2C) ──────────
├── wds-product-brief.md        ← Saga
├── wds-trigger-map.md          ← Saga
│
│  ── Grupo B2: WDS Onda 2 (APÓS PRD+Arch, produto B2C) ─────────
├── wds-scenarios.md            ← Freya
├── wds-design-system.md        ← Freya  [pode ser paralelo desde o início]
├── wds-ui-roadmap.md           ← Idunn
│
│  ── Grupo C: CIS (legacy — PM Skills substitui) ───────────────
├── brainstorming-report.md     ← Carson/Maya/Victor (CIS)
│
│  ── Grupo D: Implementação (criados durante desenvolvimento) ───
├── ui-components.md            ← Claude Code ou Lovable
├── flows.md                    ← Claude Code ou Lovable
├── changelog.md                ← atualizado a cada mudança
├── handover.md                 ← Claude Code ou Lovable
├── security-rules.md           ← Claude Code ou Lovable
├── documentation-rules.md      ← Claude Code ou Lovable
├── session-close.md            ← ao fechar sessão
├── delivery-report.md          ← ao fechar milestone
├── security-report.md          ← pipeline/revisão
│
│  ── Grupo E: Auditoria BMAD (mensal, /docs/audit/) ────────────
├── audit/AUDIT-HISTORY.md              ← índice histórico
└── audit/YYYY-MM-DD/                   ← pasta datada
    ├── audit-estrutura.md              ← Winston
    ├── audit-codigo.md                 ← Bob
    ├── audit-produto-ux.md             ← Mary
    ├── audit-drift.md                  ← John
    ├── RELATORIO-MESTRE-AUDITORIA.md   ← consolidado
    └── audit-comparacao.md             ← John (2ª rodada em diante)
```

## Regras de Segurança (resumo)

- RLS obrigatório em toda tabela Supabase
- Nunca `service_role` no frontend
- Validar todos os inputs com Zod
- Tratar todos os erros: `const { data, error } = await ...`
- Nunca `dangerouslySetInnerHTML` sem DOMPurify
- Nunca dados sensíveis em localStorage
- Variáveis de ambiente via `import.meta.env`

## Stack Padrão

- Frontend: React + TypeScript + Vite
- Estilização: Tailwind CSS + shadcn/ui
- Backend: Supabase (Auth, Database, RLS, Edge Functions)
- Deploy: Vercel
- Monitoramento: Sentry + UptimeRobot
- CI/CD: GitHub Actions (Semgrep + npm audit + Scanner Kerno)
- PM: PM Skills Marketplace (phuryn/pm-skills) — MIT
- PM Gates: AI PM Copilot (slgoodrich/agents) — PolyForm Noncommercial

## Workflow de Desenvolvimento

```
Fase 0 — Validação (opcional, só ideia vaga)
  0.0  AI PM Copilot: validation-sprint → BUILD / DON'T BUILD

Fase 1 — PM Skills (decisão de produto)
  1.1  /discover      → pm-discovery-plan.md
  1.2  /strategy      → pm-strategy-canvas.md
  1.3  /value-proposition → pm-value-proposition.md
  1.4  /competitive-analysis → pm-competitive-analysis.md
  1.5  /research-users → alimenta Mary + Saga
  1.6  /north-star    → pm-north-star.md

Fase 2 — Planejamento BMAD + WDS
  2.1  WDS Onda 1: Saga → wds-product-brief.md + wds-trigger-map.md [B2C]
  2.2  Mary (Analyst)  → product-brief.md (alimentada por PM Skills + Saga)
  2.3  John (PM)       → prd.md + prd-milestones.md + features.md
  ✅ GATE 1: AI PM Copilot prd-stress-test → READY / REVISE
  2.4  Winston (Arch)  → architecture.md + database.md + decisions.md
  2.5  WDS Onda 2: Freya+Idunn → wds-scenarios/design-system/ui-roadmap [B2C]
  2.6  Bob (SM)        → stories detalhadas
  2.7  Murat (TEA)     → qa-plan.md [1ª intervenção]
  ✅ GATE 2: prd.md + architecture.md + database.md (com RLS) + qa-plan.md prontos

Fase 3 — Ambiente Seguro
  GitHub repo + security-audit-v3.yml + dependabot.yml

Fase 4 — Implementação
  Trilha A: Claude Code direto (recomendado — implementa + revisa)
  Trilha B: Lovable (vibe coder — implementa) + Cursor (revisa)
  ✅ GATE por milestone: delivery-report.md + SAST verde + P0 testados

Fase 5 — Deploy (Vercel + Cloudflare)
  ✅ GATE: securityheaders.com OK + Sentry ativo + UptimeRobot

Fase 6 — Monitoramento contínuo
  PM Skills: /sprint retro · /triage-requests · /analyze-test · /analyze-cohorts
  AI PM Copilot: context-scanner → alimenta auditoria
  Auditoria BMAD mensal (KERNO-SEIZE-AUDITORIA.html)
```

## Regras de versionamento

Ao modificar qualquer arquivo do framework:
1. Incremente o patch version (ex: 6.2.0 → 6.2.0) no arquivo modificado
2. Atualize `kerno-version` no bloco YAML deste CLAUDE.md
3. Adicione entrada no KERNO-CHANGELOG.md com data, o que mudou e por quê
4. Atualize a versão em todos os arquivos que referenciam a versão global

Mudança de minor (6.0 → 6.1): nova funcionalidade ou integração
Mudança de major (6 → 7): mudança de identidade ou reestruturação fundamental

## Ao revisar código neste projeto

- Verifique se `/docs` está atualizado com as mudanças
- Verifique se `changelog.md` foi atualizado
- Valide RLS em novas tabelas
- Confirme que não há secrets hardcoded
- Rode os testes do `qa-plan.md`
- Se `pm-north-star-defined: true`, confirme que mudanças movem a North Star Metric
