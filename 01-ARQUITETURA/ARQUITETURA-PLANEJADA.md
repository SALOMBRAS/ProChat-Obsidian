# Arquitetura planejada

Este documento descreve uma direção futura. Os componentes abaixo ainda não foram implementados.

## Componentes

- **Frontend web/PWA:** Next.js, hospedável futuramente na Vercel.
- **Dados e serviços gerenciados:** Supabase para PostgreSQL, Auth, Storage e Realtime.
- **Conector persistente:** serviço Node.js separado, executado localmente durante o desenvolvimento e migrado futuramente para hospedagem apropriada.
- **Integração WhatsApp:** Baileys restrito ao adaptador, acessível pela aplicação somente por um contrato próprio `WhatsAppProvider`.
- **Modelos internos:** mensagens e eventos normalizados em modelos próprios, sem expor tipos específicos do provider às demais camadas.
- **Cliente móvel futuro:** APK consumindo os mesmos serviços e regras de negócio usados pelo frontend.

A interface, os dados, as regras de negócio e o provedor do WhatsApp devem permanecer separados. Essa divisão permite substituir futuramente o Baileys pela Meta Cloud API ou outro provider.

O desenvolvimento acadêmico prioriza opções gratuitas. Nenhuma hospedagem paga faz parte da fase atual, e a Vercel é planejada somente para o frontend, não para o conector persistente.

## Diagrama textual

```text
[Web/PWA Next.js] -----------+
                             +--> [Serviços e regras do ChatPro] --> [Supabase]
[Aplicativo móvel futuro] ---+                 |
                                               v
                                    [WhatsAppProvider]
                                               |
                                  [Adaptador Baileys inicial]
                                               |
                                         [WhatsApp]
```
