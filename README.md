# `@jeftelabs/exception`

Biblioteca TypeScript/ESM para criar exceções personalizadas com nome, mensagem, código, status, descrição de ajuda e causa. Também oferece classes de erro comuns e uma fábrica para mensagens formatadas.

## Instalação

```sh
npm install @jeftelabs/exception
```

O pacote publica o entry point `@jeftelabs/exception` como ESM e inclui declarações TypeScript. O processo de publicação constrói os arquivos em `build/`.

## Uso

### Subclasse com metadados estáticos

```ts
import { Exception } from '@jeftelabs/exception'

class UserNotFoundException extends Exception {
  static message = 'Usuário não encontrado'
  static code = 'E_USER_NOT_FOUND'
  static status = 404
  static help = 'Confira se o identificador do usuário está correto.'
}

throw new UserNotFoundException()
```

Uma instância usa a mensagem, o código, o status e a ajuda da classe quando aplicável. Também é possível passar mensagem e opções no construtor:

```ts
const error = new Exception('Acesso negado', {
  code: 'E_FORBIDDEN',
  status: 403,
  cause: new Error('Permissão ausente'),
})
```

O status padrão é `500`, e a mensagem padrão é a string vazia se nenhuma mensagem for fornecida. Os detalhes de prioridade e formatação estão na [referência da API](./docs/references/api.md).

### Exceções predefinidas

```ts
import { InvalidArgumentsException, RuntimeException } from '@jeftelabs/exception'

throw new InvalidArgumentsException('O identificador é inválido')
```

`InvalidArgumentsException` define o código `E_INVALID_ARGUMENTS_EXCEPTION`; `RuntimeException` define `E_RUNTIME_EXCEPTION`. Ambas mantêm status `500`.

### Fábrica `createError`

```ts
import { createError } from '@jeftelabs/exception'

const E_RESOURCE_NOT_FOUND = createError<[number]>(
  'Recurso com id %d não encontrado',
  'E_RESOURCE_NOT_FOUND',
  404
)

throw new E_RESOURCE_NOT_FOUND([42])
```

Os placeholders e valores seguem `util.format` do Node.js. A fábrica recebe os argumentos de formatação como array no construtor. Consulte o [guia de uso](./docs/guides/using-exceptions.md) para exemplos e limites.

## Documentação

A [documentação completa](./docs/README.md) explica a arquitetura, a API, o uso, os testes, o build, a publicação e as decisões de planejamento. As regras de manutenção e a exigência de documentação para toda alteração estão em [`AGENTS.md`](./AGENTS.md).

## Desenvolvimento

```sh
npm install
npm run quick:test
npm run lint
npm run typecheck
npm run build
```

Os scripts e seu funcionamento estão descritos no [guia de desenvolvimento](./docs/guides/development.md). `npm test` executa lint e testes com cobertura; `npm run format` modifica os arquivos.

## Licença

MIT. Consulte [`LICENSE`](./LICENSE).
