# KERNO-DOCS.md — Framework de Documentação Obrigatória

> **Versão:** 2.3 (Kerno Seize v6.1.0)
> **Uso:** Faça upload deste arquivo na Knowledge Base de todo projeto Lovable.dev.
> Este documento define TODOS os arquivos de documentação que devem existir no projeto,
> quando criar, quando atualizar, e o formato exato de cada um.
>
> **Mudança v2.3:** Grupos F (PM Skills) e G (AI PM Copilot) adicionados. Total: 30 documentos.
> **Mudança v2.0:** Nomenclatura migrada para padrão BMAD (kebab-case).
> Documentos de planejamento agora são criados pelo BMAD ANTES do Lovable.

---

## Regra Geral

Toda documentação fica na pasta `/docs` na raiz do projeto.

### Dois momentos de criação:

1. **ANTES do Lovable (Etapas 1-2):** Os agentes BMAD criam os documentos de planejamento, arquitetura, design e testes no Claude Code. Esses documentos são enviados ao Lovable como Knowledge Base.

2. **DURANTE o desenvolvimento (Etapa 4):** O Lovable cria os documentos operacionais (changelog, sessão, entrega) e ATUALIZA os documentos que recebeu do BMAD conforme o código evolui.

**Regra de ouro:** Se um documento já existir quando o Lovable iniciar, ele NÃO deve recriar — deve IMPORTAR e ATUALIZAR durante o desenvolvimento.

---

## Lista Completa de Documentos

### Grupo A — Criados pelo BMAD (antes do Lovable)

| # | Arquivo | Criador | Agente | Atualização |
|---|---------|---------|--------|-------------|
| 1 | `/docs/product-brief.md` | BMAD BMM | Mary (Analyst) | Raro — apenas se pivotar |
| 2 | `/docs/prd.md` | BMAD BMM | John (PM) | A cada mudança de escopo |
| 3 | `/docs/prd-milestones.md` | BMAD BMM | John (PM) | A cada conclusão ou replanejamento |
| 4 | `/docs/architecture.md` | BMAD BMM | Winston (Architect) | A cada mudança estrutural |
| 5 | `/docs/decisions.md` | BMAD BMM | Winston (Architect) | A cada decisão relevante |
| 6 | `/docs/database.md` | BMAD BMM | Winston (Architect) | Schema inicial → Lovable detalha |
| 7 | `/docs/features.md` | BMAD BMM | John/Bob (PM/SM) | A cada feature implementada ou planejada |
| 8 | `/docs/qa-plan.md` | BMAD TEA | Murat (Test Architect) | A cada feature que precisa de teste |

### Grupo B — Criados pelo WDS (opcionais, antes do Lovable)

O WDS entra em **duas ondas** com momentos distintos. Ambas as ondas são opcionais, mas recomendadas para produtos B2C.

#### Grupo B1 — WDS Onda 1: Estratégia (ANTES do PRD)

Criados por Saga, **antes** de John fechar o PRD, para que a pesquisa de usuário informe o produto.

| # | Arquivo | Criador | Agente | Momento | Atualização |
|---|---------|---------|--------|---------|-------------|
| 9 | `/docs/wds-product-brief.md` | WDS | Saga | Antes do PRD | Raro — apenas se pivotar |
| 10 | `/docs/wds-trigger-map.md` | WDS | Saga | Antes do PRD | A cada mudança de estratégia |

#### Grupo B2 — WDS Onda 2: Design e Handoff (APÓS PRD e Arquitetura)

Criados por Freya e Idunn, **após** Winston fechar a arquitetura, para especificar como o produto vai parecer e funcionar.

> **Exceção:** O Design System visual (cores, tipografia, tokens) pode ser iniciado a qualquer momento — não depende do PRD. Apenas os cenários UX e o UI Roadmap precisam do escopo fechado.

