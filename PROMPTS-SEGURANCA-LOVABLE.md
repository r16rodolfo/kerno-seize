# Prompts Prontos para Lovable — Segurança e Monitoramento

> **Uso:** Copie e cole cada prompt diretamente no chat do Lovable.
> Execute UM de cada vez. Teste que funciona antes de passar ao próximo.
> **📌 Fixe a versão no Lovable antes de cada prompt.**

---

## Prompt 1: Sistema de Audit Logs (Supabase)

```
IMPLEMENTAÇÃO DE SEGURANÇA — Audit Logs

Crie um sistema de logs de auditoria no Supabase que registra
automaticamente TODA ação de INSERT, UPDATE e DELETE nas tabelas
com dados sensíveis.

### O que criar no Supabase (SQL):

1. Tabela `audit_log` com as colunas:
   - id (UUID, PK, gen_random_uuid())
   - timestamp (TIMESTAMPTZ, default now())
   - user_id (UUID, FK auth.users, nullable — para ações do sistema)
   - action (TEXT — 'INSERT', 'UPDATE', 'DELETE')
   - table_name (TEXT)
   - record_id (TEXT)
   - old_data (JSONB — dados antes da mudança, null para INSERT)
   - new_data (JSONB — dados depois da mudança, null para DELETE)
   - ip_address (TEXT, nullable)

2. RLS na tabela audit_log:
   - Ativar RLS
   - SELECT: apenas admins (usando a função has_role existente ou
     verificando na tabela user_roles)
   - Nenhuma policy para INSERT/UPDATE/DELETE pelo frontend
     (apenas o trigger SECURITY DEFINER pode inserir)

3. Indexes:
   - idx_audit_log_timestamp (timestamp DESC)
   - idx_audit_log_user (user_id)
   - idx_audit_log_table (table_name)

4. Função `log_audit()` como TRIGGER FUNCTION:
   - SECURITY DEFINER (roda com permissão elevada)
   - Captura auth.uid() como user_id
   - Captura TG_OP como action
   - Captura TG_TABLE_NAME como table_name
   - Captura OLD.id ou NEW.id como record_id
   - Captura to_jsonb(OLD) e to_jsonb(NEW) conforme a operação
   - RETURN NEW para INSERT/UPDATE, RETURN OLD para DELETE

5. Triggers: Conecte o trigger `log_audit()` a TODAS as tabelas
   que contenham dados de usuário. Liste as tabelas existentes e
   me diga em quais vai conectar.

### Regras:
- NÃO altere nenhuma funcionalidade existente
- NÃO altere nenhuma tabela existente (apenas adicione a nova + triggers)
- Teste que as operações existentes continuam funcionando normalmente
- O audit_log deve ser invisível para o usuário — funciona em background

Atualize /docs/DATABASE.md, /docs/SECURITY_REPORT.md e /docs/CHANGELOG.md.
```

---

## Prompt 2: Cron Job para Limpeza de Audit Logs

