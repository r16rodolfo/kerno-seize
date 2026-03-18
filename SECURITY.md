# SECURITY.md — Regras de Segurança Obrigatórias

> Este documento deve ser seguido em TODA geração de código neste projeto.
> Ele define as regras de segurança baseadas no OWASP Top 10, OWASP LLM Top 10
> e boas práticas do Supabase. Nenhuma regra pode ser ignorada.

---

## Classificação do Projeto

Este projeto manipula dados sensíveis:
- Dados de usuários (cadastro, login, perfil)
- Dados financeiros (pagamentos, transações)
- Dados sensíveis (saúde, documentos pessoais)

Por isso, TODAS as regras abaixo são obrigatórias sem exceção.

---

## 1. Supabase — Regras de Banco de Dados

### 1.1 RLS (Row Level Security) — OBRIGATÓRIO

Toda tabela criada DEVE ter RLS ativado e policies explícitas.

```sql
-- SEMPRE faça isso ao criar qualquer tabela:
ALTER TABLE nome_da_tabela ENABLE ROW LEVEL SECURITY;

-- NUNCA crie policies com "true" (isso dá acesso a todos):
-- ❌ ERRADO:
CREATE POLICY "allow_all" ON tabela FOR SELECT USING (true);

-- ✅ CORRETO — usuário só acessa seus próprios dados:
CREATE POLICY "users_own_data" ON tabela FOR SELECT
  USING (auth.uid() = user_id);

CREATE POLICY "users_insert_own" ON tabela FOR INSERT
  WITH CHECK (auth.uid() = user_id);

CREATE POLICY "users_update_own" ON tabela FOR UPDATE
  USING (auth.uid() = user_id);

CREATE POLICY "users_delete_own" ON tabela FOR DELETE
  USING (auth.uid() = user_id);
```

### 1.2 Chaves e Credenciais

- **NUNCA** use `service_role` no frontend (pasta `src/`).
- A chave `service_role` só pode ser usada em Edge Functions.
- Use apenas `anon key` no frontend.
- Todas as chaves devem vir de variáveis de ambiente:

```typescript
// ✅ CORRETO
const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseKey = import.meta.env.VITE_SUPABASE_ANON_KEY

// ❌ ERRADO — nunca hardcode URLs ou chaves
const supabaseUrl = "https://abc123.supabase.co"
const supabaseKey = "eyJhbGci..."
```

### 1.3 Queries — Sempre com Tratamento de Erro

```typescript
// ✅ CORRETO — sempre desestruture data e error
const { data, error } = await supabase
  .from('tabela')
  .select('*')
  .eq('user_id', user.id)

if (error) {
  console.error('Erro ao buscar dados:', error.message)
  // Mostrar mensagem amigável ao usuário
  toast.error('Não foi possível carregar os dados. Tente novamente.')
  return
}

// ❌ ERRADO — nunca ignore o error
const { data } = await supabase.from('tabela').select('*')
```

### 1.4 Queries — Nunca Use Interpolação em Filtros

```typescript
// ❌ ERRADO — vulnerável a injeção
const { data } = await supabase
  .from('tabela')
  .or(`name.eq.${userInput}`)

// ✅ CORRETO — use os métodos tipados
const { data } = await supabase
  .from('tabela')
  .eq('name', userInput)
```

### 1.5 Storage — Regras de Upload

- Buckets com dados sensíveis (documentos, exames) DEVEM ser privados.
- Valide tipo e tamanho de arquivo ANTES do upload.
- Nunca aceite arquivos executáveis (.exe, .bat, .sh, .js).

```typescript
// Validação obrigatória antes de upload
const ALLOWED_TYPES = ['image/jpeg', 'image/png', 'application/pdf']
const MAX_SIZE = 5 * 1024 * 1024 // 5MB

function validateFile(file: File): { valid: boolean; error?: string } {
  if (!ALLOWED_TYPES.includes(file.type)) {
    return { valid: false, error: 'Tipo de arquivo não permitido.' }
  }
  if (file.size > MAX_SIZE) {
    return { valid: false, error: 'Arquivo muito grande. Máximo: 5MB.' }
  }
  return { valid: true }
}
```

---

## 2. Autenticação e Autorização

### 2.1 Proteção de Rotas — OBRIGATÓRIO

Toda página que mostra dados de usuário DEVE verificar autenticação.