| # | Arquivo | Criador | Agente | Momento | Atualização |
|---|---------|---------|--------|---------|-------------|
| 11 | `/docs/wds-scenarios.md` | WDS | Freya | Após PRD + Arquitetura | A cada novo fluxo UX |
| 12 | `/docs/wds-design-system.md` | WDS | Freya | Após ideia definida (paralelo) | A cada mudança visual |
| 13 | `/docs/wds-ui-roadmap.md` | WDS | Idunn | Após wds-scenarios.md | A cada novo sprint de UI |

### Grupo C — Criados pelo CIS (opcionais, antes do BMAD)

| # | Arquivo | Criador | Agente | Atualização |
|---|---------|---------|--------|-------------|
| 14 | `/docs/brainstorming-report.md` | CIS | Carson/Maya/Victor | Raro — fase de ideação |

### Grupo D — Criados pelo Lovable (durante desenvolvimento)

| # | Arquivo | Criador | Atualização |
|---|---------|---------|-------------|
| 15 | `/docs/ui-components.md` | Lovable | A cada novo componente ou hook |
| 16 | `/docs/flows.md` | Lovable | A cada novo fluxo (complementa WDS) |
| 17 | `/docs/changelog.md` | Lovable | A CADA prompt respondido |
| 18 | `/docs/handover.md` | Lovable | No fechamento de cada milestone |
| 19 | `/docs/security-rules.md` | Lovable | A cada nova regra ou exceção |
| 20 | `/docs/documentation-rules.md` | Lovable | Raramente |
| 21 | `/docs/session-close.md` | Lovable | A cada fechamento de sessão |
| 22 | `/docs/delivery-report.md` | Lovable | No fechamento de milestone |
| 23 | `/docs/security-report.md` | Lovable | Após cada execução do pipeline |

### Grupo F — Criados pelo PM Skills (antes do BMAD)

Documentos de decisão estratégica criados durante a Fase 1.
Alimentam Mary, John e Saga com contexto validado.

> **Regra:** Salve o output de cada command do PM Skills como arquivo `.md` na pasta `/docs/`.
> Use o prefixo `pm-` para distinguir dos documentos BMAD.
> Esses documentos são INPUT para os agentes BMAD — Mary e John devem lê-los.

| # | Arquivo | Criador | Command | Atualização |
|---|---------|---------|---------|-------------|
| 24 | `/docs/pm-discovery-plan.md` | PM Skills | `/discover` | Raro — fase de validação |
| 25 | `/docs/pm-strategy-canvas.md` | PM Skills | `/strategy` | A cada pivô estratégico |
| 26 | `/docs/pm-value-proposition.md` | PM Skills | `/value-proposition` | A cada mudança de posicionamento |
| 27 | `/docs/pm-competitive-analysis.md` | PM Skills | `/competitive-analysis` | Trimestral |
| 28 | `/docs/pm-north-star.md` | PM Skills | `/north-star` | Anual ou quando modelo mudar |

### Grupo G — Criados pelo AI PM Copilot (gates de qualidade)

| # | Arquivo | Criador | Momento | Atualização |
|---|---------|---------|---------|-------------|
| 29 | `/docs/pm-validation-verdict.md` | AI PM Copilot | Gate 0 (ideia vaga) | Raro |
| 30 | `/docs/pm-prd-stress-test.md` | AI PM Copilot | Gate 1 (após PRD) | A cada PRD novo |

> **Regra do Gate 1:** Se o prd-stress-test retornar REVISE, volte para John antes de prosseguir para Winston.
> Documente o veredicto e as revisões em `/docs/pm-prd-stress-test.md`.
>
> **Nota de licença:** AI PM Copilot usa PolyForm Noncommercial. Os gates podem ser substituídos por processos manuais se a licença for bloqueio.

---

## Mapeamento de Nomenclatura (Kerno v3.0 → v5.0)

