# Kerno Framework — Changelog

> Histórico completo de versões. Cada versão documenta o que foi adicionado, modificado ou removido.

---

## [6.1.1] — 2026-03-18

### Corrigido
- `KERNO-RETROFIT.html` — Fase 1, passo 1.2: nome do arquivo de diagnóstico alterado de `diagnostico-seguranca.md` para `diagnostico-seguranca-DDMMAA-vX.md` (ex: `diagnostico-seguranca-180326-v1.md`). Evita sobrescrever relatórios anteriores quando o usuário roda a auditoria múltiplas vezes. Alterações feitas em: título do passo, prompt de instrução ao Claude/GPT, instruções pós-resposta, prompt do Lovable para salvar o arquivo, referência no passo 1.4 e checklist.

---

## [6.1.0] — 2026-03-18

### Modificado
- `KERNO-RETROFIT.html` — Fase 1 completamente reescrita com sistema de **triagem 🅰️/🅱️/🅾️** (quebra o loop circular onde problemas de histórico Git nunca podiam ser resolvidos pelo Lovable):
  - **1.1**: rodar pipeline + tabela explicando Semgrep/npm audit/Scanner Lovable + baixar Artifacts
  - **1.2** (novo): triagem com Claude/GPT — prompt gera `diagnostico-seguranca.md` com 3 categorias: 🅰️ Lovable resolve (código), 🅱️ Ação manual (rotacionar chaves, `npm audit fix`, verificar RLS), 🅾️ Ignorar (falsos positivos: libs de terceiros, páginas públicas intencionais)
  - **1.3** (novo): executar ações 🅱️ manualmente — rotacionar chaves Supabase, rodar `npm audit fix`, verificar RLS no painel
  - **1.4** (novo): aplicar prompts Lovable um a um para itens 🅰️, com fixação de versão
  - **1.5** (novo): re-rodar pipeline + explicação de que histórico Git ❌ é esperado/aceitável após rotação de chaves
  - Checklist atualizado de 4 linhas (1.1–1.4) para 5 linhas (1.1–1.5)

### Adicionado (análise)
- Identificação de **falsos positivos recorrentes** nos relatórios do projeto hexapago-2.0:
  - `chart.tsx` `dangerouslySetInnerHTML` — interno da biblioteca Recharts, não controlado pelo usuário
  - Páginas sem autenticação intencionalmente públicas (links de pagamento, docs)
  - 9 vulnerabilidades npm todas em dependências de desenvolvimento (não afetam produção)
  - `.env no histórico Git` e `service_role no histórico Git` — não resolvíveis pelo Lovable; solução: rotação de chaves + categoria 🅱️
- Documentado: pipeline mostrará ❌ persistente para histórico Git mesmo após todas as ações corretas — isso é comportamento esperado e documentado, não um problema pendente

---

## [6.0.5] — 2026-03-18

### Modificado
- `KERNO-RETROFIT.html` — Fase 1, passo 1.2 reescrito com fluxo correto em 2 etapas:
  1. Claude/GPT analisa os 3 arquivos anexados e **gera o `diagnostico-seguranca.md`** (o usuário baixa o arquivo gerado — sem copy-paste de conteúdo extenso no prompt)
  2. No Lovable, o usuário **anexa o `.md` gerado** para salvá-lo no projeto (`/docs/retrofit/`) — evita o limite de tamanho do prompt
- `CLAUDE.md` — adicionada seção `## Regras de versionamento` (patch, minor, major) com critérios objetivos para incremento de versão

---

## [6.0.4] — 2026-03-18

### Modificado
- `KERNO-RETROFIT.html` — Fase 1, passo 1.2: separação entre análise (Claude/GPT externo, sem acesso ao projeto) e criação do arquivo (Lovable). O prompt de análise não pede mais para "salvar em /docs/..." — apenas gera os prompts de correção. Um segundo prompt separado instrui o Lovable a criar o `diagnostico-seguranca.md` com o conteúdo copiado da resposta do Claude/GPT.

---

## [6.0.3] — 2026-03-18

### Modificado
- `KERNO-RETROFIT.html` — Fase 1, passo 1.2: instrução de diagnóstico migrada para usar **anexo de arquivos** (📎) em vez de copy-paste do conteúdo nos relatórios JSON/TXT/HTML. O usuário baixa os 3 Artifacts e os anexa diretamente no Claude/GPT — evita limites de tamanho do prompt.

---

## [6.0.2] — 2026-03-18

### Modificado
- `KERNO-RETROFIT.html` — Fase 1 reestruturada:
  - **1.1**: adicionada tabela explicando o que cada ferramenta (Semgrep, npm audit, Scanner Lovable) analisa e o que significa ✅/❌. Adicionada instrução para baixar os 3 arquivos de Artifacts ao final do pipeline.
  - **1.2**: fluxo migrado do Lovable para o **Claude/GPT externo**. O usuário anexa os 3 relatórios → Claude/GPT interpreta os resultados e gera sequência de prompts de correção priorizados (🔴/🟠/🟡) → salva em `diagnostico-seguranca.md`. Lovable não é adequado para interpretar relatórios de segurança.
  - Checklist da Fase 1 atualizado para refletir novo fluxo.

---

## [6.0.1] — 2026-03-18

### Corrigido
- `KERNO-RETROFIT.html` — Fase 1, passo 1.1: nome do workflow corrigido de "Security Audit" para "Lovable Security Audit" (nome real exibido na aba Actions do GitHub).