```
IMPLEMENTAÇÃO DE MANUTENÇÃO — Cron Job de Limpeza de Audit Logs

Crie uma Supabase Edge Function + pg_cron para limpar logs antigos
automaticamente, mantendo o banco saudável.

### Regras de retenção:
- Logs de INSERT e UPDATE: manter por 90 dias, deletar os mais antigos
- Logs de DELETE: manter por 365 dias (deleções são mais sensíveis)
- Rodar a limpeza toda segunda-feira às 3h da manhã (UTC)

### Opção A — pg_cron (preferido, se disponível no plano):

Crie no SQL Editor do Supabase:

1. Ative a extensão pg_cron:
   `CREATE EXTENSION IF NOT EXISTS pg_cron;`

2. Crie a função de limpeza:
   ```sql
   CREATE OR REPLACE FUNCTION cleanup_audit_logs()
   RETURNS void AS $$
   BEGIN
     -- Deletar INSERT/UPDATE com mais de 90 dias
     DELETE FROM audit_log
     WHERE action IN ('INSERT', 'UPDATE')
     AND timestamp < now() - interval '90 days';

     -- Deletar DELETE com mais de 365 dias
     DELETE FROM audit_log
     WHERE action = 'DELETE'
     AND timestamp < now() - interval '365 days';
   END;
   $$ LANGUAGE plpgsql SECURITY DEFINER;
   ```

3. Agende o cron job:
   ```sql
   SELECT cron.schedule(
     'cleanup-audit-logs',
     '0 3 * * 1',  -- toda segunda às 3h UTC
     'SELECT cleanup_audit_logs()'
   );
   ```

### Opção B — Se pg_cron não estiver disponível:

Crie uma Supabase Edge Function chamada `cleanup-audit-logs` que:
1. Executa as mesmas deleções da Opção A
2. Retorna quantos registros foram removidos
3. Me instrua a configurar um cron externo (cron-job.org) para
   chamar essa function semanalmente

### Importante:
- NÃO altere nenhuma funcionalidade existente
- Adicione um log no próprio audit_log registrando a limpeza:
  action='SYSTEM', table_name='audit_log', new_data com contagem de removidos
- Atualize /docs/DATABASE.md e /docs/CHANGELOG.md
```

---

## Prompt 3: Instalação do Sentry

```
IMPLEMENTAÇÃO DE MONITORAMENTO — Sentry Error Tracking

Instale o Sentry para capturar erros em produção automaticamente.

### O que fazer:

1. Adicione a dependência: @sentry/react

2. Crie o arquivo src/lib/sentry.ts:
   ```typescript
   import * as Sentry from '@sentry/react';

   export function initSentry() {
     if (import.meta.env.PROD) {
       Sentry.init({
         dsn: import.meta.env.VITE_SENTRY_DSN,
         integrations: [
           Sentry.browserTracingIntegration(),
           Sentry.replayIntegration(),
         ],
         tracesSampleRate: 0.1,
         replaysSessionSampleRate: 0.1,
         replaysOnErrorSampleRate: 1.0,
         beforeSend(event) {
           // Remover dados sensíveis
           if (event.request?.cookies) {
             delete event.request.cookies;
           }
           return event;
         },
       });
     }
   }
   ```

3. No main.tsx, ANTES do ReactDOM.createRoot:
   ```typescript
   import { initSentry } from './lib/sentry';
   initSentry();
   ```

4. Envolva o App com ErrorBoundary:
   ```tsx
   import * as Sentry from '@sentry/react';

   // Dentro do render:
   <Sentry.ErrorBoundary fallback={<p>Algo deu errado. Tente recarregar.</p>}>
     <App />
   </Sentry.ErrorBoundary>
   ```

5. Adicione ao .env.example:
   ```
   VITE_SENTRY_DSN=
   ```

### Regras:
- Sentry SÓ inicializa em produção (import.meta.env.PROD)
- NUNCA envie tokens, senhas ou PII ao Sentry
- NÃO altere nenhuma funcionalidade existente
- O ErrorBoundary deve mostrar uma mensagem amigável, não o stack trace

Atualize /docs/ARCHITECTURE.md, /docs/CHANGELOG.md e /docs/SECURITY_REPORT.md.
```

---

## Prompt 4: Revisão de Segurança Completa

```
REVISÃO DE SEGURANÇA — Análise completa do projeto

NÃO altere nenhum código ainda. Apenas analise e me reporte:

1. SUPABASE — Para CADA tabela existente, diga:
   | Tabela | RLS Ativo? | Policies | Audit Trigger? |
   Verifique se TODAS as tabelas têm RLS ativo.

2. CÓDIGO — Busque nos arquivos src/ por:
   - service_role (NUNCA deve estar no frontend)
   - dangerouslySetInnerHTML (deve ter DOMPurify)
   - localStorage com dados sensíveis (tokens, senhas, CPF)
   - console.log com dados sensíveis
   - Queries Supabase sem { data, error } tratados
   - Formulários sem validação Zod
   - eval(), innerHTML, document.write (nunca usar)

3. CONFIGURAÇÃO:
   - vercel.json existe com headers de segurança?
   - Variáveis de ambiente estão corretas?
   - Alguma chave hardcoded no código?

4. CLASSIFICAÇÃO:
   Para cada problema encontrado, classifique:
   🔴 CRÍTICO — Dados podem ser acessados indevidamente
   🟠 ALTO — Vulnerabilidade explorável
   🟡 MÉDIO — Má prática que deve ser corrigida
   ⚪ BAIXO — Melhoria recomendada

Apresente em formato de tabela e me diga a ordem de correção.
NÃO corrija nada — apenas reporte. Eu decido o que corrigir.

Atualize /docs/SECURITY_REPORT.md com os resultados.
```