| Kerno v3.0 (antigo) | Kerno v5.0 (novo) | Motivo |
|---------------------|-------------------|--------|
| `ARCHITECTURE.md` | `architecture.md` | Padrão BMAD (kebab-case) |
| `DATABASE.md` | `database.md` | Padrão BMAD |
| `UI_COMPONENTS.md` | `ui-components.md` | Padrão BMAD |
| `FLOWS.md` | `flows.md` | Padrão BMAD |
| `FEATURES.md` | `features.md` | Padrão BMAD |
| `DECISIONS.md` | `decisions.md` | Padrão BMAD |
| `CHANGELOG.md` | `changelog.md` | Padrão BMAD |
| `HANDOVER.md` | `handover.md` | Padrão BMAD |
| `SECURITY_RULES.md` | `security-rules.md` | Padrão BMAD |
| `DOCUMENTATION_RULES.md` | `documentation-rules.md` | Padrão BMAD |
| `SESSION_CLOSE.md` | `session-close.md` | Padrão BMAD |
| `DELIVERY_REPORT.md` | `delivery-report.md` | Padrão BMAD |
| `QA_PLAN.md` | `qa-plan.md` | Padrão BMAD |
| `SECURITY_REPORT.md` | `security-report.md` | Padrão BMAD |
| `PRD_SYSTEM_MILESTONES.md` | `prd-milestones.md` | Padrão BMAD + nome mais direto |
| `PRD_SYSTEM.md` | `prd.md` | Padrão BMAD + nome mais direto |
| _(não existia)_ | `product-brief.md` | Novo — Mary (Analyst) |
| _(não existia)_ | `brainstorming-report.md` | Novo — CIS (Carson/Maya) |
| _(não existia)_ | `wds-product-brief.md` | Novo — WDS (Saga) |
| _(não existia)_ | `wds-trigger-map.md` | Novo — WDS (Saga) |
| _(não existia)_ | `wds-scenarios.md` | Novo — WDS (Freya) |
| _(não existia)_ | `wds-design-system.md` | Novo — WDS (Freya) |
| _(não existia)_ | `wds-ui-roadmap.md` | Novo — WDS (Idunn) |

---

## 1. product-brief.md — Product Brief

**Criador:** Mary (Analyst) — BMAD BMM
**Objetivo:** Visão do produto, personas, mercado e critérios de sucesso.

**Formato obrigatório:**

```markdown
# Product Brief

## Visão do Produto
[O que estamos construindo e por quê — em 3 frases]

## Problema
[Que dor ou necessidade resolve]

## Personas
| Persona | Descrição | Necessidades | Dores |
|---------|-----------|--------------|-------|

## Mercado
[Tamanho, concorrência, oportunidade]

## Critérios de Sucesso
[Como sabemos se funcionou — métricas]

## Restrições e Premissas
[Limitações técnicas, de tempo, orçamento, equipe]
```

---

## 2. prd.md — Product Requirements Document

**Criador:** John (PM) — BMAD BMM
**Objetivo:** Documento de requisitos completo do produto.

**Formato obrigatório:**

```markdown
# Product Requirements Document

## Resumo Executivo
[O que, para quem, por quê]

## Requisitos Funcionais
| # | Feature | Descrição | Prioridade | Épico |
|---|---------|-----------|------------|-------|

## Requisitos Não-Funcionais
| Categoria | Requisito |
|-----------|----------|
| Performance | [ex: < 3s para load inicial] |
| Segurança | [ex: auth obrigatória, RLS em tudo] |
| Acessibilidade | [nível desejado] |

## User Stories
### Épico: [Nome]
- Como [persona], quero [ação] para [benefício]
  - Critério de aceite: [critério]

## Integrações
| Serviço | Propósito | Prioridade |
|---------|-----------|------------|

## Dados Sensíveis
[Tipos de dados: saúde, financeiro, pessoal, etc.]

## Fora de Escopo
[O que NÃO será implementado nesta versão]
```

---

## 3. prd-milestones.md — Milestones do Produto

**Criador:** John (PM) — BMAD BMM
**Objetivo:** Plano de implementação faseado seguindo a Árvore de Dependências.