---

## [6.0.0] — 2026-03-18

### Corrigido
- KERNO-RETROFIT.html — Fase 1 (Auditoria de Segurança) reescrita completamente:
  - **1.1**: adicionada instrução para rodar o pipeline após criar os arquivos (antes só criava sem executar, mas o 1.4 pedia para "rodar novamente" algo que nunca rodou)
  - **1.2**: prompt agora pede ao Lovable para gerar os prompts de correção prontos na ordem certa (antes mandava o usuário ao Claude/ChatGPT para interpretar — etapa desnecessária que exigia expertise)
  - **1.3**: fluxo simplificado para copiar e colar cada prompt do 1.2, com ciclo fixar → colar → testar → fixar (antes o usuário escrevia o prompt manualmente usando template genérico)
  - **1.4**: validação via prompt ao Lovable que compara diagnóstico vs estado atual (antes pedia para "baixar relatório" e "comparar" — sem instrução de como e sem expertise para interpretar)
  - Aviso de fixar versão movido para o topo da fase (antes ficava no final do 1.3, o usuário já tinha feito alterações sem fixar)

---

## [5.5.2] — 2026-03-18

### Corrigido
- Custom Knowledge do Lovable — o fluxo anterior pedia para colar 4 arquivos (~55K chars) num campo com limite de 10K. Criado **KERNO-KNOWLEDGE.md** (4KB) como arquivo único compacto. Todas as referências a "Seção A no Knowledge" atualizadas em 7 arquivos.

### Modificado
- LOVABLE-SYSTEM-PROMPT-V3.md — versão `3.2` → `3.3`. Seção A reclassificada como referência expandida.
- index.html — seção Knowledge simplificada: 1 arquivo em vez de 4.
- KERNO-RETROFIT.html — Passo 0.3, 4.3, seção "E os agentes BMAD + PM Skills?" atualizados.
- KERNO-SECURITY-TUTORIAL.html — rodapé atualizado para v5.5.2.
- KERNO-RESPOSTA-INCIDENTES.html — rodapé atualizado para v5.5.2.
- KERNO-MONITOR-TUTORIAL.html — rodapé atualizado para v5.5.2.

### Adicionado
- `KERNO-KNOWLEDGE.md` (novo) — arquivo único compacto (4KB) para o Custom Knowledge do Lovable.

---

## [5.5.1] — 2026-03-17

### Corrigido
- KERNO-RETROFIT.html — Passos 0.2 e 1.2 pediam para criar arquivo em Plan Mode (impossível — Plan Mode do Lovable é somente leitura). Corrigido: cada passo agora tem Passo A (análise em Plan Mode) e Passo B (criação do arquivo com Plan Mode desativado).

---

## [5.5.0] — 2026-03-17

### Mudança Fundamental
- **Integração PM Skills Marketplace + AI PM Copilot**: O framework ganha duas novas camadas de product management. PM Skills (65 skills, 36 workflows, 8 plugins — MIT) entra como Fase 1 obrigatória antes do BMAD, cobrindo discovery, estratégia, value proposition, competitive analysis, North Star Metric e ciclo contínuo pós-launch. AI PM Copilot (slgoodrich/agents — PolyForm Noncommercial) entra em 3 pontos como gates de qualidade: validation-sprint (Gate 0), PRD stress test (Gate 1), e context-scanner (auditoria mensal).
- **CIS sai do caminho recomendado**: Carson/Maya/Victor deixam de ser o ponto de entrada de ideação. PM Skills `/discover` e AI PM Copilot `validation-sprint` cobrem o mesmo terreno com mais profundidade. CIS permanece disponível no BMAD para uso direto.
- **Framework de documentação expandido de 23 para 30 documentos**: 5 novos documentos PM Skills (Grupo F: pm-discovery-plan, pm-strategy-canvas, pm-value-proposition, pm-competitive-analysis, pm-north-star) + 2 novos documentos AI PM Copilot (Grupo G: pm-validation-verdict, pm-prd-stress-test).
- **Duas trilhas de implementação formalizadas**: com Lovable (trilha vibe coder, 6 etapas) e sem Lovable (trilha desenvolvedor, Claude Code direto, 4 etapas).

### Adicionado
- `KERNO-PM-INTEGRACAO.md` (novo) — documento completo de integração: o que muda, instalação, documentos adicionados, fluxo atualizado, impacto em cada arquivo, notas de licença. 11 seções.
- Grupo F no KERNO-DOCS.md — 5 documentos PM Skills (`/docs/pm-*.md`)
- Grupo G no KERNO-DOCS.md — 2 documentos AI PM Copilot (gates)
- Flags no CLAUDE.md: `pm-skills-enabled`, `pm-discovery-done`, `pm-strategy-done`, `pm-north-star-defined`, `aipm-copilot-enabled`, `aipm-prd-stress-test`
- Fase 0 (validação) e Fase 1 (PM Skills) no workflow do CLAUDE.md
- Gate 1 (PRD stress test) entre John e Winston no fluxo
- Parte 0 no KERNO-SEIZE-INICIO.md — instalação e sequência PM Skills antes do BMAD
- Lista de docs PM Skills na Seção A do LOVABLE-SYSTEM-PROMPT-V3.md

