# Referência dos testes

## Execução

[`bin/test.ts`](../../bin/test.ts) carrega os plugins `@t8ngs/expect` e `@t8ngs/expect-type`, configura o padrão `tests/**/*.spec.ts` e chama `run()`. O script `quick:test` inicia o runner com `@jeftelabs/exec` e source maps.

`npm test` executa `pretest` (lint) e `c8 npm run quick:test`, com relatórios text e HTML. `c8` exclui `tests/**` do relatório de cobertura de código. Os scripts oficiais estão em `package.json`.

## Contratos cobertos atualmente

[`tests/exception.spec.ts`](../../tests/exception.spec.ts) contém casos para:

- mensagem, código e status por instância;
- stack trace em `Exception` e em subclasses;
- metadados estáticos `message`, `status`, `code` e `help`;
- encadeamento de causa por `ErrorOptions.cause`;
- mensagem estática quando o construtor não recebe mensagem;
- renderização textual com e sem código;
- `Symbol.toStringTag` da subclasse;
- mensagem vazia da classe base;
- `createError` com e sem argumentos de formatação e nome da classe gerada.

## Manutenção

Ao alterar um contrato, ajuste os testes comportamentais relacionados e a documentação de API/uso. Para código novo, adicione casos de sucesso e de borda adequados ao comportamento; não dependa apenas de snapshots ou de descrição no README. Confirme os comandos em `package.json` antes de usá-los.