**Formato obrigatório:**

```markdown
# Milestones do Produto

## Milestone 1: [Nome]
**Status:** 🟢 Concluída | 🟡 Em andamento | ⬜ Não iniciada
**Período:** YYYY-MM-DD a YYYY-MM-DD

### Escopo
- [Feature 1]
- [Feature 2]

### Dependências
- [O que precisa estar pronto antes]

### Critérios de Aceite
- [Critério 1]
- [Critério 2]

[Repetir para cada milestone]

## Visão Geral de Progresso
| Milestone | Status | % |
|-----------|--------|---|
| 1. [Nome] | 🟢 | 100% |
| 2. [Nome] | 🟡 | 60% |
| 3. [Nome] | ⬜ | 0% |
```

---

## 4. architecture.md — Arquitetura do Sistema

**Criador:** Winston (Architect) — BMAD BMM
**Objetivo:** Visão técnica completa de como o sistema é construído.

**Quando atualizar:** Ao adicionar/remover tecnologia, mudar estrutura de pastas, alterar padrões.

**Formato obrigatório:**

```markdown
# Arquitetura do Sistema

## Visão Geral
[Parágrafo descrevendo o que o sistema faz e como]

## Stack Tecnológica
| Camada | Tecnologia |
|--------|-----------|
| Frontend | [ex: React 18 + TypeScript + Vite] |
| Estilização | [ex: Tailwind CSS + shadcn/ui] |
| Backend | [ex: Supabase (Auth, Database, RLS)] |
| Estado | [ex: TanStack React Query v5] |
| Roteamento | [ex: React Router DOM v6] |
| Deploy | [ex: Vercel] |
| CDN/Proxy | [ex: Cloudflare] |

## Estrutura de Pastas
[Árvore de pastas com descrição de cada pasta principal]

## Variáveis de Ambiente
| Variável | Descrição | Onde usar |
|----------|-----------|----------|
| VITE_SUPABASE_URL | URL do Supabase | Frontend |

## Padrões Utilizados
[Lista de design patterns e convenções adotadas]

## Integrações Externas
| Serviço | Propósito | Autenticação |
|---------|-----------|--------------|
```

---

## 5. decisions.md — Decisões Técnicas (ADRs)

**Criador:** Winston (Architect) — BMAD BMM (iniciais) + Lovable (durante dev)
**Objetivo:** Registrar POR QUE cada decisão técnica foi tomada.

**Formato obrigatório:**

```markdown
## ADR-XXX: [Título]
**Data:** YYYY-MM-DD
**Status:** Proposta | Aceita | Depreciada | Substituída
**Contexto:** [Por que essa decisão foi necessária]
**Decisão:** [O que foi decidido]
**Alternativas:** [O que foi considerado e descartado]
**Consequências:** [Trade-offs e impactos]
```

---

## 6. database.md — Banco de Dados

**Criador:** Winston (Architect) — BMAD BMM (schema inicial) + Lovable (detalhamento)
**Objetivo:** Documentação completa do schema, policies RLS e migrações.

**Formato obrigatório:**

```markdown
# Banco de Dados

## Visão Geral
[Descrição do banco e tecnologia]

## Tabelas

### [nome_da_tabela]
| Coluna | Tipo | Descrição | Constraints |
|--------|------|-----------|-------------|

> **RLS:** [Status — Ativo/Inativo]
> **Policies:** [Lista resumida de policies]

## Enums
[Lista de enums e valores]

## Políticas RLS
| Tabela | Policy | Operação | Regra |
|--------|--------|----------|-------|

## Funções e Triggers
| Nome | Tipo | Descrição |
|------|------|-----------|

## Migrações
| # | Data | Descrição |
|---|------|-----------|

## Storage Buckets
| Bucket | Público/Privado | Tipos aceitos | Max size |
|--------|-----------------|---------------|----------|
```

---

## 7. features.md — Features

