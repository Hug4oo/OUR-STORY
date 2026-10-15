# 💞 Our Story

> O cantinho privado de vocês dois para guardar cada capítulo da história.

**Our Story** é um app web (PWA) para casais: memórias, fotos, metas financeiras, viagens, datas importantes, sonhos e muito mais, tudo sincronizado em **tempo real** entre as duas pessoas. Funciona no celular como um app instalado e é 100% privado: só o casal acessa os próprios dados.

![Next.js](https://img.shields.io/badge/Next.js-15-black?logo=nextdotjs)
![TypeScript](https://img.shields.io/badge/TypeScript-strict-3178c6?logo=typescript&logoColor=white)
![Tailwind](https://img.shields.io/badge/Tailwind-3.4-38bdf8?logo=tailwindcss&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-Auth%20%7C%20DB%20%7C%20Storage%20%7C%20Realtime-3ecf8e?logo=supabase&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-instalável-5a0fc8)

📖 Descrição completa do projeto: [SOBRE.md](./SOBRE.md)

---

## ✨ Funcionalidades

| Tela | O que faz |
|---|---|
| **Dashboard** | Visão geral: próximo encontro, atividade recente, metas e datas próximas |
| **Contador do encontro** | Contagem regressiva em tempo real (dias, horas, min, seg) |
| **Memórias** | Registro de momentos com comentários dos dois |
| **Galeria** | Fotos em grade masonry com upload |
| **Ela Gosta** | Preferências e gostos do par, base para ideias de presente |
| **Objetivos** | Metas financeiras com aportes e retiradas; saldo atualizado por trigger |
| **Viagens** | Planejamento com checklist, documentos e anotações |
| **Wishlist** | Lista de desejos do casal |
| **Linha do tempo** | A história em ordem cronológica |
| **Datas importantes** | Aniversários, comemorações e lembretes |
| **Mural dos Sonhos** | Sonhos do casal, com curtir e favoritar |
| **Insights IA** | Análise do que vocês registraram (local ou com Claude) |
| **Busca global** | Procura em tudo de uma vez |
| **Configurações** | Perfil, tema claro/escuro, capa, convite, exportar JSON, dados de exemplo |

Também: modo escuro, mobile first, instalável como app, multi-casal (cada casal isolado por RLS).

## 🧱 Stack

- **Front-end:** Next.js 15 (App Router), React 19, TypeScript, TailwindCSS, Radix UI (estilo shadcn/ui), lucide-react, next-themes
- **Back-end:** Supabase: Auth (Google, e-mail/senha, código por e-mail), Postgres com RLS, Storage privado, Realtime
- **IA (opcional):** Claude via `ANTHROPIC_API_KEY`
- **Deploy:** Vercel
- **PWA:** manifest + service worker

## 🚀 Como rodar

Pré-requisitos: **Node 20+** e uma conta gratuita no [Supabase](https://supabase.com).

```bash
git clone https://github.com/Hug4oo/our-story.git
cd our-story
npm install
cp .env.example .env.local     # preencha as variáveis (veja abaixo)
npm run dev                    # http://localhost:3000
```

### 1. Supabase

1. Crie um projeto em supabase.com.
2. Abra **SQL Editor → New query**, cole todo o conteúdo de `supabase/schema.sql` e execute. Cria tabelas, RLS, triggers, Realtime, bucket de Storage, busca e dados de exemplo. Pode rodar de novo com segurança.
3. Em **Project Settings → API**, copie para o `.env.local`:
   ```env
   NEXT_PUBLIC_SUPABASE_URL=https://SEU-PROJETO.supabase.co
   NEXT_PUBLIC_SUPABASE_ANON_KEY=sua-anon-key
   ```
   O nome do arquivo é exatamente `.env.local` (sem `.txt`) e a URL não leva `/rest/v1/`.
4. Em **Authentication → Sign In / Providers → Email**, mantenha **Enable Email provider** ligado. Para criar conta só com e-mail e senha, desative **Confirm email**.
5. (Opcional) **Google:** crie um OAuth Client ID no Google Cloud Console com a redirect URI `https://SEU-PROJETO.supabase.co/auth/v1/callback` e ative em **Providers → Google**.
6. Em **Authentication → URL Configuration**:
   - **Site URL:** `http://localhost:3000` (depois a URL da Vercel)
   - **Redirect URLs:** `http://localhost:3000/auth/callback` e `https://SEU-APP.vercel.app/auth/callback`

### 2. Primeiro uso

1. A **primeira pessoa** cria a conta → **Criar nosso espaço**.
2. Em **Configurações**, copia o código de convite e envia ao par.
3. A **segunda pessoa** cria a conta → **Tenho um convite** → cola o código.
4. Opcional: **Configurações → Carregar dados de exemplo**.
5. Depois que os dois tiverem conta, desligue **Enable email signups** no Supabase para fechar o cadastro.

### 3. IA (opcional)

```env
ANTHROPIC_API_KEY=sk-ant-...
ANTHROPIC_MODEL=claude-sonnet-4-5   # opcional
```

Sem a chave, os Insights usam análise local (contagens, palavras e categorias mais frequentes, metas, ideias de presente). A chave fica só no servidor.

## ☁️ Deploy na Vercel (grátis)

1. Suba o projeto para o GitHub.
2. Na [Vercel](https://vercel.com): **Add New → Project** e importe o repositório.
3. Em **Environment Variables** (no projeto, não na aba "Shared") adicione `NEXT_PUBLIC_SUPABASE_URL` e `NEXT_PUBLIC_SUPABASE_ANON_KEY` (e `ANTHROPIC_API_KEY`, se quiser).
4. **Deploy**. Se alterar variáveis depois, faça **Redeploy**.
5. Coloque a URL da Vercel no Supabase (Site URL e Redirect URLs).

Via CLI: `npx vercel login`, `npx vercel`, `npx vercel --prod`.

## 📱 Instalar no celular (PWA)

- **iPhone (Safari):** Compartilhar → *Adicionar à Tela de Início*
- **Android (Chrome):** menu ⋮ → *Instalar app*
- **Desktop (Chrome/Edge):** ícone de instalar na barra de endereço

O service worker só é registrado em produção.

## 🗂️ Estrutura

```
supabase/schema.sql          banco completo (tabelas, RLS, triggers, realtime, storage, busca, seed)
public/                      manifest.webmanifest, sw.js, ícones
src/middleware.ts            renova sessão e protege rotas
src/lib/supabase/            clientes browser/server
src/hooks/use-table.ts       CRUD + Realtime genérico
src/components/              shell, FormDialog, StorageImage, Countdown, ui/*
src/app/login, onboarding    entrada e criação/entrada no casal
src/app/(app)/*              todas as telas (protegidas)
src/app/api/ai/insights      rota de IA
```

## 🔒 Segurança

- Todas as tabelas com **RLS**: só membros do casal leem e escrevem (`is_member(couple_id)`).
- Storage privado, políticas por pasta `couple_id`, URLs assinadas.
- Funções `create_couple`, `join_couple`, `seed_demo_data` e `search_all` são `SECURITY DEFINER` e validam `auth.uid()`.
- Nunca use a `service_role key` no front-end (este projeto não a usa).
- **Nunca faça commit do `.env.local`.** Só o `.env.example`, sem valores reais.

## 🛠️ Problemas comuns

| Sintoma | Causa provável |
|---|---|
| `Invalid supabaseUrl` | `.env.local` sem os nomes das variáveis, ou salvo como `.env.local.txt` |
| "Email Logins are disabled" | Provedor Email desligado em *Authentication → Sign In / Providers* |
| Link mágico expira (`otp_expired`) | Scanners de e-mail (Hotmail/Outlook) consomem o link. Use e-mail e senha |
| `redirect_uri_mismatch` no Google | Falta a URI `…/auth/v1/callback` no OAuth client |
| Volta para `/login` após autenticar | Falta `…/auth/callback` em *Redirect URLs* |
| Mudanças não aparecem no outro aparelho | Rode o `schema.sql` completo (publicação Realtime) |
| Upload falha | Bucket `couple-media` não existe ou arquivo > 20 MB |
| "Este casal já está completo" | Cada casal aceita 2 pessoas |

## 🗺️ Roadmap

- Exportação em PDF/ZIP com fotos
- Notificações push de datas importantes
- Compressão de imagens no upload
- Avatar no perfil
- Tipos gerados do banco (`supabase gen types`)

## 📄 Licença

Projeto pessoal. Defina a licença que preferir (ex.: MIT) antes de tornar o repositório público.