### Modificado
- CLAUDE.md — versão `5.3.2` → `5.5.0`. YAML expandido com flags PM. Workflow expandido com Fases 0-1. Estrutura `/docs` expandida com Grupos F e G.
- KERNO-DOCS.md — versão `2.2` → `2.3`. Grupos F e G adicionados. Total de documentos: 23 → 30.
- KERNO-SEIZE-INICIO.md — Parte 0 adicionada antes da Parte 1 (Instalação BMAD). Gates 0 e 1 documentados com prompts exatos.
- LOVABLE-SYSTEM-PROMPT-V3.md — versão `3.1` → `3.2`. Seção A: docs PM Skills na lista de "documentos que podem existir".
- KERNO-ROADMAP.html — Passo 0.0 (PM Skills) adicionado antes do CIS. CIS marcado como legacy.
- KERNO-FLUXO.html — Trilha A inclui PM Skills como primeira fase. Nota sobre trilha sem Lovable.
- KERNO-CURSOR-REVIEW.md — versão `1.1` → `1.2`.
- KERNO-RETROFIT.html — referência ao context-scanner para produto existente.
- index.html — Versão 5.4 → 5.5. Referências PM Skills e AI PM Copilot adicionadas. Contagem 21 → 22 arquivos.

### Análise / Motivação
- O BMAD é forte em especificação técnica (fases 0→3) e governança. Mas assume que o usuário já sabe O QUE construir e PARA QUEM — não valida a ideia, não articula trade-offs estratégicos, não define métricas. O PM Skills preenche exatamente esse gap com frameworks consagrados (Teresa Torres, Cagan, Savoia, Olsen).
- O CIS (Carson/Maya/Victor) fazia brainstorming e análise de mercado, mas de forma menos estruturada que o PM Skills. O `/discover` sozinho entrega brainstorm + assumptions + priorização + experiments com critério de sucesso quantificado — o CIS não chega nesse nível.
- O AI PM Copilot é 70-80% redundante com PM Skills, mas traz 3 capacidades exclusivas: agent teams com cross-examination (validation-sprint, prd-stress-test) e context-scanner automático. Essas 3 peças entram como gates de qualidade, não como fluxo principal.
- A licença PolyForm Noncommercial do AI PM Copilot exige documentação explícita de que uso comercial requer acordo separado.

---

## [5.4.0] — 2026-03-11

### Problema resolvido
O framework havia crescido para 28 arquivos de forma iterativa, sem sessões de curadoria. O resultado: conteúdo triplicado sem hierarquia clara, arquivos de processo interno misturados com guias de uso, duas versões do system prompt ativas ao mesmo tempo, e dois guias cobrindo o mesmo fluxo com estruturas diferentes. O usuário não conseguia distinguir o que era essencial do que era histórico.

### Arquivado (movido para /archive — não deletado)
- `KERNO-ANALISE-WORKFLOW-v5.html` — documento de processo interno de design do framework, não serve ao usuário final
- `KERNO-SEIZE-MAPEAMENTO.html` — diagnóstico de fusão Kerno × BMAD, idem
- `LOVABLE-SYSTEM-PROMPT-V2.md` — versão anterior do V3, gerava confusão sobre qual usar
- `KERNO-GUIA-PASSO-A-PASSO.html` — cobria o mesmo terreno que KERNO-ROADMAP.html com estrutura diferente
- `KERNO-INICIO.html` — fluxograma de decisão substituído funcionalmente pelo KERNO-FLUXO.html

### Modificado
- `index.html` — reescrito do zero. Deixou de ser lista de 28 arquivos e virou hub com dois caminhos claros: "Tenho um projeto existente" (→ KERNO-RETROFIT.html) e "Vou começar um projeto novo" (→ KERNO-FLUXO.html). Seção de arquivos que vão no Lovable (Custom Knowledge) e no GitHub separadas visualmente. Referências e tutoriais no final, não no topo.

### Resultado
Framework passou de 28 para 21 arquivos ativos (5 arquivados). Hierarquia clara: index define os dois caminhos → guias de trilha definem os passos → SECURITY.md é a fonte de verdade de segurança → tutoriais são consulta opcional.

---

## [5.3.4] — 2026-03-07

### Problema resolvido
Projetos que integram LLM (chatbot, assistente, agentes) não tinham nenhuma cobertura de segurança específica para os vetores de ataque da camada de IA: prompt injection, system prompt leakage, insecure output handling, excessive agency, resource exhaustion.

### Adicionado

- `LLM-SECURITY.md` (novo) — complemento ao SECURITY.md para projetos com LLM exposto ao usuário. Cobre 8 seções: system prompt seguro (servidor, não frontend), prompt injection, validação de input antes do LLM, autorização antes de qualquer chamada ao LLM, rate limiting com tabela ai_usage_logs, minimização de dados sensíveis enviados ao modelo, tratamento seguro do output (sem eval(), DOMPurify obrigatório), e princípio de mínimo privilégio para agentes. Upload obrigatório no Custom Knowledge do Lovable junto ao SECURITY.md quando o projeto tiver LLM.

### Modificado

- `MAPEAMENTO-FERRAMENTAS.html` — reformulado com dois cenários via abas: "Projetos sem LLM exposto" (mapeamento original preservado) e "Projetos com LLM exposto ao usuário" (novo). Novas entradas: LLM-SECURITY.md (🟣 USANDO), NeMo Guardrails (🟣 DISPONÍVEL), promptmap (📋 pré-deploy), Lakera Guard e langkit (📋 fase futura). promptfoo e garak migrados de "não se aplica" para "com expertise".
- `index.html` — LLM-SECURITY.md adicionado ao grupo de Segurança com badge NOVO; contagem 27 → 28 arquivos.

