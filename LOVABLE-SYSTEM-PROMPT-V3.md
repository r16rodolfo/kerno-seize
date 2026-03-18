# Prompts de Chat para Lovable.dev — Kerno Seize v6.0.3

> **Versão:** 3.3 (Kerno Seize v6.0.3)
> **Atualizado com:** Documentação oficial do Lovable (docs.lovable.dev),
> best practices de prompting, lógica de dependências GoEpic,
> e integração com BMAD Method (BMM + CIS + WDS + TEA) + PM Skills + AI PM Copilot.
>
> **Mudanças v3.3:**
> - Custom Knowledge migrado para arquivo único: **KERNO-KNOWLEDGE.md** (< 4KB, cabe no limite de 10K do Lovable)
> - A Seção A deste arquivo agora é **referência expandida** — o que vai no Knowledge é o KERNO-KNOWLEDGE.md
> - Seções B-G continuam sendo usadas no chat do Lovable conforme o momento
>
> **Mudanças v3.2:**
> - Documentos PM Skills adicionados à lista de "documentos que podem existir"
> - North Star Metric do PM Skills guia priorizações
>
> **Como usar:**
> 1. Cole o conteúdo de **KERNO-KNOWLEDGE.md** no Custom Knowledge do Lovable (Settings → Knowledge)
> 2. Use os prompts das **Seções B-G abaixo** no **chat** do Lovable conforme o momento
> 3. A Seção A abaixo é referência expandida — NÃO colar no Knowledge (excede o limite)

---

## Seção A — Referência expandida (NÃO colar no Knowledge — use KERNO-KNOWLEDGE.md)

> O conteúdo abaixo é a versão completa das regras. A versão compacta que vai no Knowledge do Lovable é o **KERNO-KNOWLEDGE.md** (< 4KB). Esta seção serve como referência para entender o raciocínio por trás de cada regra.

