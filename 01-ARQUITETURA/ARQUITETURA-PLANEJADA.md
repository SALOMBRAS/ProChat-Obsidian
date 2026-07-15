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

## Componentes planejados

- **PWA e hospedagem:** evolução futura do frontend; Vercel permanece apenas uma opção futura para o frontend.
- **Dados e serviços gerenciados:** Supabase Auth está limitado ao fluxo inicial de e-mail e senha; PostgreSQL de aplicação, Storage e Realtime permanecem futuros.
- **Integração WhatsApp:** Baileys restrito ao adaptador, acessível pela aplicação somente por um contrato próprio `WhatsAppProvider`.
- **Modelos internos:** mensagens e eventos normalizados em modelos próprios, sem expor tipos específicos do provider às demais camadas.
- **Cliente móvel futuro:** APK consumindo os mesmos serviços e regras de negócio usados pelo frontend.

A interface, os dados, as regras de negócio e o provedor do WhatsApp devem permanecer separados. Essa divisão permite substituir futuramente o Baileys pela Meta Cloud API ou outro provider.

O desenvolvimento acadêmico prioriza opções gratuitas. Nenhuma hospedagem paga faz parte da fase atual, e a Vercel é planejada somente para o frontend, não para o conector persistente.

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