---

## [5.3.3] — 2026-03-07

### Problema resolvido
O framework tinha conteúdo de qualidade mas nenhum fio condutor. O usuário sabia *que* documentos existiam mas não *quando abrir cada um*, *como eles se relacionam*, ou *o que fazer depois de terminar uma fase*. O problema mais agudo: "terminei o Retrofit — e agora?". Não havia resposta visível em lugar nenhum.

### Adicionado

- `KERNO-FLUXO.html` (novo) — **Mapa GPS do framework**: seletor de trilha (A/B/C), etapas em sequência com links diretos, botões "marcar como feito", progresso salvo no navegador, gates de qualidade visíveis, bloco de transição "Terminei o Retrofit — e agora?", bridge de migração Lovable-First → BMAD.

### Modificado

- `KERNO-RETROFIT.html`, `KERNO-ROADMAP.html`, `KERNO-LOVABLE-FIRST.html`, `KERNO-SEIZE-AUDITORIA.html` — barra de navegação contextual sticky adicionada no topo: link permanente ao Mapa do Fluxo + breadcrumb da trilha atual + "Próximo: [documento] →" no canto direito.
- `index.html` — botão principal redirecionado para `KERNO-FLUXO.html`; arquivo adicionado ao índice; contagem 26 → 27.

---

## [5.3.2] — 2026-03-06

### Motivação
A Fase 2 do Retrofit assumia que o usuário já tinha a Vercel configurada — o que é incorreto. O deploy padrão do Lovable é o Lovable Cloud (`.lovable.app`), e a Vercel é uma etapa ativa que o usuário precisa fazer manualmente via GitHub. Além disso, todas as configurações da fase eram tabelas técnicas sem contexto — o usuário não sabia o que era cada item, para que servia, nem o que fazer se algo não estivesse como esperado.

### Modificado

- `KERNO-RETROFIT.html` — Fase 2 reescrita completamente:
  - **Passo 2.0 — Diagnóstico de situação:** dois cards visuais (Situação A: só tem preview Lovable / Situação B: já está na Vercel) com critério objetivo para o usuário identificar onde está sem precisar de conhecimento técnico
  - **Caminho A** (Lovable Cloud → Vercel): sequência de 4 passos — conectar GitHub, criar vercel.json via prompt, importar na Vercel com instrução de onde encontrar as variáveis do Supabase, validar que está funcionando. Nota explícita: o Lovable Cloud continua ativo como backup, não precisa excluir nada.
  - **Caminho B** (já na Vercel): verificação de variáveis de ambiente + prompt para atualizar vercel.json — sem tabelas técnicas
  - **Passo 2.1 — Supabase URLs:** substituído por prompt que pede ao Lovable para identificar o problema e dar as instruções exatas de onde clicar — o usuário não precisa saber o que são "redirect URLs"
  - **Passo 2.2 — GitHub:** tabela de configurações substituída por lista com descrição em linguagem simples do que cada proteção faz
  - **Passo 2.3 — Validação:** prompt de confirmação via Lovable substitui checklist manual técnico
  - Checklist final atualizado com novos itens 2.0, 2.A, 2.B, 2.1, 2.2, 2.3

---

## [5.3.1]— 2026-03-06


### Princípio central estabelecido
Adotado formalmente o princípio **"usuário não precisa de competência técnica"** como regra de design do framework. Todo passo deve: (1) ter o Lovable ou agente BMAD fazendo o diagnóstico, (2) gerar um arquivo documentando o resultado, (3) indicar onde buscar ajuda se o resultado for confuso.

### Modificado

- `KERNO-RETROFIT.html`:
  - **Princípio não-técnico** adicionado como bloco de destaque logo no início do documento (antes da seção Retrofit vs. Reconstrução) — declara explicitamente que o usuário não precisa tomar decisões técnicas
  - **Passo 0.2** — "Fotografar o estado atual": prompt atualizado para criar automaticamente `/docs/retrofit/raio-x.md`; instrução "Salve essa resposta" removida (era uma ação manual desnecessária)
  - **Passo 1.2** — renomeado de "Verificação manual do Supabase (CRÍTICO)" para "Diagnóstico de segurança via Lovable": tabela de verificação manual eliminada; substituída por prompt completo que faz o diagnóstico automaticamente, classifica os problemas com 🔴/🟠/🟡 e cria `/docs/retrofit/diagnostico-seguranca.md`; adicionada instrução explícita de como usar o arquivo com Claude/ChatGPT se o resultado for confuso
  - **Passo 1.3** — atualizado para referenciar `diagnostico-seguranca.md` como fonte de correções
  - **Checklist final** — itens 0.2 e 1.2 atualizados com nomes de arquivos e descrições corretas

### Análise / Motivação
- "Salve essa resposta" exige que o usuário saiba copiar, colar e criar um arquivo no lugar certo. É uma etapa invisível que muitos pulam, perdendo o diagnóstico inicial. O prompt agora cria o arquivo automaticamente.
- A verificação manual de RLS no painel do Supabase exige saber o que é RLS, o que são policies, o que significa `auth.uid() = user_id`, e como avaliar se uma configuração é segura. Nenhum desses é conhecimento que podemos assumir. O Lovable faz tudo isso e entrega uma classificação de risco já interpretada.
- O padrão "resultado confuso → copie e pergunte ao Claude/ChatGPT" cria um mecanismo de escape para qualquer passo do framework sem exigir que o usuário aprenda o conteúdo técnico por trás.

