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
- A fundação do Supabase está configurada; autenticação, tabelas, migrações, banco de aplicação, Baileys, WhatsApp, QR Code, CRM, mensagens, mídias, PWA e deploy não estão implementados.
