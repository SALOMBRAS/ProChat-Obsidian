# Mapa de pastas do Main

Raiz local: `C:\Projeto Salo\ChatPro\ChatPro Main`

Este mapa descreve a estrutura real após a Fase 1. Diretórios gerados e ignorados, como `node_modules`, `.next` e `dist`, não fazem parte da estrutura versionada.

```text
ChatPro Main/
├── apps/
│   └── web/
│       ├── src/app/
│       │   ├── globals.css
│       │   ├── layout.tsx
│       │   └── page.tsx
│       ├── eslint.config.mjs
│       ├── next.config.ts
│       ├── package.json
│       ├── postcss.config.mjs
│       └── tsconfig.json
├── services/
│   └── whatsapp-connector/
│       ├── src/index.ts
│       ├── package.json
│       └── tsconfig.json
├── packages/
│   ├── shared/README.md
│   ├── database/README.md
│   └── whatsapp-core/README.md
├── scripts/setup-kit-mcp.ps1
├── package.json
├── package-lock.json
└── tsconfig.base.json
```

## Raiz do workspace

- `package.json`: declara npm workspaces, versões mínimas do ambiente e scripts centralizados.
- `package-lock.json`: único lockfile dos workspaces.
- `tsconfig.base.json`: opções TypeScript comuns.
- `.nvmrc`: fixa Node.js `24.16.0` para ambientes compatíveis com nvm.
- `.editorconfig`: regras mínimas de codificação, indentação e finais de linha.
- `AGENTS.md`: regras operacionais do repositório e do Cofre.

## Frontend

`apps/web` contém o workspace `@chatpro/web`, criado pelo `create-next-app 16.2.10` com App Router, TypeScript, Tailwind CSS e ESLint.

- `src/app/page.tsx`: página inicial neutra da fundação.
- `src/app/layout.tsx`: metadata acadêmico e idioma `pt-BR`.
- `src/app/globals.css`: Tailwind e estilos globais mínimos.
- `next.config.ts`: configuração estável sem opções experimentais.

O frontend não contém backend de negócio, autenticação, banco, Supabase, cliente WhatsApp ou CRM.

## Conector

`services/whatsapp-connector` contém o workspace `@chatpro/whatsapp-connector`.

`src/index.ts` apenas registra a inicialização, mantém o processo ativo e trata `SIGINT` e `SIGTERM`. O build TypeScript é emitido em `dist`, que permanece ignorado.

Não existe HTTP, WebSocket, fila, sessão, QR Code, Baileys, Supabase ou conexão externa.

## Pacotes reservados

- `packages/shared`: futuros tipos, validações e utilitários compartilhados.
- `packages/database`: futura camada de banco e Supabase.
- `packages/whatsapp-core`: futuros contratos normalizados e `WhatsAppProvider`.

Esses diretórios contêm apenas README e não são workspaces npm ativos. Não existe código placeholder nem interface `WhatsAppProvider` nesta fase.

## Fronteiras e impactos cruzados

- O frontend não deve incorporar o ciclo de vida persistente do conector.
- O conector não deve importar Next.js nem implementar regras visuais.
- Baileys ficará restrito a um adaptador futuro e não poderá vazar para as demais camadas.
- Alterações em `package.json`, `package-lock.json` ou `tsconfig.base.json` podem afetar web e conector e exigem validação completa.
- Quando os pacotes reservados ganharem código, seus contratos poderão afetar os dois workspaces e deverão ser documentados antes da adoção.