---

## [5.3.0] — 2026-03-06
 — revisão inteligente pós-Lovable com governança mensal. Os agentes BMAD (Winston, Bob, Mary, John) passam a atuar no Retrofit não apenas como ferramentas de planejamento, mas como camada de controle de qualidade de tudo que foi gerado. Inclui sistema de histórico por data e rastreamento de evolução entre rodadas.

### Adicionado

- `KERNO-SEIZE-AUDITORIA.html` (novo, ~950 linhas) — módulo completo com:
  - **Fase A0:** inicialização — cria pasta datada `/docs/audit/YYYY-MM-DD/`, verifica histórico anterior, cria `AUDIT-HISTORY.md`
  - **Fase A1 — Winston:** auditoria de arquitetura. Cria `audit-estrutura.md` automaticamente
  - **Fase A2 — Bob:** auditoria de qualidade de código. Cria `audit-codigo.md` automaticamente
  - **Fase A3 — Mary:** auditoria de produto + UX (jornadas, PRD, stories). Cria `audit-produto-ux.md` automaticamente
  - **Fase A4 — John:** análise de drift docs vs. código. Cria `audit-drift.md` automaticamente
  - **Fase A5:** orquestrador consolida os 4 relatórios em `RELATORIO-MESTRE-AUDITORIA.md` e atualiza `AUDIT-HISTORY.md` com métricas
  - **Fase A5-B:** John compara auditoria atual com a anterior — itens resolvidos, recorrentes e regressões. Cria `audit-comparacao.md`. A partir da 2ª rodada.
  - **Fase A6:** prompts cirúrgicos para aplicar correções no Lovable uma a uma
  - Governança mensal: pastas datadas preservam histórico completo; `AUDIT-HISTORY.md` como índice de todas as rodadas
  - Todos os prompts criam os arquivos automaticamente — zero ação manual do usuário

### Modificado

- `KERNO-RETROFIT.html` — Fase 4.5 adicionada ao fluxo: bloco visual com os 4 agentes, link para `KERNO-SEIZE-AUDITORIA.html`; checklist atualizado (item 4.2, tempo total ~3h–4h30); Fase 5.2 inclui auditoria mensal; seção "Agentes BMAD" reescrita
- `index.html` — `KERNO-SEIZE-AUDITORIA.html` adicionado com badge NOVO; contagem 25 → 26 arquivos; versão e zip atualizados
- `CLAUDE.md` — Grupo E adicionado à estrutura `/docs` (pasta `audit/`); Etapa 5 do workflow expandida; `kerno-version: 5.3.2`
- Todos os arquivos — versão global `5.2.4` → `5.3.2`

### Análise / Motivação
- Gerar documentação sem auditá-la é metade do trabalho. O Lovable cria o que foi pedido da forma que entendeu — frequentemente com componentes de 300+ linhas, jornadas com dead ends, PRD que não reflete o código, e 30–40% de código redundante. A Auditoria BMAD fecha esse gap antes de escalar.
- A governança mensal com pastas datadas resolve o problema de sobrescrever relatórios anteriores. Com `AUDIT-HISTORY.md` e a Fase A5-B, o usuário enxerga em minutos se o projeto está melhorando, se há dívida técnica recorrente ignorada, ou se houve regressões.
- Prompts com criação automática de arquivos eliminam a dependência de memória do usuário — o risco de "esquecer de salvar" foi removido by design.

---

## [5.2.4] — 2026-03-06

### Corrigido
- `KERNO-RETROFIT.html` — bug crítico no prompt de diagnóstico: checkboxes `[ ]`, verbos imperativos ("CALCULE", "Apresente", "some pontos") e setas `→` estavam sendo interpretados pelo Lovable como instruções de execução, causando modificações indesejadas no código do projeto
- Prompt reescrito com linguagem 100% descritiva e declarativa: critérios nomeados como "Item S1…S7 / R1…R6", pontuação descrita como consequência ("Verdadeiro: 15 pontos"), sem checkboxes, sem imperativos, sem setas
- Adicionado "MODO LEITURA APENAS" no início e "LEMBRETE FINAL" no fim do prompt como barreiras explícitas contra execução

### Análise / Motivação
- O Lovable não distingue entre "critérios a avaliar" e "tarefas a executar" quando o prompt usa sintaxe de checkboxes e verbos de ação. A reformulação descritiva elimina esse vetor de ambiguidade.

---

## [5.2.3] — 2026-03-06

### Corrigido
- Varredura completa identificou 14 ocorrências de `v5.1` em 7 arquivos — todas corrigidas para `v5.2`
- `CLAUDE.md` — `kerno-version: 5.1 → 5.2` + texto do header
- `KERNO-DOCS.md` — versão interna `2.1 → 2.2`
- `KERNO-SEIZE-INICIO.md` — rodapé `v5.1 → v5.2`
- `KERNO-INICIO.html` — badge `v5.1 → v5.2`
- `KERNO-LOVABLE-FIRST.html` — 2 ocorrências (ver + footer)
- `KERNO-RETROFIT.html` — badge e footer
- `KERNO-ROADMAP.html` — badge e footer
- `LOVABLE-SYSTEM-PROMPT-V3.md` — título, header `3.1 → 3.2`, 2 referências inline

