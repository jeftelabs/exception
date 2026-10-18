# Visão geral e arquitetura

## Objetivo

`@jeftelabs/exception` fornece uma classe base para erros JavaScript personalizados com metadados úteis para diagnóstico e tratamento: código, status e texto de ajuda. Inclui duas subclasses comuns e uma função que fabrica uma classe com mensagem formatável.

O pacote não implementa um middleware HTTP, logging, serialização JSON ou um sistema de registro de erros. `status` é apenas um número carregado na instância; cabe à aplicação decidir como convertê-lo em resposta.

## Organização

- [`index.ts`](../../index.ts) reexporta tudo de `src/exception.ts` e é a única entrada declarada pelo pacote.
- [`src/exception.ts`](../../src/exception.ts) contém `Exception`, as subclasses predefinidas e `createError`.
- [`tests/exception.spec.ts`](../../tests/exception.spec.ts) cobre os contratos comportamentais observados.
- [`bin/test.ts`](../../bin/test.ts) configura os plugins do runner e executa os arquivos `tests/**/*.spec.ts`.
- `tsdown` gera o entry point JavaScript ESM e `tsc` gera declarações em `build/`.

## Fluxo de criação de uma exceção

1. O chamador instancia `Exception` ou uma subclasse.
2. O construtor de `Exception` chama `Error` com a mensagem recebida e `ErrorOptions`, incluindo `cause` quando informado.
3. O nome vem do construtor real da instância. A mensagem usa o argumento se for truthy; caso contrário, usa `static message`, ou `''`.
4. O status usa `options.status`, depois `static status`, depois `500` (com operadores `||`, portanto valores falsy também caem no próximo padrão).
5. O código usa `options.code` ou `static code`. A ajuda vem de `static help`; ela não é uma opção do construtor.
6. `Error.captureStackTrace` ajusta a pilha para apontar a partir da classe concreta.

As prioridades exatas e exceções relevantes estão detalhadas na [referência da API](../references/api.md).

## `createError`

A fábrica retorna uma subclasse anônima de `Exception`, com `static message`, `static code` e `static status`. Seu construtor aplica `node:util` `format` à mensagem e ao array de argumentos, e define o nome observado como `Exception`. A forma de fornecer o array, inclusive quando não há placeholders, está no [guia de uso](./using-exceptions.md).

## Decisões visíveis no código

- O pacote herda de `Error` nativo e preserva `ErrorOptions`/`cause`.
- O status padrão é 500.
- `toString()` acrescenta `[code]` quando `code` é truthy.
- `Symbol.toStringTag` retorna o nome do construtor da instância; `createError` sobrescreve o `name` da instância para `Exception`.
- A configuração usa ESM e imports relativos com extensão `.ts`, reescritos no build por `rewriteRelativeImportExtensions`.

Essas observações descrevem o estado atual, não garantias além do que testes e código confirmam. Mudanças nestas decisões devem atualizar os testes e esta documentação.