---

## Prompt 5: Headers de Segurança (Vercel)

```
SEGURANÇA DE DEPLOY — Headers Vercel

Crie o arquivo vercel.json na raiz do projeto com os seguintes
headers de segurança para TODAS as rotas:

- X-Frame-Options: DENY
- X-Content-Type-Options: nosniff
- X-XSS-Protection: 1; mode=block
- Referrer-Policy: strict-origin-when-cross-origin
- Permissions-Policy: camera=(), microphone=(), geolocation=()
- Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
- Content-Security-Policy:
  default-src 'self';
  script-src 'self' 'unsafe-inline' 'unsafe-eval';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  connect-src 'self' https://*.supabase.co wss://*.supabase.co;
  font-src 'self' https://fonts.gstatic.com;
  frame-ancestors 'none'

Se o vercel.json já existir, apenas adicione os headers sem
alterar configurações existentes.

NÃO altere nenhum outro arquivo.
Atualize /docs/ARCHITECTURE.md e /docs/CHANGELOG.md.
```

---

## Prompt 6: Política de Privacidade e Termos de Uso

```
CONFORMIDADE LEGAL — LGPD

Crie duas páginas acessíveis publicamente (sem autenticação):

1. /privacy — Política de Privacidade
2. /terms — Termos de Uso

### Conteúdo da Política de Privacidade:
- Nome da aplicação: [NOME DO APP]
- Dados coletados: email, nome, dados de uso
- Finalidade: funcionamento do serviço
- Armazenamento: Supabase (servidores AWS)
- Compartilhamento: não compartilhamos com terceiros
- Direitos do usuário: acesso, correção, exclusão (LGPD Art. 18)
- Contato: [SEU EMAIL]
- Cookies: apenas cookies essenciais de sessão

### Conteúdo dos Termos de Uso:
- Descrição do serviço
- Responsabilidades do usuário
- Limitação de responsabilidade
- Propriedade intelectual
- Modificações nos termos
- Lei aplicável: Brasil

### Design:
- Use o mesmo layout do app (mesma sidebar/header se logado, ou layout limpo se não logado)
- Texto em português do Brasil
- Tipografia legível, espaçamento generoso

### Links:
- Adicione links para /privacy e /terms no rodapé do app
- Adicione checkbox obrigatório no registro: "Li e concordo com os Termos de Uso e Política de Privacidade"

NÃO altere nenhuma funcionalidade existente.
Atualize /docs/UI_COMPONENTS.md, /docs/FLOWS.md e /docs/CHANGELOG.md.
```

---

## Ordem de Execução Recomendada

| # | Prompt | Pré-requisito | Prioridade |
|---|--------|---------------|-----------|
| 4 | Revisão de Segurança | Nenhum | 🔴 Fazer primeiro |
| 1 | Audit Logs | Revisão feita | 🔴 Crítico |
| 5 | Headers Vercel | Nenhum | 🟠 Alto |
| 3 | Sentry | Nenhum | 🟠 Alto |
| 2 | Cron Job Cleanup | Audit Logs implementado | 🟡 Médio |
| 6 | LGPD (Privacy + Terms) | Layout pronto | 🟡 Médio |

**📌 REGRA: Fixe a versão no Lovable ANTES e DEPOIS de cada prompt.**