### Análise / Motivação
- Referências de versão desatualizadas em documentos colados no Lovable (como o System Prompt) criam confusão sobre qual versão do framework está ativa no projeto. A varredura sistemática garante consistência total.

---

## [5.2.2] — 2026-03-06

### Modificado
- `index.html` — removido grupo "Análise e Mapeamento BMAD" do índice: `KERNO-SEIZE-MAPEAMENTO.html` e `KERNO-ANALISE-WORKFLOW-v5.html` deixam de aparecer na vitrine (arquivos mantidos no pacote para não quebrar links existentes)
- `KERNO-CURSOR-REVIEW.md` — versão interna `1.0 (Kerno Seize v5.1)` → `1.1 (Kerno Seize v5.2)`
- `KERNO-RETROFIT.html` — prompt de diagnóstico substituído por scorecard com autopontuação (7 critérios de saúde + 6 critérios de risco + 3 faixas de veredicto automático)

### Análise / Motivação
- `KERNO-SEIZE-MAPEAMENTO.html` e `KERNO-ANALISE-WORKFLOW-v5.html` são documentos de processo interno, não de uso. Expô-los no índice confunde o usuário com informação que não é acionável para ele.
- O prompt de diagnóstico original exigia que o usuário interpretasse as respostas e tomasse a decisão — trabalho que o Lovable pode fazer automaticamente.

---

## [5.2.1] — 2026-03-06

### Adicionado
- `index.html` — recriado completamente para v5.2: nova seção "Por onde começar" com as duas trilhas (BMAD vs. Lovable-First); roadmap com WDS em 2 ondas e 5 gates; índice inclui `KERNO-LOVABLE-FIRST.html` e `KERNO-SEIZE-INICIO.md`; stats corrigidos (11 agentes, 5 gates); download aponta para `kerno-framework-v5.2.zip`

### Corrigido
- `KERNO-INICIO.html` — botão "Ver sequência dos sprints" inacessível: conteúdo do canvas terminava atrás da barra de navegação inferior fixa; corrigido com `padding-bottom: 140px` no `.canvas-wrap` e `padding-bottom: 120px` no `body`

### Análise / Motivação
- O `index.html` do pacote v5.2 inicial era o arquivo original v5.1, sem as atualizações dos 4 sprints. Recriação completa alinha o hub de entrada com o estado real do framework.
- O bottombar fixo de ~100px sobrepunha o último elemento do fluxograma com apenas 48px de espaço reservado.

---

## [5.2] — 2026-03-06

### Resumo
Ciclo de 4 sprints executados em sessão contínua. Três eixos principais: **governança** (gates de qualidade + WDS em duas ondas), **onboarding** (tutoriais de apoio), e **acessibilidade** (trilha Lovable-First para quem quer pular o BMAD). Nenhuma mudança de identidade do framework — maturidade incremental da v5.x.

### Adicionado

**Sprint 2 — Tutorial de onboarding BMAD**
- `KERNO-SEIZE-INICIO.md` (612 linhas) — tutorial completo do zero ao primeiro prompt no Lovable: instalação (`npx bmad-method install`), tabela dos 11 agentes, prompts exatos copy-paste para cada agente, integração com Lovable, FAQ de 7 perguntas, checklist de entrega com 16 itens. Inclui diagramas ASCII dos dois fluxos de invocação (B2C vs. SaaS técnico).

**Sprint 4 — Modo Lovable-First**
- `KERNO-LOVABLE-FIRST.html` (790 linhas) — guia completo da trilha sem BMAD para vibe coders que querem ir direto ao Lovable. 5 etapas (PRD Mínimo → Ambiente Seguro → Desenvolvimento → Revisão de Segurança → Deploy), 7 prompts prontos para copiar, comparativo "Lovable-First vs. BMAD" com 6 critérios cada, tabela de limites do modo (quando migrar), nota sobre BMAD retroativo.

### Modificado

**Sprint 1 — Correções no KERNO-GUIA-PASSO-A-PASSO.html**
- Versão do System Prompt corrigida: V2 → V3 em todas as referências
- Nomenclatura corrigida: `PRD_SYSTEM.md` → `prd.md`, `PRD_SYSTEM_MILESTONES.md` → `prd-milestones.md`
- Contagem de documentos corrigida: 15 → 23 (reflect a expansão da v5.0)
- Prompt da Seção B atualizado: Lovable agora *importa e valida* milestones do BMAD (não cria do zero)
- Caminhos de arquivos nos prompts D/E/F atualizados para kebab-case
- Adicionada Etapa 00.5 (Planejamento com BMAD) antes da Etapa 01

**Sprint 3 — WDS em duas ondas + 5 gates de qualidade**

`KERNO-ROADMAP.html`:
- WDS desmembrado em dois momentos distintos:
  - Passo 0.2 — WDS Onda 1: Saga (ANTES do PRD) — para produtos B2C onde UX é diferencial; outputs: `wds-product-brief.md`, `wds-trigger-map.md`
  - Passo 0.6 — WDS Onda 2: Freya + Idunn (APÓS PRD e Arquitetura) — outputs: `wds-scenarios.md`, `wds-design-system.md`, `wds-ui-roadmap.md`
  - Nota explícita: Design System visual (cores, tipografia, tokens) pode começar em paralelo a qualquer momento