**Criador:** John/Bob (PM/SM) — BMAD BMM (lista inicial) + Lovable (atualiza status)
**Objetivo:** Rastrear o que foi implementado, o que está planejado e o backlog.

**Formato obrigatório:**

```markdown
# Features

## Implementadas
- [x] [Feature] — [Descrição breve]

## Em Andamento
- [ ] [Feature] — [Descrição + quem/quando]

## Planejadas
- [ ] [Feature] — [Descrição]

## Backlog
- [ ] [Ideia futura]
```

---

## 8. qa-plan.md — Plano de Qualidade

**Criador:** Murat (Test Architect) — BMAD TEA (estratégia) + Lovable (cenários durante dev)
**Objetivo:** Estratégia de testes baseada em risco, priorização P0-P3.

**Formato obrigatório:**

```markdown
# Plano de QA

## Estratégia de Testes
[Abordagem geral: o que testar, o que priorizar, níveis de teste]

## Priorização por Risco
| Feature | Risco | Impacto | Prioridade |
|---------|-------|---------|------------|
| Auth | Alto | Crítico | P0 |

## Testes por Feature

### [Nome da Feature]
| # | Cenário | Passos | Resultado Esperado | Status |
|---|---------|--------|-------------------|--------|
| 1 | [Cenário] | [Passos] | [Esperado] | ⬜ Pendente |

## Testes de Segurança
| # | Verificação | Como testar | Status |
|---|-------------|-------------|--------|
| 1 | RLS ativo em todas as tabelas | Painel Supabase | ⬜ |
| 2 | service_role ausente no frontend | Buscar no código | ⬜ |
| 3 | Inputs validados com Zod | Revisar formulários | ⬜ |

## Testes de Responsividade
| Página | Mobile | Tablet | Desktop | Status |
|--------|--------|--------|---------|--------|

## Plano de Automação
[O que automatizar, com quais ferramentas, quando]
```

---

## 9-13. Documentos WDS (opcionais)

Os documentos WDS são criados pelos agentes do Whiteport Design Studio quando o módulo é utilizado. Eles seguem o padrão de nomes do próprio WDS com prefixo `wds-`.

| Arquivo | Agente | Conteúdo |
|---------|--------|----------|
| `wds-product-brief.md` | Saga | Visão de produto do ponto de vista de design |
| `wds-trigger-map.md` | Saga | Mapa de gatilhos: metas de negócio × psicologia do usuário |
| `wds-scenarios.md` | Freya | Cenários de uso, interações, hierarquia de conteúdo |
| `wds-design-system.md` | Freya | Design tokens, componentes visuais, identidade |
| `wds-ui-roadmap.md` | Idunn | Specs de componentes, Object IDs, handoff para dev |

**Nota:** O formato desses documentos é definido pelo próprio WDS. Não impomos formato aqui — o WDS tem seus próprios templates.

---

## 14. brainstorming-report.md — Relatório de Brainstorming

**Criador:** CIS (Carson/Maya/Victor) — BMAD CIS
**Objetivo:** Registro da fase de ideação e validação.
**Quando usar:** Apenas quando o projeto inicia pela Fase 0 (ideia vaga).

**Formato obrigatório:**

```markdown
# Relatório de Brainstorming

## Ideia Original
[A ideia como chegou — vaga ou refinada]

## Técnicas Utilizadas
[SCAMPER, Design Thinking, Análise SWOT, etc.]

## Insights Principais
- [Insight 1]
- [Insight 2]

## Validações
| Aspecto | Status | Notas |
|---------|--------|-------|
| Mercado | ✅/⚠️/❌ | [notas] |
| Viabilidade | ✅/⚠️/❌ | [notas] |
| Diferencial | ✅/⚠️/❌ | [notas] |

## Decisão
[Seguir com a ideia? Pivotar? Descartar? Por quê?]
```

---

## 15. ui-components.md — Componentes de Interface

**Criador:** Lovable (durante desenvolvimento)
**Objetivo:** Catálogo de todos os componentes customizados, hooks e páginas.