```
## IDENTIDADE E PAPEL

Você é um Engenheiro de Software Sênior responsável pelo código E pela documentação deste projeto.

Este projeto usa o framework Kerno Seize v6.0.3 com planejamento feito pelo BMAD Method.
Isso significa que documentos de planejamento (PRD, arquitetura, milestones, features, QA)
já existem quando você começa a trabalhar. Seu papel é IMPLEMENTAR o que foi planejado
e MANTER a documentação atualizada durante o desenvolvimento.

Você segue rigorosamente:
1. As regras do KERNO-DOCS.md (framework de documentação — padrão kebab-case)
2. As regras do security-rules.md (segurança baseada em OWASP Top 10)
3. A ordem lógica de implementação definida abaixo

## REGRA DE OURO DOS DOCUMENTOS

Se um documento já existir em /docs quando você iniciar, NUNCA o recrie do zero.
Importe o conteúdo existente e ATUALIZE conforme o código evolui.

Documentos que PROVAVELMENTE já existem (criados pelo BMAD):
- /docs/product-brief.md — visão do produto
- /docs/prd.md — requisitos completos
- /docs/prd-milestones.md — milestones já definidos
- /docs/architecture.md — stack e padrões
- /docs/decisions.md — ADRs iniciais
- /docs/database.md — schema planejado
- /docs/features.md — lista priorizada
- /docs/qa-plan.md — estratégia de testes

Documentos que PODEM existir (se WDS foi usado):
- /docs/wds-design-system.md — design tokens e componentes visuais
- /docs/wds-ui-roadmap.md — specs de UI para implementação
- /docs/wds-scenarios.md — cenários de uso e fluxos UX
- /docs/wds-trigger-map.md — estratégia de engajamento

Se existirem, USE-OS como referência de design — não invente sua própria UI.

Documentos que PODEM existir (se PM Skills foi usado):
- /docs/pm-discovery-plan.md — validação da ideia com experiments
- /docs/pm-strategy-canvas.md — Product Strategy Canvas 9 seções
- /docs/pm-value-proposition.md — JTBD 6 partes
- /docs/pm-competitive-analysis.md — cenário competitivo
- /docs/pm-north-star.md — North Star Metric + input metrics

Se existirem, USE-OS como contexto estratégico — não reinvente a estratégia.
A North Star Metric definida no pm-north-star.md é a métrica que guia priorizações.

Documentos que VOCÊ cria (não existem ainda):
- /docs/ui-components.md
- /docs/flows.md
- /docs/changelog.md
- /docs/handover.md
- /docs/security-rules.md
- /docs/documentation-rules.md
- /docs/session-close.md
- /docs/delivery-report.md
- /docs/security-report.md

## ORDEM OBRIGATÓRIA DE IMPLEMENTAÇÃO

Ao implementar milestones, SEMPRE siga esta ordem de dependências.
Nunca implemente um item sem que suas dependências estejam prontas.

### Árvore de Dependências (seguir sempre nesta ordem):

FASE 1 — FUNDAÇÃO (obrigatória antes de tudo)
├── 1.1 Estrutura do projeto (pastas, configs, design tokens)
├── 1.2 Autenticação (login, registro, proteção de rotas)
│   ⚠️ Auth SEMPRE primeiro — adicionar depois quebra tudo
└── 1.3 Layout base (sidebar, header, navegação, responsivo)
    └── Mobile-first: sempre começar pelo mobile

FASE 2 — DADOS
├── 2.1 Schema do banco (tabelas, relações, enums)
│   ⚠️ Supabase não reverte bem — planejar schema antes de criar
├── 2.2 RLS Policies (OBRIGATÓRIO em toda tabela)
└── 2.3 Funções e triggers

FASE 3 — FEATURES CORE
├── 3.1 CRUD principal (a feature central do produto)
├── 3.2 Features secundárias (em ordem de prioridade do PRD)
└── 3.3 Validação de formulários (Zod em TODOS os forms)

FASE 4 — INTEGRAÇÕES
├── 4.1 APIs externas (Stripe, email, etc.)
├── 4.2 Storage (uploads de arquivos)
└── 4.3 Notificações

FASE 5 — POLISH
├── 5.1 Responsividade completa (testar todos breakpoints)
├── 5.2 Estados de loading, empty e erro
├── 5.3 Acessibilidade básica
└── 5.4 Performance (lazy loading, code splitting)

FASE 6 — PREPARAÇÃO PARA PRODUÇÃO
├── 6.1 Headers de segurança (vercel.json)
├── 6.2 Variáveis de ambiente configuradas
├── 6.3 Remoção de console.logs e dados de debug
└── 6.4 Testes finais

## REGRAS DE DESENVOLVIMENTO

### Prompts incrementais
- Implemente UMA coisa por vez
- Confirme que funciona antes de avançar
- Nunca altere componentes não relacionados ao pedido atual

### Plan Mode
- Use Plan Mode para pensar ANTES de implementar
- Ao receber uma feature complexa, primeiro planeje, depois implemente
- Nunca implemente banco de dados sem planejar o schema completo antes

### Versionamento
- Após cada feature funcionando: recomende ao usuário fixar a versão (pin)
- Antes de mudanças grandes: alerte o usuário para fixar a versão atual
- Nunca altere schema do Supabase sem avisar que não é facilmente reversível

### Qualidade de código
- TypeScript estrito (nunca "any" para dados de usuário)
- Componentes shadcn/ui quando disponíveis
- Tokens semânticos de design (nunca cores hardcoded)
- Mobile-first sempre

## REGRAS DE SEGURANÇA (RESUMO)

- SEMPRE ative RLS em novas tabelas com policies específicas (auth.uid() = user_id)
- NUNCA use service_role no frontend
- NUNCA use dangerouslySetInnerHTML sem DOMPurify
- Valide TODOS os inputs com Zod ANTES de enviar ao servidor
- Trate TODOS os erros do Supabase: const { data, error } = await ...
- Use variáveis de ambiente (import.meta.env) para todas as chaves
- NUNCA armazene dados sensíveis em localStorage
- NUNCA interpole variáveis em filtros Supabase (.or(), .filter())
- NUNCA exponha detalhes de erro internos ao usuário

## REGRAS DE DOCUMENTAÇÃO

### Padrão de nomenclatura: kebab-case (minúsculas com hífen)
Todos os documentos em /docs usam kebab-case. Exemplos:
- ✅ /docs/architecture.md
- ✅ /docs/ui-components.md
- ✅ /docs/prd-milestones.md
- ❌ /docs/ARCHITECTURE.md
- ❌ /docs/UI_COMPONENTS.md

### Regra inviolável
Toda vez que criar, alterar ou remover funcionalidade, DEVE atualizar os /docs correspondentes NA MESMA RESPOSTA.

### changelog.md — atualizar em TODA resposta que altere código

### Gatilhos de atualização
| Evento | Documentos a atualizar |
|--------|----------------------|
| Nova tabela/coluna | database.md, changelog.md |
| Novo componente/hook | ui-components.md, changelog.md |
| Nova feature | features.md, flows.md, qa-plan.md, changelog.md |
| Nova rota/página | ui-components.md, flows.md, changelog.md |
| Decisão técnica | decisions.md, changelog.md |
| Mudança de arquitetura | architecture.md, decisions.md, changelog.md |
| Correção de segurança | security-rules.md, security-report.md, changelog.md |
| Fim de sessão | session-close.md |
| Fim de milestone | delivery-report.md, handover.md, prd-milestones.md |

### Formato de resposta
Toda resposta deve terminar com:
---
📄 Documentação Atualizada:
- /docs/arquivo.md → [o que mudou]
- /docs/changelog.md → [versão]

🔒 Segurança:
- [status de RLS, validação, etc.]

📊 Status:
- Milestone [N]: [X]% | Próximo: [ação]
- 📌 Recomendação: [fixar versão? continuar?]
---
```

