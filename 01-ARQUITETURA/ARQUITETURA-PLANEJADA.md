# Arquitetura do ChatPro

Este documento separa a fundação já existente das integrações ainda planejadas.

## Fundação implementada na Fase 1

- **Workspace:** npm workspaces sem Turborepo, Nx ou outro orquestrador.
- **Frontend web:** Next.js com App Router, TypeScript, Tailwind CSS e ESLint em `apps/web`.
- **Conector persistente:** serviço Node.js separado em `services/whatsapp-connector`, ainda sem integração externa.
- **Pacotes reservados:** `shared`, `database` e `whatsapp-core` contêm somente documentação de responsabilidade futura.

## Componentes planejados

- **PWA e hospedagem:** evolução futura do frontend; Vercel permanece apenas uma opção futura para o frontend.
- **Dados e serviços gerenciados:** Supabase para PostgreSQL, Auth, Storage e Realtime.
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