**Formato obrigatório:**

```markdown
# Componentes UI

## Biblioteca Base
[Nome da biblioteca de componentes]

## Componentes Customizados
| Componente | Caminho | Descrição | Props |
|------------|---------|-----------|-------|

## Hooks Customizados
| Hook | Caminho | Descrição | Retorno |
|------|---------|-----------|---------|

## Páginas
| Página | Rota | Descrição | Auth? |
|--------|------|-----------|-------|

## Design Tokens
[Cores, fontes, espaçamentos — nunca hardcode]

## Diretrizes de Estilo
[Regras de estilização do projeto]
```

---

## 16. flows.md — Fluxos do Sistema

**Criador:** Lovable (complementa cenários do WDS com fluxos técnicos)
**Objetivo:** Documentar cada fluxo funcional passo a passo.

**Formato obrigatório:**

```markdown
# Fluxos do Sistema

## [Nome do Fluxo]
1. [Passo 1]
2. [Passo 2]
...

[Repetir para cada fluxo: autenticação, CRUD, pagamento, etc.]
```

---

## 17. changelog.md — Log de Mudanças

**Criador:** Lovable (a cada prompt respondido)
**Objetivo:** Registro cronológico de TODAS as alterações.

**Formato obrigatório:**

```markdown
## [X.Y.Z] - YYYY-MM-DD

### Adicionado
- [Nova feature ou arquivo]

### Modificado
- [Mudança em feature existente]

### Corrigido
- [Bug fix]

### Removido
- [Feature removida]

### Segurança
- [Correção de segurança]
```

**Regra de versionamento:**
- X (major): Mudança que quebra compatibilidade
- Y (minor): Nova feature
- Z (patch): Bug fix ou ajuste

---

## 18. handover.md — Documento de Entrega

**Criador:** Lovable (no fechamento de cada milestone)
**Objetivo:** Tudo que alguém precisa saber para assumir o projeto.

**Formato obrigatório:**

```markdown
# Handover do Projeto

## Resumo Executivo
[O que o sistema faz em 3 frases]

## Acesso e Credenciais
[Onde estão, como acessar — NUNCA colocar valores reais]

## Como Rodar Localmente
[Passo a passo: clone, install, env, dev]

## Arquitetura Resumida
[Stack + decisões principais]

## Funcionalidades Principais
[Lista numerada das features core]

## Pontos de Atenção
[O que NÃO fazer, cuidados especiais]

## Status Atual
[O que está pronto, o que falta]

## Próximos Passos
[Lista priorizada do que fazer a seguir]
```

---

## 19. security-rules.md — Regras de Segurança

**Criador:** Lovable (baseado no SECURITY.md do Kerno)
**Objetivo:** Regras de segurança que o código deve seguir. Baseado em OWASP Top 10.
**Conteúdo:** O arquivo SECURITY.md completo com regras de Supabase, Auth, Validação, XSS, Dados Sensíveis, HTTPS, Error Handling, IDs/URLs.

---

## 20. documentation-rules.md — Regras de Documentação

**Criador:** Lovable (apenas quando o processo mudar)
**Objetivo:** Meta-documento que explica como a documentação funciona.

**Conteúdo:**

```markdown
# Regras de Documentação

## Princípio
Toda alteração de código DEVE ser acompanhada de atualização na documentação correspondente.

## Gatilhos de Atualização
| Evento | Documentos a atualizar |
|--------|----------------------|
| Nova tabela | database.md, changelog.md |
| Novo componente | ui-components.md, changelog.md |
| Nova feature | features.md, changelog.md, flows.md |
| Nova rota | ui-components.md, flows.md |
| Decisão técnica | decisions.md |
| Qualquer mudança | changelog.md (SEMPRE) |
| Fim de sessão | session-close.md |
| Fim de milestone | delivery-report.md, handover.md |
| Mudança de segurança | security-rules.md, changelog.md |

## Formato
- Markdown puro, sem HTML
- Tabelas para dados estruturados
- Código em blocos com linguagem especificada
- Datas no formato YYYY-MM-DD
- Nomes de arquivo em kebab-case (padrão BMAD)
```

