# Configuração local

## Requisitos

- Windows com Git disponível.
- Node.js `24.16.0`.
- npm `11.16.0`.
- Repositório Main em `C:\Projeto Salo\ChatPro\ChatPro Main`.

Não são necessários pnpm, Yarn, Bun ou Python. Docker Desktop é necessário para operar o WAHA local. A CLI oficial do Supabase é usada pontualmente por `npx`, sem instalação global ou dependência versionada do projeto.

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

## WAHA local

- O serviço local usa WAHA Core 2026.6.2 na imagem oficial fixada por digest em `docker-compose.yml` na raiz do Main.
- Copie manualmente `.env.waha.example` para `.env.waha` e preencha `WAHA_API_KEY`, as credenciais do dashboard e as credenciais do Swagger. O arquivo real e `.waha/sessions/` são ignorados pelo Git.
- O serviço fica limitado a `127.0.0.1:${WAHA_PORT:-3000}`; não há publicação externa. As sessões ficam em `.waha/sessions/`, montado em `/app/.sessions`.
- Operação: `npm run waha:start`, `npm run waha:stop`, `npm run waha:status` e `npm run waha:logs`.
- Endpoints operacionais validados: `GET /health`, interface Swagger em `/`, `POST /api/sessions` e `GET /api/{session}/auth/qr`; as chamadas de API usam `X-Api-Key`.
- O QR Code é temporário e não deve ser registrado. Não escaneie uma conta real sem decisão expressa; WAHA, WhatsApp, Supabase e o frontend continuam sem integração entre si.
- No futuro, o consumo do WAHA deve ocorrer por adaptador próprio, nunca diretamente pelos componentes da aplicação.

## Conector WAHA local

- O contrato está em `packages/whatsapp-core/src/index.ts`; o adaptador e a API estão em `services/whatsapp-connector/src/`.
- Reutilize `.env.waha` para `WAHA_API_KEY`; defina também `WAHA_BASE_URL`, `WAHA_TIMEOUT_MS`, `CONNECTOR_HOST` e `CONNECTOR_PORT` conforme `.env.waha.example`. Não duplique chaves nem versione esse arquivo.
- O conector aceita apenas WAHA e API em loopback. Os scripts do conector carregam `.env.waha`; depois de `npm run build:connector`, inicie com `npm run start --workspace @chatpro/whatsapp-connector`.
- Smoke local: `GET /health`, `POST /instances` com `{ "id": "teste" }`, `POST /instances/teste/start`, `GET /instances/teste/status`, `GET /instances/teste/qr` e `POST /instances/teste/stop`. O QR é temporário e não deve ser salvo ou exibido em logs.
- A validação obrigatória inclui `npm run lint`, `npm run typecheck`, `npm run build`, `npm run test --workspace @chatpro/whatsapp-connector` e a remoção da sessão/processos de teste.

## Tela WhatsApp autenticada

- Defina somente `NEXT_PUBLIC_CONNECTOR_API_URL=http://127.0.0.1:3001` em `apps/web/.env.local`, conforme `apps/web/.env.example`. Essa variável contém apenas a URL local pública; credenciais WAHA permanecem em `.env.waha` e não chegam ao navegador.
- Com WAHA, conector e web na mesma máquina, a tela `/app` permite criar, iniciar, consultar status, atualizar QR e parar a única instância `chatpro-main`.
- Teste a camada cliente com `npm run test:connector-client`. Para smoke autenticado real, são necessárias as variáveis públicas do Supabase em `apps/web/.env.local`; elas não foram criadas pelo Codex.
- Não há mensagens, múltiplas instâncias, persistência do QR, CRM, tabelas ou integração Supabase adicional nesta fase.

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
