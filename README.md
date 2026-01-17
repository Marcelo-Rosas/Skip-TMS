# 📦 Módulo de Embarcadores - SKIP TMS

Sistema completo de CRUD para gerenciamento de Embarcadores com máscaras, validações e integração Supabase.

## 🚀 Instalação

### 1. Dependências necessárias

```bash
# Instalar dependências
npm install @supabase/ssr @supabase/supabase-js
npm install @tanstack/react-query @tanstack/react-table
npm install react-hook-form @hookform/resolvers zod
npm install sonner date-fns
npm install lucide-react

# shadcn/ui components (se ainda não tiver)
npx shadcn-ui@latest init
npx shadcn-ui@latest add button input label textarea select switch
npx shadcn-ui@latest add separator card dialog alert-dialog
npx shadcn-ui@latest add dropdown-menu table badge skeleton
npx shadcn-ui@latest add form
```

### 2. Configurar Supabase

#### 2.1 Criar projeto no Supabase
1. Acesse https://supabase.com
2. Crie um novo projeto
3. Anote a URL e a ANON KEY

#### 2.2 Executar SQL
Execute no **SQL Editor** do Supabase:

```sql
-- Copie e cole o conteúdo de create-shippers-table.sql
```

#### 2.3 Configurar .env.local

```bash
NEXT_PUBLIC_SUPABASE_URL=sua_url_do_supabase
NEXT_PUBLIC_SUPABASE_ANON_KEY=sua_anon_key
NEXT_PUBLIC_APP_URL=http://localhost:3000
```

### 3. Estrutura de pastas

Organize os arquivos conforme a estrutura:

```
skip-tms/
├── app/
│   ├── layout.tsx (adicionar Providers)
│   ├── providers.tsx (novo)
│   └── registrations/
│       └── shippers/
│           ├── page.tsx
│           └── _components/
│               ├── shipper-form.tsx
│               └── shipper-table.tsx
├── components/
│   └── ui/
│       └── masked-input.tsx (novo)
├── lib/
│   ├── masks/
│   │   └── index.ts (novo)
│   ├── validations/
│   │   └── shipper.ts (novo)
│   ├── supabase/
│   │   └── client.ts (novo)
│   └── hooks/
│       └── use-shippers.ts (novo)
└── types/
    └── supabase.ts (gerar com CLI)
```

### 4. Atualizar app/layout.tsx

```tsx
import { Providers } from './providers';

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="pt-BR">
      <body>
        <Providers>
          {children}
        </Providers>
      </body>
    </html>
  );
}
```

### 5. Gerar tipos TypeScript do Supabase (opcional mas recomendado)

```bash
# Instalar CLI do Supabase
npm install -g supabase

# Login
supabase login

# Gerar tipos
npx supabase gen types typescript --project-id "seu-project-id" > types/supabase.ts
```

## 📝 Como usar

### Navegação
Acesse: `http://localhost:3000/registrations/shippers`

### Funcionalidades disponíveis

#### ✅ Criar Embarcador
1. Clique em "Novo Embarcador"
2. Preencha os campos obrigatórios (marcados com *)
3. As máscaras são aplicadas automaticamente (CPF/CNPJ, CEP, Telefone)
4. Validação em tempo real
5. Clique em "Criar Embarcador"

#### ✅ Listar Embarcadores
- Visualização em tabela com paginação
- Busca por nome, razão social ou documento
- Ordenação por colunas
- Badge de status (Ativo/Inativo)

#### ✅ Editar Embarcador
1. Clique nos 3 pontos (⋮) na linha do embarcador
2. Selecione "Editar"
3. Faça as alterações
4. Clique em "Atualizar Embarcador"

#### ✅ Ativar/Desativar
1. Clique nos 3 pontos (⋮)
2. Selecione "Ativar" ou "Desativar"
3. Embarcadores inativos não aparecem em cotações

#### ✅ Deletar Embarcador
1. Clique nos 3 pontos (⋮)
2. Selecione "Deletar"
3. Confirme a ação
4. **Nota:** Não é possível deletar se houver operações vinculadas

## 🎯 Validações Implementadas