---

## 21. session-close.md — Fechamento de Sessão

**Criador:** Lovable (final de cada sessão)

**Formato obrigatório:**

```markdown
# Fechamento de Sessão

## Sessão [N] — YYYY-MM-DD

### Resumo do que foi feito
- [Lista de ações realizadas]

### Arquivos criados/modificados
- [Lista de arquivos com descrição da mudança]

### Estado atual do projeto
- **Milestone atual:** [Nome/número]
- **% concluído da milestone:** [X%]
- **Próxima ação:** [O que fazer na próxima sessão]

### Pendências e Bloqueios
- [O que ficou pendente e por quê]

### Documentação atualizada nesta sessão
- [Lista de docs atualizados]
```

---

## 22. delivery-report.md — Relatório de Entrega

**Criador:** Lovable (no fechamento de cada milestone)

**Formato obrigatório:**

```markdown
# Relatório de Entrega

## Milestone [N]: [Nome]
**Data de início:** YYYY-MM-DD
**Data de entrega:** YYYY-MM-DD
**Status:** Entregue | Parcial | Atrasado

### Escopo Planejado
- [Lista do que estava previsto]

### Escopo Entregue
- [x] [Feature entregue]
- [ ] [Feature não entregue — motivo]

### Métricas
| Métrica | Valor |
|---------|-------|
| Features planejadas | X |
| Features entregues | Y |
| Bugs encontrados | Z |
| Bugs corrigidos | W |

### Problemas Encontrados
- [Problema e como foi resolvido ou contornado]

### Recomendações para Próxima Milestone
- [O que melhorar no processo]
```

---

## 23. security-report.md — Relatório de Segurança

**Criador:** Pipeline SAST + Lovable

**Formato obrigatório:**

```markdown
# Relatório de Segurança

## Última Auditoria: YYYY-MM-DD

### Resumo
| Gravidade | Quantidade |
|-----------|-----------|
| 🔴 Crítico | X |
| 🟠 Alto | X |
| 🟡 Médio | X |
| ✅ OK | X |

### Ferramentas Utilizadas
- Semgrep (análise estática)
- npm audit (dependências)
- Scanner Lovable+Supabase (customizado)
- Dependabot (monitoramento contínuo)

### Problemas Encontrados
[Lista de problemas do último scan]

### Correções Aplicadas
[O que foi corrigido e quando]

### Pendências
[O que ainda precisa ser corrigido]

### Verificações Manuais
| Verificação | Status | Data |
|-------------|--------|------|
| RLS em todas as tabelas | ✅/⬜ | YYYY-MM-DD |
| service_role fora do frontend | ✅/⬜ | YYYY-MM-DD |
| Headers de segurança no Vercel | ✅/⬜ | YYYY-MM-DD |
```

---

## Resumo dos Gatilhos

| Evento no Desenvolvimento | Documentos que DEVEM ser atualizados |
|---------------------------|--------------------------------------|
| **Qualquer alteração de código** | changelog.md |
| **Nova tabela ou coluna** | database.md, changelog.md |
| **Nova policy RLS** | database.md, security-report.md |
| **Novo componente ou hook** | ui-components.md, changelog.md |
| **Nova página ou rota** | ui-components.md, flows.md, changelog.md |
| **Nova feature completa** | features.md, flows.md, qa-plan.md, changelog.md |
| **Decisão técnica** | decisions.md, changelog.md |
| **Mudança de arquitetura** | architecture.md, decisions.md, changelog.md |
| **Correção de segurança** | security-rules.md, security-report.md, changelog.md |
| **Fim de sessão** | session-close.md |
| **Fim de milestone** | delivery-report.md, handover.md, prd-milestones.md |
| **Início do projeto no Lovable** | Criar apenas os docs do Grupo D (15-23) |
