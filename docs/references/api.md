# Referência da API pública

A API pública é reexportada por [`index.ts`](../../index.ts). A implementação está em [`src/exception.ts`](../../src/exception.ts). Tipos abaixo descrevem os tipos explícitos e o comportamento implementado.

## `Exception`

```ts
class Exception extends Error {
  constructor(message?: string, options?: ErrorOptions & { code?: string; status?: number })
  name: string
  message: string
  status: number
  code?: string
  help?: string
  toString(): string
}
```

Também declara os campos estáticos opcionais `help`, `code`, `status` e `message` para subclasses.

### Inicialização e precedência

- Chama `Error(message, options)`, preservando os recursos de `ErrorOptions`, incluindo `cause`.
- Define `name` como o nome do construtor concreto.
- Define `message` como `message || Constructor.message || ''`. Mensagens vazias usam a mensagem estática, se houver.
- Define `status` como `options.status || Constructor.status || 500`. Valores falsy, inclusive `0`, não são mantidos como status.
- Define `code` como `options.code || Constructor.code`; só atribui a propriedade se o resultado não for `undefined`. Código vazio em `options` cai no código estático.
- Copia `Constructor.help` se não for `undefined`. Não há opção de ajuda no construtor.
- Chama `Error.captureStackTrace(this, Constructor)`.

### Métodos e propriedades especiais

- `toString()` retorna `${name} [${code}]: ${message}` se `code` for truthy; caso contrário `${name}: ${message}`.
- `Symbol.toStringTag` retorna `this.constructor.name`, portanto `Object.prototype.toString.call(error)` resulta em `[object NomeDaClasse]` para uma subclasse nomeada.

## `InvalidArgumentsException`

Subclasse de `Exception`, exportada diretamente. Declara `static code = 'E_INVALID_ARGUMENTS_EXCEPTION'` e `static status = 500`.

## `RuntimeException`

Subclasse de `Exception`, exportada diretamente. Declara `static code = 'E_RUNTIME_EXCEPTION'` e `static status = 500`.

## `createError`

```ts
function createError<T extends any[] = never>(
  message: string,
  code: string,
  status?: number
): Constructor
```

Retorna uma classe que herda `Exception`. A classe define a mensagem, código e status fornecidos como propriedades estáticas. O construtor chama `node:util` `format(message, ...(args || []))`, passa `ErrorOptions` a `Exception` e depois define o nome da instância para `Exception`.

Para `T` diferente de `never`, o construtor gerado recebe o tuple `T` como primeiro argumento. Para o caso padrão, o tipo publicado permite omitir o argumento ou fornecer `any`; confira as declarações geradas quando precisar da assinatura completa numérica.

## Exportações

O único subpath publicado é `@jeftelabs/exception`. Não há subpaths públicos adicionais declarados em `package.json`.