### Campos obrigatórios
- Nome Fantasia
- Tipo de documento (CPF/CNPJ)
- Número do documento

### Máscaras aplicadas
- **CEP:** 00000-000
- **CPF:** 000.000.000-00
- **CNPJ:** 00.000.000/0000-00
- **Telefone:** (00) 00000-0000
- **Placa:** ABC-1D23

### Validações em tempo real
- CPF/CNPJ válidos (dígitos verificadores)
- Email formato válido
- CEP 8 dígitos
- Telefone 10-11 dígitos
- UF 2 caracteres
- Documento deve bater com o tipo selecionado

### Validações de banco
- Documento único (não duplicado)
- Relacionamento com cliente (se informado)
- Auditoria automática (created_at, updated_at)

## 🔧 Customização

### Adicionar novos campos

1. **Atualizar schema Zod** (`lib/validations/shipper.ts`):

```typescript
export const shipperSchema = z.object({
  // ... campos existentes
  novo_campo: z.string().optional()
});
```

2. **Adicionar no formulário** (`shipper-form.tsx`):

```tsx
<FormField
  control={form.control}
  name="novo_campo"
  render={({ field }) => (
    <FormItem>
      <FormLabel>Novo Campo</FormLabel>
      <FormControl>
        <Input {...field} />
      </FormControl>
    </FormItem>
  )}
/>
```

3. **Adicionar coluna na tabela** (`shipper-table.tsx`):

```typescript
{
  accessorKey: 'novo_campo',
  header: 'Novo Campo',
  cell: ({ row }) => row.original.novo_campo
}
```

4. **Atualizar SQL**:

```sql
ALTER TABLE public.shippers ADD COLUMN novo_campo VARCHAR(255);
```

### Adicionar nova máscara

Edite `lib/masks/index.ts`:

```typescript
export type MaskType = 'cep' | 'cpf' | 'cnpj' | 'phone' | 'plate' | 'currency' | 'nova_mascara';

// Adicione no switch do applyMask
case 'nova_mascara':
  return valor.replace(/padrão/, 'formato');
```

## 🐛 Troubleshooting

### Erro: "Documento inválido"
- Verifique se o CPF/CNPJ tem dígitos verificadores corretos
- Certifique-se que o tipo (CPF/CNPJ) bate com o número digitado

### Erro: "Já existe um embarcador com este documento"
- O documento deve ser único no sistema
- Verifique se não há outro embarcador com o mesmo CPF/CNPJ

### Erro: "Não é possível deletar"
- Existe operação vinculada a este embarcador
- Primeiro desvincule ou delete as operações relacionadas

### Máscaras não aparecem
- Verifique se o `MaskedInput` está importado corretamente
- Confirme que o `maskType` está correto

### React Query não funciona
- Verifique se o `Providers` está no `layout.tsx`
- Confirme que o `QueryClientProvider` está envolvendo o app

## 📊 Próximos Passos

Após concluir o módulo de Embarcadores, seguir com:

1. ✅ **CRUD de Clientes** (similar ao de Embarcadores)
2. ✅ **CRUD de Motoristas** (com campo ANTT)
3. ✅ **CRUD de Veículos** (com placa e eixos)
4. 🔄 **Pipeline Comercial** (integrado com Embarcadores)
5. 🔄 **Board Operacional** (selecionar Embarcador)
6. 🔄 **Motor de Precificação** (usar Embarcador nas regras)

## 🎓 Conceitos Aprendidos

- ✅ Sistema de máscaras reutilizável
- ✅ Validação Zod + React Hook Form
- ✅ React Query (queries + mutations)
- ✅ TanStack Table (filtros, ordenação, paginação)
- ✅ Supabase (CRUD + RLS)
- ✅ shadcn/ui components
- ✅ TypeScript strict mode
- ✅ Error handling
- ✅ Toasts com Sonner

## 📞 Suporte

Para dúvidas sobre o módulo:
1. Revise este README
2. Verifique os comentários no código
3. Consulte a documentação do Supabase
4. Consulte a documentação do shadcn/ui

---

**Desenvolvido com ❤️ para o SKIP TMS**
