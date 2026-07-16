# Arquitetura do ChatPro

Este documento separa a fundação já existente das integrações ainda planejadas.

## Fundação implementada na Fase 1

- **Workspace:** npm workspaces sem Turborepo, Nx ou outro orquestrador.
- **Frontend web:** Next.js com App Router, TypeScript, Tailwind CSS e ESLint em `apps/web`.
- **Autenticação inicial:** Supabase Auth com e-mail e senha, clientes SSR em `apps/web/src/lib/supabase`, sessão por cookies e proteção limitada à rota `/app`.
- **Conector persistente:** serviço Node.js separado em `services/whatsapp-connector`, ainda sem integração externa.
- **Pacotes reservados:** `shared`, `database` e `whatsapp-core` contêm somente documentação de responsabilidade futura.

## WAHA local preparado

- WAHA Core 2026.6.2 está preparado somente em Docker Compose pela imagem oficial fixada em digest, isolado em loopback e com sessões persistidas localmente.
- Há cliente WAHA somente no conector local, por adaptador e contrato próprio `WhatsAppProvider`; não há integração com frontend, Supabase ou fluxo de mensagens.

## Contrato e adaptador WAHA

- `packages/whatsapp-core/src/index.ts` define `WhatsAppProvider`, estados normalizados, resultados de instância/QR e `WhatsAppProviderError`; o contrato não expõe modelos do WAHA.
- `services/whatsapp-connector/src/waha-provider.ts` implementa esse contrato por `WahaClient`; `waha-mappers.ts` converte estados WAHA para modelos ChatPro.
- `services/whatsapp-connector/src/server.ts` expõe apenas em loopback `GET /health`, `POST /instances`, `GET /instances/:id/status`, `GET /instances/:id/qr`, `POST /instances/:id/start` e `POST /instances/:id/stop`.
- Web e APK futuros devem consumir a API/contrato ChatPro, nunca respostas diretas do WAHA. Mensagens, frontend e Supabase permanecem fora de escopo.

## Interface autenticada da instância

- `apps/web/src/app/app/page.tsx` inclui a seção WhatsApp somente para o usuário autenticado; `whatsapp-instance.tsx` gerencia uma instância fixa em memória e não persiste o QR Code.
- `apps/web/src/lib/whatsapp-connector/client.ts` é o único cliente web da API própria do conector. O navegador não chama WAHA e não recebe credenciais WAHA.
- Os estados visíveis são Não criada, Parada, Iniciando, Aguardando QR, Conectada e Erro. O QR é atualizado sob ação do usuário e descartado ao parar, conectar ou ocorrer erro.
- A API do conector continua limitada a loopback; CORS permite somente `http://localhost:3000` e `http://127.0.0.1:3000`. Web e APK futuros devem continuar consumindo apenas esse contrato ChatPro.

## Componentes planejados

- **PWA e hospedagem:** evolução futura do frontend; Vercel permanece apenas uma opção futura para o frontend.
- **Dados e serviços gerenciados:** Supabase Auth está limitado ao fluxo inicial de e-mail e senha; PostgreSQL de aplicação, Storage e Realtime permanecem futuros.
- **Integração WhatsApp:** Baileys restrito ao adaptador, acessível pela aplicação somente por um contrato próprio `WhatsAppProvider`.
- **Modelos internos:** mensagens e eventos normalizados em modelos próprios, sem expor tipos específicos do provider às demais camadas.
- **Cliente móvel futuro:** APK consumindo os mesmos serviços e regras de negócio usados pelo frontend.

A interface, os dados, as regras de negócio e o provedor do WhatsApp devem permanecer separados. Essa divisão permite substituir futuramente o Baileys pela Meta Cloud API ou outro provider.

O desenvolvimento acadêmico prioriza opções gratuitas. Nenhuma hospedagem paga faz parte da fase atual, e a Vercel é planejada somente para o frontend, não para o conector persistente.

## Persistência local do backend web (15/07/2026)

O checkout atual também possui o workspace `web/`, com API Express em `web/apps/api` e contratos em `web/packages/contracts`. A API ganhou uma fundação SQLite local, exclusivamente para desenvolvimento, via `better-sqlite3`: migration versionada em `web/apps/api/migrations/001_initial_persistence.sql`, journal `schema_migrations` e banco ignorado em `web/.chatpro-data/backend.sqlite` (ou `CHATPRO_DATABASE_PATH`).

Os adaptadores SQLite em `web/apps/api/src/persistence/` expõem repositórios por workspace para contatos, tags, opt-out, templates, CRM, campanhas e configurações. O isolamento usa chaves compostas com `workspaceId`; regras e futuras camadas de serviço não dependem diretamente do driver. A função `initializeWorkspaceCrm` é a única que cria etapas CRM padrão, e não é chamada no runtime normal.

Não existem endpoints CRUD novos, dados de exemplo, sessões, QR, credenciais ou conexão WhatsApp nessa fundação. PostgreSQL/Supabase continua uma troca futura de adaptador, não uma dependência ativa.

## Diagrama textual

```text
[Web Next.js existente] -----+
                              +--> [Serviços e regras futuros] --> [Supabase futuro]
[Aplicativo móvel futuro] ----+                 |
                                                v
                              [Serviço Node existente, sem integração]
                                                |
                                 [WhatsAppProvider futuro]
                                                |
                                   [Adaptador Baileys futuro]
                                                |
                                        [WhatsApp futuro]
```
