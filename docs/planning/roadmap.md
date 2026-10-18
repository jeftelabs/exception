# Estado do projeto e critérios de evolução

## Escopo observado

O projeto entrega uma pequena biblioteca ESM de erros personalizados. A API atual consiste em `Exception`, `InvalidArgumentsException`, `RuntimeException` e `createError`, sem integrações de framework ou camada HTTP. Essa descrição vem do código exportado em `index.ts` e da implementação em `src/exception.ts`.

Este documento registra o estado observado e critérios para futuras propostas; não representa funcionalidades planejadas, datas ou compromissos de entrega.

## Critérios para propor mudanças

Ao propor uma mudança, registre claramente:

1. O caso de uso que a API atual não cobre.
2. O impacto na compatibilidade ESM, tipos publicados e comportamento herdado de `Error`.
3. As decisões de semântica (precedência, defaults, serialização e tipos), com exemplos.
4. Os testes de contrato e documentação necessários.
5. O efeito sobre build, pacote publicado, CI e release, caso aplicável.

## Áreas para revisão quando a API evoluir

- Contratos da classe base e `createError`: `docs/references/api.md` e `docs/guides/using-exceptions.md`.
- Organização e limites do pacote: `docs/guides/overview.md`.
- Testes e execução: `docs/references/tests.md` e `docs/guides/development.md`.
- Entradas do pacote, TypeScript, build e workflows: `docs/references/package-and-build.md`.
- Descoberta de documentos: `docs/README.md` e o README da raiz.

## Manutenção do planejamento

Só mova itens para um roadmap comprometido quando houver decisão explícita do mantenedor. Remova ou atualize afirmações obsoletas na mesma alteração que mudar o escopo real do pacote. Toda atualização deste planejamento também deve explicar o estado e sua fonte de evidência.
