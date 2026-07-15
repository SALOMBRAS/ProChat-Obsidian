# Configuração local

## Requisitos

- Windows com Git disponível.
- Node.js `24.16.0`.
- npm `11.16.0`.
- Repositório Main em `C:\Projeto Salo\ChatPro\ChatPro Main`.

Não são necessários pnpm, Yarn, Bun, Python ou Docker. A CLI oficial do Supabase é usada pontualmente por `npx`, sem instalação global ou dependência versionada do projeto.

## Instalação

Na raiz do Main:

```powershell
npm install
```

O comando instala os workspaces `@chatpro/web` e `@chatpro/whatsapp-connector` e utiliza somente o `package-lock.json` da raiz.

## Supabase

- Projeto remoto gratuito criado: `ChatPro`, região `sa-east-1`, instância `nano`.
- A integração web usa `@supabase/supabase-js` e `@supabase/ssr` em `apps/web`.
- Os clientes estão em `apps/web/src/lib/supabase/client.ts` (browser) e `apps/web/src/lib/supabase/server.ts` (server).
- O modelo de variáveis está em `apps/web/.env.example`. O Codex não criou `.env`, `.env.local` nem arquivos com valores reais.

Para uso local posterior, copie manualmente os valores públicos exibidos no diálogo **Connect** do projeto Supabase para `apps/web/.env.local`:

```dotenv
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY=
```

Use apenas a chave publicável. Nunca exponha nem versione `service_role`, senha do banco ou token de acesso da CLI.

O diagnóstico removível `apps/web/src/lib/supabase/connection-test.ts` verifica a URL e a chave publicável pelo endpoint de saúde do Supabase quando chamado em código de servidor após as variáveis serem configuradas. Ele não é executado automaticamente, não autentica usuários e não acessa tabelas da aplicação.

## Autenticação mínima

- As ações de e-mail e senha estão em `apps/web/src/app/auth/actions.ts`; o callback de confirmação está em `apps/web/src/app/auth/callback/route.ts`.
- As telas são `apps/web/src/app/login/page.tsx` e `apps/web/src/app/cadastro/page.tsx`.
- A rota `apps/web/src/app/app/page.tsx` mostra somente o e-mail autenticado e o botão de saída.
- `apps/web/src/proxy.ts` atualiza a sessão SSR e protege exclusivamente `/app`.

Com as variáveis do Supabase configuradas em `apps/web/.env.local`, inicie `npm run dev:web` e verifique `/login`, `/cadastro` e `/app`. Uma solicitação anônima para `/app` deve redirecionar para `/login`.

## Desenvolvimento

Aplicação web:

```powershell
npm run dev:web
```

Por padrão, o Next.js informa a URL local ao iniciar. Encerre com `Ctrl+C`.

Conector:

```powershell
npm run dev:connector
```

O conector imprime uma mensagem de inicialização e aguarda `SIGINT` ou `SIGTERM`. Encerre com `Ctrl+C`; a mensagem de encerramento controlado deve aparecer.

## Validação

```powershell
npm run lint
npm run typecheck
npm run build:web
npm run build:connector
npm run build
npm run check
```

`npm run check` executa lint, typecheck e os dois builds sem manter processos ativos.

Para iniciar o frontend compilado:

```powershell
npm run start --workspace @chatpro/web
```

Para iniciar o conector compilado:

```powershell
npm run start --workspace @chatpro/whatsapp-connector
```

## Estado e limitações

- Next.js `16.2.10` usa o bundler estável padrão informado pela própria CLI; nenhuma opção experimental foi habilitada.
- O npm informou scripts de instalação pendentes para `unrs-resolver`, `sharp` e `esbuild`; lint, typecheck, builds e execução de desenvolvimento passaram sem aprovação adicional.
- `npm audit` informou duas vulnerabilidades moderadas relacionadas ao `postcss@8.4.31` transitivo do Next.js. Não há correção estável coerente indicada pelo npm nesta data; não foi usado `--force`, canary, downgrade ou override.
- O Next.js exibe o aviso padrão de telemetria anônima durante o primeiro build.
- O Auth atual cobre somente cadastro, login, logout, confirmação de e-mail e sessão SSR. OAuth, recuperação de senha, perfis, workspaces, tabelas, migrações, banco de aplicação, Baileys, WhatsApp, QR Code, CRM, mensagens, mídias, PWA e deploy não estão implementados.
- A confirmação por e-mail depende das URLs de redirecionamento permitidas e do serviço de e-mail configurados no painel do Supabase. Não exponha chaves ou tokens ao configurar esse ambiente.
