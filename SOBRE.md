# Our Story: descrição completa

## Resumo para o campo "About" do GitHub

> App web privado para casais (PWA): memórias, galeria, metas, viagens, datas e sonhos sincronizados em tempo real. Next.js 15 + Supabase.

**Topics sugeridos:** `nextjs` `typescript` `tailwindcss` `supabase` `pwa` `realtime` `couples-app` `postgres` `row-level-security` `vercel` `pt-br`

---

## O que é

**Our Story** é uma plataforma privada para casais registrarem e acompanharem a vida a dois em um só lugar. Em vez de espalhar fotos no celular, planos em anotações e metas em planilhas, o casal tem um espaço compartilhado, bonito e seguro, onde tudo o que um adiciona aparece na hora para o outro.

O app funciona no navegador e pode ser instalado na tela inicial do celular como um aplicativo (PWA), sem passar pela App Store ou Google Play.

## Para quem é

Casais que querem guardar memórias, planejar o futuro juntos e ter um espaço só deles. Cada casal é totalmente isolado: ninguém além das duas pessoas vê o conteúdo.

## Como funciona

1. **A primeira pessoa** cria a conta e o "nosso espaço". Ela vira o membro A.
2. O app gera um **código de convite**.
3. **A segunda pessoa** cria a conta e entra com o código. Ela vira o membro B.
4. A partir daí, tudo é compartilhado e sincronizado em **tempo real**: se um adiciona uma foto ou faz um aporte numa meta, o outro vê instantaneamente, sem recarregar a página.

Cada casal aceita exatamente duas pessoas.

## Telas e recursos

**Dashboard.** Visão geral com próximo encontro, atividade recente, progresso das metas e datas que estão chegando.

**Contador do encontro.** Contagem regressiva em tempo real até o próximo encontro, com dias, horas, minutos e segundos.

**Memórias.** Registro de momentos especiais, com texto, data e comentários dos dois.

**Galeria.** Fotos do casal em grade masonry, com upload direto do celular. As imagens ficam em storage privado e são servidas por URLs assinadas e temporárias.

**Ela Gosta.** Caderno de preferências e gostos do par (comidas, lugares, estilos, tamanhos, o que ama e o que evita). Serve de base para ideias de presente.

**Objetivos.** Metas financeiras do casal (viagem, casa, reserva) com aportes e retiradas. O saldo é recalculado automaticamente por um trigger no banco.

**Viagens.** Planejamento completo por viagem: checklist, documentos (passagens, reservas) e anotações.

**Wishlist.** Lista de desejos compartilhada.

**Linha do tempo.** A história do casal em ordem cronológica.

**Datas importantes.** Aniversários, comemorações e lembretes.

**Mural dos Sonhos.** Sonhos do casal em formato de mural, com curtir e favoritar.

**Insights IA.** Análise do conteúdo registrado: palavras e categorias mais frequentes, progresso das metas e sugestões de presentes a partir das preferências e da wishlist. Funciona sem chave, com análise local. Com uma chave da Anthropic, usa o Claude para insights mais ricos. A chave fica só no servidor.

**Busca global.** Procura em memórias, fotos, metas, viagens, anotações e documentos de uma só vez (função SQL `search_all`).

**Configurações.** Perfil, tema claro/escuro, capa, código de convite, exportação dos dados em JSON e carga de dados de exemplo.

## Diferenciais

- **Tempo real:** sincronização entre os dois aparelhos via Supabase Realtime.
- **Privacidade por design:** RLS em todas as tabelas, storage privado e nenhuma `service_role key` no front-end.
- **Instalável (PWA):** ícone na tela inicial, abre em tela cheia.
- **Mobile first:** pensado para o celular, funciona bem no desktop.
- **Modo escuro** e visual com identidade própria.
- **Multi-casal:** a arquitetura já suporta vários casais isolados na mesma instância.
- **Login flexível:** e-mail e senha, código por e-mail ou Google.
- **Dados de exemplo:** um clique para ver o app cheio de conteúdo.

## Arquitetura

| Camada | Tecnologia |
|---|---|
| Interface | Next.js 15 (App Router), React 19, TypeScript strict, Tailwind 3.4, Radix UI, lucide-react, next-themes |
| Autenticação | Supabase Auth (`@supabase/ssr`), middleware que renova sessão e protege rotas |
| Banco | Postgres (Supabase) com RLS, triggers e funções `SECURITY DEFINER` |
| Arquivos | Supabase Storage, bucket privado `couple-media`, URLs assinadas |
| Tempo real | Supabase Realtime, hook genérico `useTable` (INSERT/UPDATE/DELETE) |
| IA | Rota `/api/ai/insights`: heurística local ou Claude |
| Hospedagem | Vercel (plano gratuito) |

### Modelo de dados

`profiles`, `couples`, `couple_members`, `memories`, `memory_comments`, `photos`, `preferences`, `goals`, `goal_contributions`, `trips`, `trip_checklist`, `trip_documents`, `trip_notes`, `wishlist`, `timeline_events`, `important_dates`, `dreams`, `dream_likes`, `activity_log`, `ai_insights`.

Toda tabela de conteúdo tem `couple_id`, e as políticas usam `is_member(couple_id)`.

### Segurança

- RLS em todas as tabelas: só membros do casal leem e escrevem.
- Storage privado com políticas por pasta (`couple_id`).
- Funções `create_couple`, `join_couple`, `seed_demo_data` e `search_all` validam `auth.uid()` e a participação no casal.
- Triggers de auditoria (`log_activity`) alimentam a atividade recente.

## Como distribuir

O caminho gratuito é o **PWA**: abrir o link no celular e usar *Adicionar à Tela de Início* (iPhone/Safari) ou *Instalar app* (Android/Chrome). Para publicar nas lojas seria preciso empacotar o app e, no iOS, ter uma conta Apple Developer (cerca de US$ 99 por ano).

## Estado do projeto

Versão funcional com as 12 telas, busca global, configurações, sincronização em tempo real, PWA e deploy na Vercel.

**Roadmap:** exportação em PDF/ZIP com fotos, notificações push de datas, compressão de imagens no upload, avatar de perfil, tipos gerados do banco.

---

*Feito com 💞 para guardar cada capítulo.*
