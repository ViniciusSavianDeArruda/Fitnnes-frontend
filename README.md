<div align="center">

# 🏋️ FIT.AI

**Plataforma inteligente de gestão de treinos com IA**

[![Next.js](https://img.shields.io/badge/Next.js-16-black?logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?logo=typescript)](https://www.typescriptlang.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-4-teal?logo=tailwindcss)](https://tailwindcss.com/)
[![License](https://img.shields.io/badge/license-MIT-green)](./LICENSE)

[Demo](https://app.fitnnesapp.online) · [API](https://api.fitnnesapp.online) · [Reportar Bug](https://github.com/issues) · [Sugerir Feature](https://github.com/issues)

</div>

---

## 📋 Índice

- [Sobre o Projeto](#-sobre-o-projeto)
- [Features](#-features)
- [Stack Tecnológica](#-stack-tecnológica)
- [Arquitetura](#-arquitetura)
- [Pré-requisitos](#-pré-requisitos)
- [Instalação e Configuração](#-instalação-e-configuração)
- [Variáveis de Ambiente](#-variáveis-de-ambiente)
- [Scripts Disponíveis](#-scripts-disponíveis)
- [Estrutura de Pastas](#-estrutura-de-pastas)
- [Autenticação](#-autenticação)
- [Camada de API (Orval)](#-camada-de-api-orval)
- [Sistema de Estilização](#-sistema-de-estilização)
- [Convenções de Código](#-convenções-de-código)
- [Componentes UI](#-componentes-ui)
- [Deploy](#-deploy)
- [Roadmap](#-roadmap)
- [Como Contribuir](#-como-contribuir)
- [Licença](#-licença)

---

## 🎯 Sobre o Projeto

O FIT.AI é uma aplicação web moderna para gestão inteligente de treinos. Desenvolvida com Next.js 16 (App Router), a plataforma integra inteligência artificial para personalizar planos de treino, rastrear progresso e fornecer estatísticas detalhadas sobre consistência e desempenho.

O frontend consome uma API REST cujo OpenAPI spec está disponível em [`/swagger.json`](https://api.fitnnesapp.online/swagger.json). A autenticação usa `better-auth` com Google OAuth e sessões no servidor.

---

## ✨ Features

- **Autenticação** — Login via Google OAuth com sessões gerenciadas pelo BetterAuth
- **Planos de Treino** — Criação, visualização e gerenciamento de planos e dias de treino
- **Rastreamento de Exercícios** — Acompanhamento de séries, repetições e carga
- **Estatísticas** — Heatmap de consistência, streaks e métricas de desempenho
- **Integração com IA** — Sugestões e análises geradas via Vercel AI SDK
- **Integração com Smartwatch** — Sincronização com dispositivos vestíveis
- **UI Responsiva** — Design mobile-first com suporte a todos os tamanhos de tela
- **Validação de Formulários** — Validação com Zod + React Hook Form
- **Client API Gerado** — Tipagem end-to-end via Orval + OpenAPI

---

## 🛠️ Stack Tecnológica

### Core

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [Next.js](https://nextjs.org/) | 16.x | Framework React (App Router) |
| [TypeScript](https://www.typescriptlang.org/) | 5.x | Tipagem estática |
| [React](https://react.dev/) | 19.x | Biblioteca de UI |

### Estilização

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [Tailwind CSS](https://tailwindcss.com/) | 4.x | Utilitários CSS |
| [shadcn/ui](https://ui.shadcn.com/) | latest | Componentes acessíveis |
| [tw-animate-css](https://www.npmjs.com/package/tw-animate-css) | 1.x | Animações Tailwind |
| [class-variance-authority](https://cva.style/) | 0.7.x | Variantes de componentes |
| [clsx](https://github.com/lukeed/clsx) + [tailwind-merge](https://github.com/dcastil/tailwind-merge) | latest | Merge condicional de classes |

### Formulários e Validação

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [React Hook Form](https://react-hook-form.com/) | 7.x | Gerenciamento de formulários |
| [Zod](https://zod.dev/) | 4.x | Schema e validação |
| [@hookform/resolvers](https://github.com/react-hook-form/resolvers) | 5.x | Integração RHF + Zod |

### Dados e API

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [Orval](https://orval.dev/) | 8.x | Geração de client a partir do OpenAPI |
| [nuqs](https://nuqs.47ng.com/) | 2.x | State em query params (URL) |
| [dayjs](https://day.js.org/) | 1.x | Manipulação de datas |

### IA e Streaming

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [Vercel AI SDK](https://sdk.vercel.ai/) | 6.x | Integração com modelos de IA |
| [@ai-sdk/react](https://sdk.vercel.ai/docs/ai-sdk-ui) | 3.x | Hooks React para streaming |
| [streamdown](https://github.com/nicolo-ribaudo/streamdown) | 2.x | Renderização de Markdown streamado |

### Autenticação

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [BetterAuth](https://www.better-auth.com/) | 1.x | Autenticação e gerenciamento de sessão |

### Tooling

| Tecnologia | Versão | Finalidade |
|---|---|---|
| [pnpm](https://pnpm.io/) | latest | Package manager |
| [ESLint](https://eslint.org/) | 9.x | Linting |
| [Prettier](https://prettier.io/) | 3.x | Formatação de código |
| [dotenv](https://github.com/motdotla/dotenv) | 17.x | Variáveis de ambiente |

---

## 🏗️ Arquitetura

### Visão geral

O frontend usa o App Router do Next.js. A arquitetura separa componentes server (Server Components) das partes client, e usa código gerado pelo Orval para chamadas à API.

Fluxo simplificado:
- Browser → Next.js (Server/Client Components) → Mutator customizado (`app/_lib/fetch.ts`) → Backend API (OpenAPI /swagger.json)

### Path aliases

O alias `@/*` aponta para a raiz do projeto. Exemplos de import:

```ts
import { Button } from "@/components/ui/button"
import { authClient } from "@/app/_lib/auth-client"
import { cn } from "@/lib/utils"
```

### Mutator de fetch (Orval)

O arquivo `app/_lib/fetch.ts` é o mutator usado pelo client gerado pelo Orval. Principais responsabilidades:

- Preencher a URL base (`NEXT_PUBLIC_API_URL`) nos endpoints gerados
- Encaminhar cookies/headers quando usado em Server Components (via `next/headers`)
- Ser injetado automaticamente nas funções geradas pelo Orval

---

## 📦 Pré-requisitos

- **Node.js** 18 ou superior
- **pnpm** — `npm install -g pnpm`
- Backend FIT.AI rodando localmente ou apontando para `https://api.fitnnesapp.online`

---

## 🚀 Instalação e Configuração

```bash
# 1. Clonar o repositório
git clone https://github.com/seu-usuario/fitnnes-frontend.git
cd fitnnes-frontend

# 2. Instalar dependências
pnpm install

# 3. Configurar variáveis de ambiente
cp .env.example .env
# Edite o .env com os valores corretos (veja seção abaixo)

# 4. (Opcional) Regenerar o client da API
# Necessário se o backend estiver rodando localmente com alterações no schema
npx orval

# 5. Iniciar o servidor de desenvolvimento
pnpm dev
```

Acesse `http://localhost:3000` no navegador.

---

## 🔑 Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```env
# URL do backend (API REST)
NEXT_PUBLIC_API_URL=http://localhost:8080

# URL do frontend (usado para callbacks de OAuth e afins)
NEXT_PUBLIC_BASE_URL=http://localhost:3000
```

> As variáveis prefixadas com `NEXT_PUBLIC_` são expostas ao browser. Não coloque segredos nelas.

Para apontar para o ambiente de produção:

```env
NEXT_PUBLIC_API_URL=https://api.fitnnesapp.online
NEXT_PUBLIC_BASE_URL=https://app.fitnnesapp.online
```

---

## 📜 Scripts Disponíveis

```bash
pnpm dev              # Inicia o servidor de desenvolvimento (Next.js)
pnpm build            # Gera o build de produção
pnpm start            # Inicia o servidor de produção (requer build)
pnpm lint             # Executa o ESLint em todo o projeto
npx orval             # Regenera o client da API a partir do OpenAPI spec
npx shadcn@latest add <component>   # Instala um novo componente shadcn/ui
```

> ⚠️ Nunca execute `pnpm dev` para verificar mudanças em ambiente de CI/CD ou automações — use `pnpm build` para validar.

---

## 📁 Estrutura de Pastas

```
fitnnes-frontend/
│
├── app/                          # Next.js App Router
│   ├── _lib/                     # Código interno da aplicação
│   │   ├── api/
│   │   │   ├── fetch-generated/  # Client gerado pelo Orval (Server Components)
│   │   │   │   └── index.ts
│   │   │   └── rc-generated/     # Hooks TanStack Query (Client Components, desabilitado)
│   │   │       └── index.ts
│   │   ├── auth-client.ts        # Instância do BetterAuth
│   │   └── fetch.ts              # Mutator customizado (URL + cookies)
│   │
│   ├── auth/                     # Página de autenticação (/auth)
│   ├── workout-plans/            # Planos de treino (/workout-plans)
│   ├── stats/                    # Estatísticas (/stats)
│   ├── profile/                  # Perfil do usuário (/profile)
│   │
│   ├── layout.tsx                # Layout raiz da aplicação
│   ├── page.tsx                  # Página inicial (/)
│   └── globals.css               # Estilos globais e variáveis de tema (oklch)
│
├── components/
│   └── ui/                       # Componentes shadcn/ui (estilo new-york)
│
├── lib/
│   └── utils.ts                  # Utilitário cn() (clsx + tailwind-merge)
│
├── public/                       # Assets estáticos (imagens, ícones, fontes)
│
├── .env.example                  # Template de variáveis de ambiente
├── eslint.config.mjs             # Configuração ESLint
├── next.config.ts                # Configuração Next.js
├── orval.config.ts               # Configuração do Orval
├── postcss.config.mjs            # Configuração PostCSS
├── components.json               # Configuração shadcn/ui
├── tsconfig.json                 # Configuração TypeScript
└── package.json
```

---

## 🔐 Autenticação

O projeto usa [BetterAuth](https://www.better-auth.com/) com Google OAuth. **Não existe middleware de autenticação** — cada página é responsável por verificar a sessão diretamente.

### Fluxo de Autenticação

```
Usuário acessa rota protegida
         │
         ▼
  getSession() / useSession()
         │
    ┌────┴────┐
    │         │
  Sessão    Sem sessão
  válida        │
    │           ▼
    ▼      redirect("/auth")
  Renderiza
  a página
```

### Em Server Components

```ts
import { headers } from "next/headers"
import { authClient } from "@/app/_lib/auth-client"
import { redirect } from "next/navigation"

const { data: session } = await authClient.getSession({
  fetchOptions: { headers: await headers() },
})

if (!session) redirect("/auth")
```

### Em Client Components

```ts
"use client"

import { authClient } from "@/app/_lib/auth-client"

export function MyComponent() {
  const { data: session, isPending } = authClient.useSession()

  if (isPending) return <Skeleton />
  if (!session) return null

  return <div>Olá, {session.user.name}</div>
}
```

### Página de Login

A rota `/auth` redireciona automaticamente para `/` se o usuário já estiver autenticado.

---

## 🔌 Camada de API (Orval)

O client da API é **gerado automaticamente** pelo [Orval](https://orval.dev/) a partir do OpenAPI spec do backend, disponível em `NEXT_PUBLIC_API_URL/swagger.json`.

### Targets Configurados

O arquivo `orval.config.ts` define dois targets:

#### `fetch` (ativo)

Gera funções fetch puras em `app/_lib/api/fetch-generated/index.ts`. Usado exclusivamente em **Server Components**, pois depende do `next/headers` para encaminhar cookies de autenticação.

```ts
// Exemplo de uso em Server Component
import { getWorkoutPlans } from "@/app/_lib/api/fetch-generated"

const plans = await getWorkoutPlans()
```

#### `rc` (comentado — para Client Components)

Quando habilitado, gera hooks TanStack Query em `app/_lib/api/rc-generated/index.ts` para uso em **Client Components**.

```ts
// orval.config.ts — descomente para ativar
// rc: {
//   output: {
//     client: "react-query",
//     target: "app/_lib/api/rc-generated/index.ts",
//     ...
//   }
// }
```

### Regenerando o Client

Sempre que o schema da API mudar, regenere o client:

```bash
npx orval
```

> O backend precisa estar rodando e acessível em `NEXT_PUBLIC_API_URL` para que o Orval busque o `swagger.json`.

---

## 🎨 Sistema de Estilização

### Tailwind CSS v4

O projeto usa Tailwind CSS v4 com variáveis de cor em **oklch**, definidas em `app/globals.css`. Isso permite suporte nativo a temas claros/escuros com alta fidelidade de cor.

**Regra obrigatória:** sempre use as CSS variables do tema, nunca cores hardcoded do Tailwind:

```tsx
// ✅ Correto — usa variáveis do tema
<div className="bg-primary text-primary-foreground" />
<p className="text-muted-foreground" />
<div className="border border-border bg-card" />

// ❌ Errado — cor hardcoded, quebra o tema
<div className="bg-blue-500 text-white" />
```

### Fontes

Três fontes estão configuradas como CSS variables:

| Variável | Fonte | Uso |
|---|---|---|
| `--font-geist-sans` | Geist Sans | Texto geral (`font-sans`) |
| `--font-geist-mono` | Geist Mono | Código (`font-mono`) |
| `--font-inter-tight` / `--font-heading` | Inter Tight | Títulos e headings |

### shadcn/ui

Componentes baseados em Radix UI com estilo **new-york** e ícones Lucide. Para instalar novos componentes:

```bash
npx shadcn@latest add button
npx shadcn@latest add dialog
npx shadcn@latest add form
```

Os componentes são adicionados em `components/ui/` e podem ser customizados livremente.

### Imagens

Sempre use o componente `Image` do Next.js para imagens — nunca a tag `<img>` nativa:

```tsx
import Image from "next/image"

<Image src={user.avatar} alt={user.name} width={40} height={40} />
```

Domínios externos permitidos (configurados em `next.config.ts`):

```
*.ufs.sh
```

---

## 📐 Convenções de Código

### Nomenclatura

| Tipo | Convenção | Exemplo |
|---|---|---|
| Arquivos e pastas | `kebab-case` | `workout-card.tsx`, `use-session.ts` |
| Componentes React | `PascalCase` | `WorkoutCard`, `StatsHeader` |
| Funções e variáveis | `camelCase` | `getSession`, `workoutPlans` |
| Variáveis de ambiente | `UPPER_SNAKE_CASE` | `NEXT_PUBLIC_API_URL` |

### Datas

Sempre use `dayjs` para formatar ou manipular datas. **Nunca** use métodos nativos de formatação do `Date`:

```ts
// ✅ Correto
import dayjs from "dayjs"
dayjs(workout.date).format("DD/MM/YYYY")
dayjs().diff(dayjs(startDate), "day")

// ❌ Errado
new Date(workout.date).toLocaleDateString()
```

### Comentários

O projeto adota a convenção de **zero comentários no código**. O código deve ser autoexplicativo via bons nomes de variáveis, funções e tipos TypeScript.

### TypeScript

- Modo `strict` ativado
- Prefira tipos explícitos a `any`
- Tipos de resposta da API são inferidos automaticamente a partir do código gerado pelo Orval

---

## 🧩 Componentes UI

O projeto usa [shadcn/ui](https://ui.shadcn.com/) com estilo `new-york`. Os componentes ficam em `components/ui/` e são importados via alias:

```tsx
import { Button } from "@/components/ui/button"
import { Card, CardHeader, CardContent } from "@/components/ui/card"
import { Input } from "@/components/ui/input"
import { Badge } from "@/components/ui/badge"
```

O utilitário `cn()` em `lib/utils.ts` combina `clsx` com `tailwind-merge` para merge seguro de classes:

```ts
import { cn } from "@/lib/utils"

<div className={cn("base-class", isActive && "active-class", className)} />
```

---

## 🚢 Deploy

O projeto está configurado para deploy na [Vercel](https://vercel.com/).

```bash
# Build de produção local
pnpm build
pnpm start
```

Variáveis de ambiente necessárias em produção:

```env
NEXT_PUBLIC_API_URL=https://api.fitnnesapp.online
NEXT_PUBLIC_BASE_URL=https://app.fitnnesapp.online
```

---

## 🗺️ Roadmap

- [ ] Melhorar cobertura de testes automatizados (Vitest + Testing Library)
- [ ] Ativar target `rc` do Orval (TanStack Query para Client Components)
- [ ] Adicionar notificações push
- [ ] Refatorar responsividade para tablets
- [ ] Evoluir integração com smartwatch
- [ ] Dashboard de estatísticas avançadas
- [ ] Adicionar temas customizáveis
- [ ] Internacionalização (i18n)

---

## 🤝 Como Contribuir

Contribuições são bem-vindas! Siga os passos abaixo:

1. **Fork** o repositório e crie uma branch a partir de `main`:
   ```bash
   git checkout -b feat/minha-feature
   ```

2. Faça suas alterações seguindo as [convenções de código](#-convenções-de-código).

3. Execute o lint antes de commitar:
   ```bash
   pnpm lint
   ```

4. Faça commit seguindo o padrão [Conventional Commits](https://www.conventionalcommits.org/):
   ```
   feat: adiciona heatmap de consistência semanal
   fix: corrige redirecionamento após login
   refactor: extrai componente WorkoutCard
   ```

5. Abra um **Pull Request** descrevendo as mudanças e o motivo.

---

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](./LICENSE) para mais detalhes.

---


> Projeto em evolução — contribuições e feedback são bem-vindos.


