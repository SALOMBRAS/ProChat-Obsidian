# Fluxo Codex e sincronização

Este fluxo mantém Main e Cofre sincronizados entre os dois computadores sem misturar seus históricos.

## Início de uma tarefa

1. No Main, execute `git status`. Se estiver limpo, atualize com `git pull --ff-only`.
2. Verifique se o Cofre existe no caminho padrão; se não existir, clone o remoto oficial.
3. No Cofre existente, execute `git status`. Se estiver limpo, atualize com `git pull --ff-only`.
4. Abra primeiro `INDEX.md`.
5. Abra somente os documentos relevantes, normalmente no máximo dois além do índice.
6. Execute apenas a tarefa autorizada.

Se qualquer repositório tiver alterações locais, não execute pull automaticamente: preserve o estado e avalie antes de prosseguir.

## Final de uma tarefa

1. Teste o código com lint, typecheck, testes e build, quando aplicável.
2. Atualize os documentos afetados no Cofre.
3. Revise separadamente as mudanças do Main e do Cofre.
4. Faça o commit do Main em seu próprio repositório.
5. Faça o commit do Cofre em seu próprio repositório.
6. Envie os dois repositórios aos respectivos remotos, sem force push.
7. Confirme `git status --short --branch` limpo e sincronizado em ambos.

## Conflitos

- Não use comandos destrutivos.
- Não apague, sobrescreva nem descarte alterações locais.
- Interrompa o fluxo quando houver conflito.
- Relate os arquivos e repositórios envolvidos.
- Solicite uma decisão quando a resolução não for inequívoca.

## Preferência de automação

Use, nesta ordem:

1. Configuração por arquivos.
2. CLI oficial.
3. API oficial.
4. Integração Git.
5. Acesso web automatizado.
6. Intervenção manual apenas para login, MFA, CAPTCHA, consentimento ou ações exclusivas do titular.
