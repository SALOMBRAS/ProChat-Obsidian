# Changelog

## 2026-07-15 — Fundação técnica

- Criação do npm workspace raiz com Node.js `24.16.0` e npm `11.16.0`.
- Criação do frontend `@chatpro/web` com Next.js `16.2.10`, App Router, TypeScript, Tailwind CSS e ESLint.
- Criação do serviço `@chatpro/whatsapp-connector` com build TypeScript e encerramento por sinais.
- Reserva documental dos futuros pacotes `shared`, `database` e `whatsapp-core`.
- Centralização dos scripts de desenvolvimento, lint, typecheck, build e check.
- Validação de lint, typecheck, builds e smoke tests da web e do conector.
- Registro de dois avisos moderados transitivos de PostCSS, sem correção forçada ou upgrade instável.
- Confirmação de que Supabase, Baileys, WhatsApp, QR Code, CRM, PWA e deploy permanecem não implementados.

## 2026-07-15 — Integração seletiva do Kit-MCP

- Registro local do Kit-MCP `1.46.0` como servidor STDIO do Codex.
- Restrição inicial ao tool consultivo `kit` e ao pack `core` como política de uso.
- Criação do script idempotente de configuração para os dois computadores.
- Registro da decisão arquitetural de manter o Kit-MCP fora do runtime.
- Nenhuma projeção de packs, dependência ou funcionalidade do aplicativo adicionada.

## 2026-07-15 — Inicialização documental

- Inicialização independente dos repositórios Main e Cofre.
- Criação da branch `main` em ambos.
- Criação da estrutura documental inicial do Cofre.
- Criação do `AGENTS.md` operacional no projeto principal.
- Registro das decisões arquiteturais iniciais.
- Confirmação de que não existe código funcional nesta fase.
## 2026-07-15 — Fundação inicial do Supabase

- Criado o projeto gratuito `ChatPro` na região `sa-east-1`, com instância `nano`.
- Adicionados os clientes browser e server do Supabase em `apps/web/src/lib/supabase` e o modelo `apps/web/.env.example`, sem valores reais.
- Adicionado diagnóstico removível do endpoint de saúde do Supabase; autenticação, tabelas, migrações e demais integrações permanecem fora de escopo.
## 2026-07-15 — Autenticação básica com Supabase

- Implementados cadastro, login, logout, callback de confirmação e sessão SSR por cookies, com proteção restrita a `/app`.
- Mantidos fora de escopo OAuth, perfis, tabelas e qualquer funcionalidade de WhatsApp ou CRM.

# 2026-07-15 — Serviço WAHA local

- Preparado o WAHA Core local em Docker Compose, isolado em loopback, com estado de sessão ignorado pelo Git e sem integração com a aplicação.
- Documentados os comandos locais, endpoints de operação, persistência, limites e o uso futuro por adaptador próprio.
# 2026-07-15 — Abstração WAHA no conector

- Adicionados o contrato `WhatsAppProvider`, o adaptador WAHA e a API local do conector com modelos ChatPro normalizados.
- Mantidos fora de escopo frontend, Supabase, mensagens e qualquer leitura de QR Code além da validação local.