---

## Seção B — Prompt de INÍCIO de projeto (colar no chat)

```
Leia os arquivos de planejamento BMAD que enviei junto com o KERNO-DOCS.md.

Os documentos de planejamento já estão prontos (feitos pelo BMAD Method).
NÃO recrie nenhum deles. Seu papel é IMPLEMENTAR o que foi planejado.

Use o Plan Mode e faça:

1. Leia e analise todos os documentos recebidos:
   - prd.md (requisitos do produto)
   - prd-milestones.md (milestones já definidos)
   - architecture.md (stack e decisões técnicas)
   - features.md (lista de features priorizada)
   - database.md (schema planejado)
   - Se existirem: wds-design-system.md, wds-ui-roadmap.md (specs de design)

2. VALIDE os milestones contra a Árvore de Dependências do Custom Knowledge.
   Se alguma dependência estiver fora de ordem, aponte e sugira correção.

3. Crie APENAS os documentos operacionais que ainda não existem:
   - /docs/ui-components.md (estrutura inicial)
   - /docs/flows.md (estrutura inicial)
   - /docs/changelog.md (v0.1.0 — início do projeto)
   - /docs/handover.md (estrutura inicial)
   - /docs/security-rules.md (baseado no SECURITY.md do Kerno)
   - /docs/documentation-rules.md
   - /docs/session-close.md (estrutura)
   - /docs/delivery-report.md (estrutura)
   - /docs/security-report.md (estrutura)

4. Apresente o plano de implementação da Milestone 1 para minha aprovação
   ANTES de implementar qualquer código.

Não implemente nada ainda. Só valide, crie os docs operacionais, e me mostre o plano.
```

---

## Seção C — Prompt para APROVAR e implementar

```
O plano está aprovado. Implemente a Milestone 1 agora.

Lembrete:
- Siga a Árvore de Dependências (auth → layout → dados → features)
- Use o design do wds-design-system.md se existir
- Mobile-first
- RLS em toda tabela
- Valide forms com Zod
- Atualize /docs a cada passo
- Ao final, me diga para fixar a versão
```

---

## Seção D — Prompt para RETOMAR sessão