```typescript
// Componente de proteção de rota OBRIGATÓRIO
import { useEffect } from 'react'
import { useNavigate } from 'react-router-dom'
import { useAuth } from '@/hooks/useAuth'

export function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const { user, loading } = useAuth()
  const navigate = useNavigate()

  useEffect(() => {
    if (!loading && !user) {
      navigate('/login', { replace: true })
    }
  }, [user, loading, navigate])

  if (loading) return <LoadingSpinner />
  if (!user) return null

  return <>{children}</>
}
```

### 2.2 Verificação de Propriedade

Antes de mostrar, editar ou deletar qualquer dado, verificar se pertence ao usuário logado:

```typescript
// ✅ CORRETO — sempre filtre por user_id
const { data } = await supabase
  .from('documents')
  .select('*')
  .eq('user_id', user.id)  // ← OBRIGATÓRIO
  .eq('id', documentId)

// ❌ ERRADO — qualquer um pode acessar qualquer documento
const { data } = await supabase
  .from('documents')
  .select('*')
  .eq('id', documentId)  // ← SEM filtro de user_id
```

### 2.3 Sessão

- Verifique sessão expirada antes de operações sensíveis.
- Redirecione para login se a sessão expirou.
- Nunca armazene tokens de sessão em localStorage.

---

## 3. Validação de Dados — OBRIGATÓRIO

### 3.1 Todo Input do Usuário DEVE Ser Validado com Zod

```typescript
import { z } from 'zod'

// Defina o schema ANTES do formulário
const profileSchema = z.object({
  name: z.string()
    .min(2, 'Nome deve ter pelo menos 2 caracteres')
    .max(100, 'Nome muito longo')
    .trim(),
  email: z.string()
    .email('Email inválido'),
  phone: z.string()
    .regex(/^\+?[1-9]\d{1,14}$/, 'Telefone inválido')
    .optional(),
  bio: z.string()
    .max(500, 'Bio muito longa')
    .optional(),
})

// Valide ANTES de enviar ao servidor
function handleSubmit(formData: unknown) {
  const result = profileSchema.safeParse(formData)

  if (!result.success) {
    // Mostrar erros ao usuário
    const errors = result.error.flatten().fieldErrors
    // ... exibir erros
    return
  }

  // Só agora salvar no banco
  await saveProfile(result.data)
}
```

### 3.2 Schemas para Dados Sensíveis

```typescript
// Dados financeiros
const transactionSchema = z.object({
  amount: z.number()
    .positive('Valor deve ser positivo')
    .max(1000000, 'Valor excede o limite'),
  currency: z.enum(['BRL', 'USD']),
  description: z.string().max(200).trim(),
})

// Dados de saúde
const healthRecordSchema = z.object({
  type: z.enum(['exam', 'prescription', 'report']),
  date: z.string().datetime(),
  notes: z.string().max(2000).trim().optional(),
  // Nunca aceite HTML em campos de texto de saúde
})
```

---

## 4. Proteção contra XSS (Cross-Site Scripting)

### 4.1 Regras Absolutas

```typescript
// ❌ PROIBIDO — nunca use sem sanitização
<div dangerouslySetInnerHTML={{ __html: userData }} />

// ❌ PROIBIDO
element.innerHTML = userData

// ❌ PROIBIDO
eval(userData)

// ❌ PROIBIDO
document.write(userData)

// ✅ Se absolutamente necessário renderizar HTML, SEMPRE sanitize:
import DOMPurify from 'dompurify'

<div dangerouslySetInnerHTML={{ __html: DOMPurify.sanitize(content) }} />
```

### 4.2 Renderização Segura de Dados

```typescript
// ✅ React já escapa automaticamente — use assim:
<p>{user.name}</p>
<span>{transaction.description}</span>

// ✅ Para URLs, valide antes de usar:
function isSafeUrl(url: string): boolean {
  try {
    const parsed = new URL(url)
    return ['https:', 'http:'].includes(parsed.protocol)
  } catch {
    return false
  }
}
```

---

## 5. Dados Sensíveis — Regras Especiais

### 5.1 O que NUNCA Armazenar no Frontend

```typescript
// ❌ NUNCA armazene no localStorage, sessionStorage ou cookies JS:
// - Senhas
// - Números de cartão de crédito
// - CPF / RG / documentos completos
// - Dados de saúde
// - Chaves de API
// - service_role key

// ❌ ERRADO
localStorage.setItem('user_cpf', cpf)
localStorage.setItem('auth_token', token)
localStorage.setItem('card_number', card)
```

