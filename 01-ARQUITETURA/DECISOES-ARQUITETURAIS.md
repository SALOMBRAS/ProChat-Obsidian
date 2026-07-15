# Decisões arquiteturais

As decisões registradas aqui representam direções aprovadas, não componentes já implementados.

| ID | Data | Decisão | Justificativa | Consequências | Status |
|---|---|---|---|---|---|
| ADR-001 | 2026-07-15 | Manter Main e Cofre em dois repositórios independentes | Separar código e memória técnica | Commits, remotos e sincronização permanecem separados | Aceita |
| ADR-002 | 2026-07-15 | Usar o Cofre como memória técnica externa | Fornecer contexto operacional objetivo ao Codex | Documentos devem registrar caminhos reais e ser atualizados com mudanças relevantes | Aceita |
| ADR-003 | 2026-07-15 | Usar Next.js para o frontend | Atender web e futura PWA com uma base conhecida | Fundação materializada em `apps/web`; PWA continua futura | Aceita |
| ADR-004 | 2026-07-15 | Planejar Supabase | Reunir PostgreSQL, Auth, Storage e Realtime | Configuração e modelo de dados dependem de fase própria | Planejada |
| ADR-005 | 2026-07-15 | Usar serviço Node.js persistente separado | Conexões WhatsApp exigem ciclo de vida diferente do frontend | Processo-base criado em `services/whatsapp-connector`; integração continua futura | Aceita |
| ADR-006 | 2026-07-15 | Isolar Baileys em um adaptador | Baileys não é API oficial e pode precisar ser substituído | Nenhuma aplicação poderá importar Baileys diretamente | Planejada |
| ADR-007 | 2026-07-15 | Definir o contrato `WhatsAppProvider` | Desacoplar regras de negócio do provedor | Providers futuros devem implementar o mesmo contrato normalizado | Planejada |
| ADR-008 | 2026-07-15 | Reservar Vercel somente para o frontend | Funções efêmeras não atendem ao conector persistente planejado | O conector exigirá ambiente apropriado no futuro | Aceita |
| ADR-009 | 2026-07-15 | Preparar um aplicativo móvel futuro | Permitir outro cliente sem duplicar serviços e regras | APIs e modelos devem evitar acoplamento exclusivo à web | Planejada |
| ADR-010 | 2026-07-15 | Priorizar custo zero na fase acadêmica | Evitar cobrança durante desenvolvimento e apresentação | Serviços pagos dependem de autorização posterior | Aceita |
| ADR-011 | 2026-07-15 | Preferir automação por CLI e APIs oficiais | Tornar procedimentos reproduzíveis e auditáveis | Intervenção manual fica reservada a ações exclusivas do titular | Aceita |
| ADR-012 | 2026-07-15 | Proibir serviços pagos sem autorização expressa | Evitar custos e compromissos não aprovados | Nenhuma contratação ou ativação paga pode ser presumida | Aceita |
| ADR-013 | 2026-07-15 | Usar Kit-MCP seletivamente e fora do runtime | Reaproveitar padrões consultivos sem ampliar dependências nem contexto desnecessário | Versão fixada, pack `core` como seleção inicial, allowlist consultiva e projeções somente após revisão expressa | Aceita |
| ADR-014 | 2026-07-15 | Usar npm workspaces sem orquestrador adicional | Manter a fundação pequena e suficiente para dois workspaces | Scripts ficam centralizados na raiz; Turborepo, Nx e Lerna ficam fora do escopo | Aceita |
| ADR-015 | 2026-07-15 | Adotar Node.js 24.16.0 e npm 11.16.0 | Reproduzir o ambiente auditado e compatível com Next.js 16 | Versões registradas em `.nvmrc`, `packageManager` e `engines` | Aceita |
| ADR-016 | 2026-07-15 | Gerar o frontend com create-next-app 16.2.10 | Partir do template oficial estável | App Router, TypeScript, Tailwind e ESLint seguem a configuração oficial sem recursos experimentais | Aceita |
| ADR-017 | 2026-07-15 | Criar somente o ciclo de vida mínimo do conector | Validar a fronteira de processo sem antecipar integrações | Serviço inicia, aguarda sinais e encerra; não abre rede nem acessa serviços externos | Aceita |
| ADR-018 | 2026-07-15 | Manter pacotes compartilhados apenas reservados | Evitar APIs e placeholders antes dos requisitos reais | `shared`, `database` e `whatsapp-core` não possuem `package.json` ou código | Aceita |
| ADR-019 | 2026-07-15 | Adiar Supabase e Baileys para fases próprias | Evitar acoplamento e implementação prematura | Nenhuma dependência, configuração, credencial ou sessão dessas integrações existe na Fase 1 | Aceita |
