# Etapa 5 — Revisão e Qualidade no Cursor

> **Versão:** 1.2 (Kerno Seize v6.0.5)
> **Quando usar:** Após cada milestone finalizada no Lovable (Etapa 4)
> **Ferramentas:** Cursor + Claude Code + BMAD TEA (Murat)

---

## Visão Geral

A Etapa 5 acontece DEPOIS que o Lovable termina uma milestone. Você clona o repositório no Cursor e usa Claude Code para revisar código, segurança, performance e cobertura de testes.

O TEA (Murat) atua aqui pela **segunda vez** — a primeira foi na Etapa 1, quando criou o `qa-plan.md`. Agora ele valida se o que foi implementado cumpre o plano original.

---

## Workflow Completo

### 5.1 Preparação

```bash
# Clonar o repositório
git clone <url-do-repo>
cd <nome-do-projeto>

# Instalar dependências
npm install

# Verificar que roda localmente
npm run dev
```

### 5.2 Revisão de Segurança (Claude Code)

Cole no terminal do Claude Code:

```
Faça uma revisão completa de segurança deste projeto.

Verifique:
1. RLS ativo em TODAS as tabelas Supabase (liste cada uma)
2. service_role ausente de todo o frontend (src/)
3. Formulários validados com Zod
4. Nenhum dangerouslySetInnerHTML sem DOMPurify
5. Nenhum dado sensível em localStorage
6. Todos os { data, error } do Supabase tratados
7. Nenhum console.log com dados sensíveis
8. Headers de segurança no vercel.json
9. Variáveis de ambiente corretas (sem hardcoded)
10. Nenhuma interpolação em filtros Supabase (.or(), .filter())

Corrija cada problema encontrado e atualize /docs/security-report.md.
```

### 5.3 Revisão de Código (Claude Code)

```
Revise a qualidade do código deste projeto.

Verifique:
1. TypeScript estrito (sem "any" para dados de usuário)
2. Componentes seguem padrão consistente
3. Hooks customizados bem extraídos (sem lógica repetida)
4. Error boundaries implementados
5. Loading/empty/error states em todos os componentes de dados
6. Acessibilidade básica (labels, alt text, keyboard nav)
7. Performance (lazy loading, memoização onde necessário)
8. Mobile-first (responsivo em todos os breakpoints)

Para cada problema, corrija e explique a mudança.
```

### 5.4 Validação TEA (Murat — 2ª intervenção)

```
Leia o /docs/qa-plan.md que foi criado pelo BMAD na fase de planejamento.

Compare o que foi planejado vs o que foi implementado:
1. Quais cenários de teste do qa-plan.md foram cobertos?
2. Quais cenários estão pendentes?
3. A priorização por risco (P0-P3) foi respeitada?
4. Alguma feature crítica (P0) está sem teste?

Atualize o /docs/qa-plan.md com o status real de cada cenário.
Sugira testes automatizados para as features P0 e P1.
```

### 5.5 Revisão de Documentação

```
Verifique se a documentação está completa e atualizada.

Para cada doc em /docs/:
1. Está consistente com o código atual?
2. Há features implementadas que não estão documentadas?
3. Há features documentadas que não foram implementadas?
4. O changelog.md reflete todas as mudanças?

Corrija inconsistências encontradas.
```

### 5.6 Push das Correções

```bash
# Criar branch de revisão
git checkout -b review/milestone-N

# Add e commit
git add .
git commit -m "review: correções Etapa 5 — segurança, qualidade, testes"

# Push
git push origin review/milestone-N

# Criar Pull Request no GitHub
# O pipeline SAST vai rodar automaticamente no PR
```

---

## Checklist Rápido

| # | Verificação | Status |
|---|-------------|--------|
| 1 | RLS ativo em todas as tabelas | ⬜ |
| 2 | service_role fora do frontend | ⬜ |
| 3 | Zod em todos os formulários | ⬜ |
| 4 | Error handling em queries Supabase | ⬜ |
| 5 | Sem secrets hardcoded | ⬜ |
| 6 | Headers de segurança no vercel.json | ⬜ |
| 7 | TypeScript estrito (sem any) | ⬜ |
| 8 | Loading/empty/error states | ⬜ |
| 9 | Mobile responsivo | ⬜ |
| 10 | qa-plan.md P0/P1 cobertos | ⬜ |
| 11 | changelog.md atualizado | ⬜ |
| 12 | Pipeline SAST passou | ⬜ |

---

## Quando pular para a Etapa 6

Avance para Deploy (Etapa 6) quando:
- Todos os itens P0 do checklist estão ✅
- Pipeline SAST passou sem findings críticos ou altos
- qa-plan.md mostra cobertura ≥ 80% em features P0-P1
- security-report.md não tem pendências críticas
