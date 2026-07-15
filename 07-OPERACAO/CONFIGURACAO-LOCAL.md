# Configuração local

## Requisitos

- Windows com Git disponível.
- Node.js `24.16.0`.
- npm `11.16.0`.
- Repositório Main em `C:\Projeto Salo\ChatPro\ChatPro Main`.

Não são necessários pnpm, Yarn, Bun, Python, Docker ou Supabase CLI para a Fase 1.

## Instalação

Na raiz do Main:

```powershell
npm install
```

O comando instala os workspaces `@chatpro/web` e `@chatpro/whatsapp-connector` e utiliza somente o `package-lock.json` da raiz.

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
- Supabase, autenticação, banco, Baileys, WhatsApp, QR Code, CRM, mensagens, mídias, PWA e deploy não estão configurados.
