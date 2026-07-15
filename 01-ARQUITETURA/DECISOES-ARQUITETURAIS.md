# Decisões arquiteturais

As decisões registradas aqui representam direções aprovadas, não componentes já implementados.

| ID | Data | Decisão | Justificativa | Consequências | Status |
|---|---|---|---|---|---|
| ADR-001 | 2026-07-15 | Manter Main e Cofre em dois repositórios independentes | Separar código e memória técnica | Commits, remotos e sincronização permanecem separados | Aceita |
| ADR-002 | 2026-07-15 | Usar o Cofre como memória técnica externa | Fornecer contexto operacional objetivo ao Codex | Documentos devem registrar caminhos reais e ser atualizados com mudanças relevantes | Aceita |
| ADR-003 | 2026-07-15 | Planejar Next.js para o frontend | Atender web e futura PWA com uma base conhecida | A escolha será materializada apenas na fase de fundação | Planejada |
| ADR-004 | 2026-07-15 | Planejar Supabase | Reunir PostgreSQL, Auth, Storage e Realtime | Configuração e modelo de dados dependem de fase própria | Planejada |
| ADR-005 | 2026-07-15 | Usar serviço Node.js persistente separado | Conexões WhatsApp exigem ciclo de vida diferente do frontend | O serviço não será hospedado na Vercel | Planejada |
| ADR-006 | 2026-07-15 | Isolar Baileys em um adaptador | Baileys não é API oficial e pode precisar ser substituído | Nenhuma aplicação poderá importar Baileys diretamente | Planejada |
| ADR-007 | 2026-07-15 | Definir o contrato `WhatsAppProvider` | Desacoplar regras de negócio do provedor | Providers futuros devem implementar o mesmo contrato normalizado | Planejada |
| ADR-008 | 2026-07-15 | Reservar Vercel somente para o frontend | Funções efêmeras não atendem ao conector persistente planejado | O conector exigirá ambiente apropriado no futuro | Aceita |
| ADR-009 | 2026-07-15 | Preparar um aplicativo móvel futuro | Permitir outro cliente sem duplicar serviços e regras | APIs e modelos devem evitar acoplamento exclusivo à web | Planejada |
| ADR-010 | 2026-07-15 | Priorizar custo zero na fase acadêmica | Evitar cobrança durante desenvolvimento e apresentação | Serviços pagos dependem de autorização posterior | Aceita |
| ADR-011 | 2026-07-15 | Preferir automação por CLI e APIs oficiais | Tornar procedimentos reproduzíveis e auditáveis | Intervenção manual fica reservada a ações exclusivas do titular | Aceita |
| ADR-012 | 2026-07-15 | Proibir serviços pagos sem autorização expressa | Evitar custos e compromissos não aprovados | Nenhuma contratação ou ativação paga pode ser presumida | Aceita |
| ADR-013 | 2026-07-15 | Usar Kit-MCP seletivamente e fora do runtime | Reaproveitar padrões consultivos sem ampliar dependências nem contexto desnecessário | Versão fixada, pack `core` como seleção inicial, allowlist consultiva e projeções somente após revisão expressa | Aceita |