- 5 gates de qualidade adicionados (blocos verdes com checklist de saída):
  - Gate Etapa 0: prd.md + architecture.md + database.md (com RLS) + qa-plan.md prontos
  - Gate Etapa 1: pipeline verde + Lovable validou milestones + Seção C aprovada
  - Gate Fase 2: delivery-report.md + SAST verde + P0 testados (por milestone)
  - Gate Fase 3: RLS 100% + SAST zero críticos + P0+P1 passando + Murat 2ª intervenção
  - Gate Fase 4: securityheaders.com OK + Sentry ativo + UptimeRobot monitorando

`KERNO-DOCS.md`:
- Grupo B reestruturado em B1 (WDS Onda 1 — Saga, antes do PRD) e B2 (WDS Onda 2 — Freya/Idunn, após PRD)
- Coluna "Momento" adicionada à tabela de cada grupo
- Exceção documentada: `wds-design-system.md` pode ser iniciado em paralelo

`CLAUDE.md`:
- Bloco de configuração YAML adicionado: `kerno-version`, `bmad-version`, `wds-enabled`, `wds-onda1`, `wds-onda2`, `cis-enabled`, `tea-enabled`
- Instrução para Claude Code: ao abrir projeto, ler flags para saber quais módulos foram usados
- Workflow reescrito como diagrama ASCII com as 5 etapas e 10 passos numerados
- Estrutura de `/docs` expandida mostrando os 4 grupos (A/B1/B2/C/D) visualmente separados

`KERNO-RETROFIT.html`:
- Nova seção "Retrofit ou Reconstrução?" inserida antes da Fase 0 (borda roxa)
- Prompt de diagnóstico em Plan Mode com 8 perguntas (sem alterar código)
- Coluna verde "Retrofit viável quando" com 7 critérios objetivos
- Coluna vermelha "Reconstrução recomendada quando" com 7 critérios objetivos
- Tabela de 4 casos intermediários com decisão e justificativa

**Sprint 4 — Atualização no KERNO-INICIO.html**
- Bloco S4 atualizado: status "concluído", link direto para `KERNO-LOVABLE-FIRST.html`

### Análise / Motivação
- **WDS em duas ondas:** a Onda 1 (Saga) precisa entrar antes do PRD para informar o produto — não decorar o que já foi decidido. A Onda 2 (Freya+Idunn) precisa do escopo fechado. Colocar todo o WDS no mesmo passo criava ambiguidade sobre timing.
- **Gates de qualidade:** pontos de checagem objetivos reduzem retrabalho — o custo de detectar um problema no Gate 1 é ordens de grandeza menor do que detectar na Fase 3.
- **Critério Retrofit vs. Reconstrução:** projetos irrecuperáveis estavam sendo submetidos ao processo de Retrofit, gerando esforço desperdiçado. O diagnóstico de 8 perguntas torna a decisão objetiva em vez de intuitiva.
- **Modo Lovable-First:** o framework completo tem barreira de entrada alta (instalação do BMAD, 11 agentes, 1-3h de planejamento). Para MVPs rápidos e spikes de validação, essa barreira é injustificada. O modo Lovable-First mantém as proteções não-negociáveis (SAST, RLS, Seção A) e elimina o planejamento antecipado.
- **KERNO-SEIZE-INICIO.md:** o mapeamento de agentes existia (KERNO-SEIZE-MAPEAMENTO.html), mas sem prompts exatos para invocação. Quem nunca usou o BMAD não sabia o que digitar para cada agente.

---

## [5.1] — 2026-03-02

### Mudança de Versão
- **Primeira release estável do Kerno Seize**: v5.0 foi o plano de fusão, v5.1 é a implementação completa e verificada de todas as correções.

### Adicionado
- `CLAUDE.md` — template de contexto automático para Claude Code (colocar na raiz do repo)
- `KERNO-CURSOR-REVIEW.md` — guia completo da Etapa 5 com workflows de revisão no Cursor + TEA
- Hub `index.html` agora lista CLAUDE.md e KERNO-CURSOR-REVIEW.md no índice

### Modificado
- `KERNO-ROADMAP.html` — reescrito com Etapa 0 (BMAD) como ponto de entrada, nomes kebab-case em todo o documento, tabela de arquivos atualizada para V3/CLAUDE.md
- `KERNO-RETROFIT.html` — reescrito como nativo v5.1: referências V3, nomes kebab-case, nota explícita sobre quando usar BMAD (apenas para funcionalidades novas, não para retrofit). Seção de migração v3.0 removida (nunca foi utilizada).
- Correções de nomenclatura residual: `QA_PLAN.md`, `DELIVERY_REPORT.md`, `SECURITY_REPORT.md` no Roadmap e Retrofit corrigidos para kebab-case
- Todas as versões de documentos atualizadas: KERNO-DOCS v2.1, System Prompt v3.1

### Análise / Motivação
- Verificação sistemática revelou nomes antigos (SCREAMING_SNAKE_CASE) ainda presentes no KERNO-ROADMAP e KERNO-RETROFIT — corrigidos nesta release
- Projetos existentes Kerno v3.0 precisavam de caminho claro de migração — criada seção dedicada no Retrofit

---

## [5.0] — 2026-03-02