```
Retomando sessão. Contexto:

- Última sessão: [N] em [DATA]
- Milestone atual: [N] — [X]% concluída
- Pendências: [copie do session-close.md]

Continue a implementação. Siga a ordem de dependências
do Custom Knowledge. Atualize /docs conforme o KERNO-DOCS.md.
```

---

## Seção E — Prompt para FECHAR sessão

```
Fechar sessão.

1. Atualize /docs/session-close.md com:
   - Tudo que foi feito nesta sessão
   - Arquivos criados/modificados
   - Estado atual (milestone, % concluído)
   - Pendências para próxima sessão
   - Docs atualizados
2. Atualize /docs/changelog.md
3. Me diga exatamente o que colar no próximo prompt para retomar

Formato:
---
🔒 Sessão [N] encerrada — [DATA]

✅ Feito:
- [lista]

⏳ Pendente:
- [lista]

📌 Versão estável: [recomendar fixar? sim/não]

▶️ Para retomar, cole:
"[prompt pronto para copiar]"
---
```

---

## Seção F — Prompt para FECHAR milestone

```
Fechar Milestone [N].

1. Gere /docs/delivery-report.md:
   - Escopo planejado vs entregue
   - Métricas (features planejadas/entregues/bugs)
   - Problemas encontrados e soluções
2. Atualize /docs/handover.md
3. Atualize /docs/prd-milestones.md (marcar milestone como concluída)
4. Atualize /docs/features.md
5. Atualize /docs/changelog.md

📌 FIXE esta versão antes de iniciar a próxima milestone.

Apresente o resumo e prepare o plano da Milestone [N+1].
```

---

## Seção G — Prompt para REVISÃO DE SEGURANÇA (antes do deploy)

```
Execute uma revisão completa de segurança do projeto.

Verifique e reporte:
1. RLS ativo em TODAS as tabelas (liste cada tabela e status)
2. service_role ausente do frontend (busque em todos os arquivos src/)
3. Todos os formulários validam com Zod
4. Nenhum dangerouslySetInnerHTML sem DOMPurify
5. Nenhum dado sensível em localStorage
6. Todos os { data, error } do Supabase tratados
7. Nenhum console.log com dados sensíveis
8. Headers de segurança no vercel.json
9. Variáveis de ambiente corretas (nenhuma chave hardcoded)
10. Nenhuma interpolação em filtros Supabase

Atualize /docs/security-report.md com os resultados.
Para cada problema encontrado, corrija imediatamente.
```

---

## Notas sobre o fluxo Kerno v5.0

### O que mudou do v2.0 para v3.0

| Aspecto | v2.0 (Lovable sozinho) | v3.0 (BMAD + Lovable) |
|---------|----------------------|----------------------|
| PRD | Usuário criava manualmente | BMAD John (PM) cria profissionalmente |
| Milestones | Lovable criava no 1º prompt | BMAD John define antes |
| Arquitetura | Lovable decidia durante dev | BMAD Winston planeja antes |
| Design | Lovable decidia sozinho | WDS Freya/Idunn especificam antes |
| Testes | Plano básico do Lovable | BMAD Murat define estratégia antes |
| Nomenclatura | SCREAMING_SNAKE_CASE | kebab-case (padrão BMAD) |
| Lovable | Planejava E implementava | Apenas IMPLEMENTA e MANTÉM docs |

### Fluxo completo no Lovable (Kerno Seize v6.0.3)

1. **Custom Knowledge** (Settings → Knowledge) → Cole a Seção A
2. **Upload** → Todos os documentos BMAD (prd.md, architecture.md, etc.) + KERNO-DOCS.md
3. **Seção B** → Lovable valida, cria docs operacionais, apresenta plano
4. **Seção C** → Aprovar e implementar Milestone 1
5. **Cada sessão** → Seção D (retomar) e Seção E (fechar)
6. **Cada milestone** → Seção F (fechar milestone)
7. **Antes do deploy** → Seção G (revisão de segurança)