### 5.2 Mascaramento de Dados Sensíveis na Tela

```typescript
// ✅ Sempre mascare dados sensíveis na exibição
function maskCPF(cpf: string): string {
  return `***.***.${cpf.slice(-5, -2)}-${cpf.slice(-2)}`
  // Mostra: ***.***. 123-45
}

function maskEmail(email: string): string {
  const [user, domain] = email.split('@')
  return `${user.slice(0, 2)}***@${domain}`
  // Mostra: jo***@gmail.com
}

function maskCardNumber(number: string): string {
  return `**** **** **** ${number.slice(-4)}`
  // Mostra: **** **** **** 1234
}
```

### 5.3 Console.log — Nunca com Dados Sensíveis

```typescript
// ❌ ERRADO
console.log('User data:', user)
console.log('Payment:', paymentDetails)
console.log('Token:', authToken)

// ✅ CORRETO — log apenas IDs e status
console.log('User loaded:', user.id)
console.log('Payment status:', payment.status)
```

---

## 6. Comunicação de Rede

### 6.1 HTTPS Obrigatório

```typescript
// ❌ ERRADO
fetch('http://api.example.com/data')

// ✅ CORRETO
fetch('https://api.example.com/data')
```

### 6.2 Headers de Segurança

Incluir no `vercel.json`, `netlify.toml` ou configuração equivalente:

```json
{
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Frame-Options", "value": "DENY" },
        { "key": "X-Content-Type-Options", "value": "nosniff" },
        { "key": "Referrer-Policy", "value": "strict-origin-when-cross-origin" },
        { "key": "Permissions-Policy", "value": "camera=(), microphone=(), geolocation=()" },
        {
          "key": "Content-Security-Policy",
          "value": "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline'; img-src 'self' data: https:; connect-src 'self' https://*.supabase.co"
        }
      ]
    }
  ]
}
```

---

## 7. Tratamento de Erros — Nunca Vazar Informações

```typescript
// ❌ ERRADO — expõe detalhes internos ao usuário
catch (error) {
  toast.error(`Erro: ${error.message} - Stack: ${error.stack}`)
}

// ✅ CORRETO — mensagem genérica para usuário, detalhe só no console
catch (error) {
  console.error('Erro interno:', error)
  toast.error('Ocorreu um erro. Tente novamente.')
}
```

---

## 8. IDs e URLs — Nunca Confiar na URL

```typescript
// ❌ ERRADO — qualquer um pode mudar o ID na URL
// URL: /users/123/documents
const userId = params.userId
const { data } = await supabase.from('documents').eq('user_id', userId)

// ✅ CORRETO — sempre use o ID do usuário autenticado
const { data: { user } } = await supabase.auth.getUser()
const { data } = await supabase.from('documents').eq('user_id', user.id)
```

---

## 9. Dependências e Pacotes

- Não adicione dependências desnecessárias.
- Prefira pacotes com muitos downloads e manutenção ativa.
- Nunca use pacotes deprecated (moment.js, request, etc).

---

## 10. Checklist Antes de Cada Commit

Antes de considerar o código pronto, verificar:

- [ ] Toda tabela nova tem RLS ativado?
- [ ] Toda tabela nova tem policies para SELECT, INSERT, UPDATE, DELETE?
- [ ] Toda query trata o campo `error`?
- [ ] Todo formulário valida com Zod antes de enviar?
- [ ] Nenhum dado sensível está em localStorage?
- [ ] Nenhum `dangerouslySetInnerHTML` sem DOMPurify?
- [ ] Nenhuma chave hardcoded no código?
- [ ] Todas as URLs usam HTTPS?
- [ ] Todas as rotas com dados estão protegidas por autenticação?
- [ ] Console.log não contém dados sensíveis?

---

## Referências

Este documento é baseado em:
- [OWASP Top 10](https://owasp.org/www-project-top-ten/) — As 10 vulnerabilidades web mais exploradas
- [OWASP LLM Top 10](https://owasp.org/www-project-top-10-for-large-language-model-applications/) — Riscos de código gerado por IA
- [Supabase Security Best Practices](https://supabase.com/docs/guides/auth/row-level-security)
- [OWASP AI Security and Privacy Guide](https://owasp.org/www-project-ai-security-and-privacy-guide/)
- [Awesome AI Security](https://github.com/ottosulin/awesome-ai-security) — Curadoria de ferramentas e frameworks