### Mudança Fundamental
- **Fusão BMAD Method**: Kerno deixa de ser "Lovable + segurança" e se torna um framework de ciclo completo: BMAD (planejamento + UX) → GitHub (segurança) → Lovable (desenvolvimento) → Cursor (revisão) → Deploy (monitoramento)
- **Nova nomenclatura padrão**: todos os documentos migram de SCREAMING_SNAKE_CASE para kebab-case (padrão BMAD), garantindo compatibilidade com agentes BMAD/WDS/CIS/TEA
- **Lovable muda de papel**: de "planeja e implementa" para "apenas implementa e mantém docs"
- **6 etapas de desenvolvimento**: BMAD → WDS → GitHub → Lovable → Cursor → Deploy

### Adicionado
- `KERNO-SEIZE-MAPEAMENTO.html` v4.0 — plano de fusão completo com 9 seções, WDS integrado, fluxo corrigido
- `KERNO-ANALISE-WORKFLOW-v5.html` — análise técnica: verificação de workflow, origem dos documentos, padronização
- `LOVABLE-SYSTEM-PROMPT-V3.md` — System Prompt v3.0 adaptado para fusão BMAD (Seção A reescrita, Seção B reescrita)
- `KERNO-DOCS.md` v2.0 — framework expandido de 15 para 23 documentos, com 4 grupos (BMAD, WDS, CIS, Lovable)
- 7 novos documentos no framework: `product-brief.md`, `brainstorming-report.md`, `wds-product-brief.md`, `wds-trigger-map.md`, `wds-scenarios.md`, `wds-design-system.md`, `wds-ui-roadmap.md`

### Modificado
- Todos os 15 nomes de documentos migrados para kebab-case
- 5 documentos mudam de criador (de Lovable para BMAD)
- System Prompt Seção A: adicionada regra "se documento já existe, não recrie"
- System Prompt Seção B: reescrita completa — Lovable importa e implementa, não planeja

### Análise / Motivação
- BMAD é forte em planejamento (fases 0→3), Kerno é forte em execução (4→∞). A fusão cria um framework completo.
- Nomenclatura BMAD (kebab-case) escolhida como padrão porque os agentes reconhecem nativamente seus arquivos.
- Conflito de criação era o problema mais crítico: Lovable recriava documentos que o BMAD já tinha feito.

---

## [3.0] — 2026-03-01

### Mudança de Estratégia
- **Kit D reescrito por completo**: Kerno Monitor passa a ser roadmap evolutivo em 3 fases, foco nos diferenciais reais (audit logs centralizados + IA consultora via API do Claude)
- **Nova estratégia de monitoramento**: usar ferramentas gratuitas prontas (Sentry, UptimeRobot, GitHub) com UMA conta cada
- **Análise de custos documentada**: pesquisa real de limites dos planos gratuitos confirmou que uma agência com 4-6 projetos opera 100% no free tier

### Adicionado
- `KERNO-CHANGELOG.md` — histórico de versões do framework (este arquivo)
- `KERNO-RESPOSTA-INCIDENTES.html` — guia completo de resposta a incidentes com playbooks
- `KERNO-MONITOR-TUTORIAL.html` — reescrito com PRD focado em diferenciais reais, roadmap evolutivo em 3 fases

### Modificado
- `index.html` — atualizado para v3.0
- Kit D — de "projeto separado para construir agora" para "roadmap evolutivo"

---

## [2.1] — 2026-02-27

### Adicionado
- `KERNO-SECURITY-TUTORIAL.html` — seção 5.5 com tutorial completo de Cron Jobs
- `PROMPTS-SEGURANCA-LOVABLE.md` — 6 prompts prontos para copiar/colar no Lovable
- `KERNO-MONITOR-TUTORIAL.html` — tutorial inicial do painel de monitoramento (v1)
- `KERNO-INDICE.html` — índice visual de todos os documentos por kit

### Modificado
- Tutorial de Segurança atualizado de v2.0 para v2.1

---

## [2.0] — 2026-02-27

### Adicionado
- `LOVABLE-SYSTEM-PROMPT-V2.md` — System Prompt com árvore de dependências obrigatória (6 fases), integração com Plan Mode, estrutura em seções (A-G)
- `KERNO-RETROFIT.html` — guia para aplicar Kerno em projetos existentes (5 fases)
- `KERNO-ROADMAP.html` — roadmap visual em 5 fases para projetos novos
- `KERNO-SECURITY-TUTORIAL.html` — tutorial de 12 capítulos (SAST, DAST, Sentry, Audit Logs, etc.)

### Modificado
- System Prompt reescrito com base em docs.lovable.dev e GoEpic.dev
- Milestones seguem ordem de dependência obrigatória (auth → dados → features → integrações → polish → produção)

---

## [1.0] — 2026-02-26

### Adicionado (entrega inicial)
- `SECURITY.md` — 10 categorias de regras de segurança baseadas em OWASP Top 10
- `security-audit-v2.yml` — GitHub Action com Semgrep + npm audit + Scanner Kerno
- `dependabot.yml` — monitoramento semanal de dependências
- `KERNO-DOCS.md` — framework de 15 documentos obrigatórios com formatos e gatilhos
- `LOVABLE-SYSTEM-PROMPT.md` — versão inicial do system prompt (substituída na v2.0)
- `MAPEAMENTO-FERRAMENTAS.html` — mapeamento de ferramentas do Awesome AI Security
