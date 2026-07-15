# Kit-MCP no ChatPro

## Finalidade e origem

O Kit-MCP é uma ferramenta auxiliar do Codex para consultar padrões, agentes, comandos e skills curados. Ele não faz parte do runtime do ChatPro e não deve substituir decisões registradas neste Cofre.

- Repositório oficial: <https://github.com/luanpdd/kit-mcp>
- Pacote npm: [`@luanpdd/kit-mcp`](https://www.npmjs.com/package/@luanpdd/kit-mcp)
- Versão configurada: `1.46.0`

O pacote npm declara o repositório oficial acima e foi executado sem instalação global por `npx -y @luanpdd/kit-mcp@1.46.0`.

## Registro no Codex

O servidor STDIO foi registrado como `kit-mcp` no arquivo local do usuário `~/.codex/config.toml`, que não deve ser versionado. O registro usa:

```text
npx -y @luanpdd/kit-mcp@1.46.0
```

A configuração desabilita a interface sidecar automática com `KIT_MCP_NO_UI=1`, exige aprovação em modo `prompt` e aplica `enabled_tools = ["kit"]`. O tool `kit` é consultivo: lista, busca e lê recursos do catálogo. Tools de sync, reverse-sync, install, auto-install, custos e outras ações permanecem bloqueadas inicialmente.

## Packs

- `core`: seleção operacional ativa desde o início.
- `supabase`: somente na fase Supabase e após revisão explícita.
- `ui`: somente na fase visual e após revisão explícita.
- `observability`, `legacy` e `cost-workflow`: somente diante de necessidade expressa.

Nenhum pack foi projetado em disco. Em particular, não foi executado `sync install`: o dry-run mostrou que o alvo Codex projeta regras e pode alcançar `AGENTS.md`. Não existe lockfile de packs no projeto. A seleção `core` é, nesta fase, uma política de uso do catálogo ao vivo.

## Diagnóstico

Execute no diretório do Main:

```powershell
npx -y @luanpdd/kit-mcp@1.46.0 -V
npx -y @luanpdd/kit-mcp@1.46.0 pack info core
npx -y @luanpdd/kit-mcp@1.46.0 doctor --project-root 'C:\Projeto Salo\ChatPro\ChatPro Main'
npx -y @luanpdd/kit-mcp@1.46.0 pack doctor --project-root 'C:\Projeto Salo\ChatPro\ChatPro Main'
```

Quando a CLI do Codex estiver executável, valide também:

```powershell
codex mcp list
codex mcp get kit-mcp
```

Uma nova thread ou reinicialização do Codex pode ser necessária para carregar um servidor adicionado ao `config.toml`. A disponibilidade nesta thread não deve ser presumida.

## Segundo computador

Depois de sincronizar Main e Cofre com `git pull --ff-only`, execute:

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File 'C:\Projeto Salo\ChatPro\ChatPro Main\scripts\setup-kit-mcp.ps1'
npx -y @luanpdd/kit-mcp@1.46.0 doctor --project-root 'C:\Projeto Salo\ChatPro\ChatPro Main'
```

O script é idempotente, configura somente `kit-mcp`, recusa sobrescrever uma seção divergente, preserva outros servidores MCP, fixa a versão e não exige segredos ou instalação global.

## Atualização, desativação e remoção

Para atualizar, não use `latest` automaticamente. Consulte primeiro o README, o changelog, a ajuda e os packs da versão desejada. Depois de revisão explícita, altere em conjunto a versão no script, neste documento e na seção `mcp_servers.kit-mcp` do `config.toml`; execute novamente o diagnóstico e abra uma nova thread.

Para desabilitar sem remover, defina `enabled = false` somente na seção `[mcp_servers.kit-mcp]`. Para remover, use `codex mcp remove kit-mcp` quando a CLI estiver disponível ou remova manualmente apenas essa seção. Não altere outros servidores.

## Limitações e impacto sobre tokens

- O pacote 1.46.0 expôs 16 tools no handshake realizado nesta configuração, embora o README ainda mencione 14; a allowlist libera somente `kit`.
- O wrapper CLI `kit install dry-run` não propagou suas opções nesta máquina. O registro foi feito pelo tool oficial `install` do próprio servidor após um dry-run correto.
- A CLI `codex` da instalação desktop retornou acesso negado no shell atual; por isso a validação por `codex mcp list` depende de reinicialização ou de outra superfície executável.
- Listagens completas consomem contexto. Prefira busca específica, `terse=true` nas listagens e leitura de um único recurso por vez.
- Não projete o kit completo, não use auto-install e não permita que o Kit-MCP substitua `AGENTS.md` ou este Cofre.

## Recursos relevantes conhecidos

- `kit`: tool MCP consultivo para `search`, `get` e listagens seletivas.
- `agent-safety-hard-rules` (`leve`): regras para auditorias somente leitura, proteção contra prompt injection e mascaramento de segredos.
- `plan-checker` (`leve`, `core`): verificação consultiva de cobertura de planos.
- `assumptions-analyzer` (`medio`, `core`): levantamento de hipóteses com evidências antes de planejar uma fase.
- `advisor-auditor` (`pesado`): orquestrador de auditoria; não usar por padrão e exigir justificativa prévia.
