# Guia de uso de exceções

## Instalar e importar

Instale `@jeftelabs/exception` pela ferramenta de pacotes do seu projeto e importe do entry point público:

```ts
import { Exception, createError } from '@jeftelabs/exception'
```

O pacote é ESM. Os nomes documentados correspondem às exportações atuais de [`index.ts`](../../index.ts).

## Criar uma subclasse nomeada

Defina metadados comuns como campos estáticos e instancie a classe:

```ts
import { Exception } from '@jeftelabs/exception'

export class PaymentRequiredException extends Exception {
  static message = 'Pagamento necessário'
  static code = 'E_PAYMENT_REQUIRED'
  static status = 402
  static help = 'Atualize o método de pagamento e tente novamente.'
}

throw new PaymentRequiredException()
```

O `name` da instância é o nome da classe. `static message` é usada quando não se passa uma mensagem truthy ao construtor. Um valor `static help` é copiado para a instância.

## Definir metadados por instância

O construtor aceita `(message?, options?)`. As opções estendem `ErrorOptions` com `code?: string` e `status?: number`:

```ts
const error = new Exception('Token expirado', {
  code: 'E_TOKEN_EXPIRED',
  status: 401,
  cause: new Error('Assinatura inválida'),
})
```

`code` e `status` passados na instância têm precedência quando truthy. `help` não é aceito em `options`; defina-o estaticamente na subclasse. Para os detalhes de precedência e valores falsy, consulte a [referência](../references/api.md).

## Usar subclasses fornecidas

O pacote exporta `InvalidArgumentsException` e `RuntimeException`. Seus códigos são, respectivamente, `E_INVALID_ARGUMENTS_EXCEPTION` e `E_RUNTIME_EXCEPTION`, e seus status estáticos são ambos `500`. A mensagem pode ser passada pelo construtor.

## Criar uma classe via `createError`

Use a fábrica para erros cujo formato é conhecido no ponto de declaração:

```ts
import { createError } from '@jeftelabs/exception'

const E_USER_NOT_FOUND = createError<[number]>(
  'Usuário %d não encontrado',
  'E_USER_NOT_FOUND',
  404
)

throw new E_USER_NOT_FOUND([123])
```

O primeiro tipo genérico representa o tuple de argumentos entregue a `util.format`. Os argumentos são passados em array como o primeiro argumento do construtor gerado. O parâmetro de status é opcional e, quando omitido, o padrão da classe `Exception` é 500. Sem parâmetros de formatação, a construção pode ser `new E_USER_NOT_FOUND()`.

## Converter para texto e tratar

`String(error)` chama `toString()`: `Nome [CODIGO]: mensagem` se `code` for truthy; caso contrário, `Nome: mensagem`. A biblioteca não cria resposta HTTP automaticamente. Um handler da aplicação pode inspecionar `status`, `code`, `help`, `message` e `cause` e decidir como apresentá-los.

## Limites

- A biblioteca não valida se os códigos ou status são válidos para um protocolo.
- `help` provém do campo estático da classe; não existe parâmetro de construtor para sobrescrevê-lo.
- Não presuma que `toString()` inclui `help`, `status`, `cause` ou stack trace.
- `createError` atribui o `name` da instância gerada como `Exception`; prefira uma subclasse nomeada quando precisar de um nome de classe próprio.
