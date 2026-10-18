# Desenvolvimento e manutenção

## Pré-requisitos e instalação

O pacote é TypeScript/ESM, usa Node.js e npm e declara dependências de desenvolvimento em [`package.json`](../../package.json). Instale-as com `npm install`. O repositório não contém lockfile versionado no estado analisado.

O runner `@jeftelabs/exec` lê `tsconfig.json` para executar imports `.ts` diretamente. A configuração estende `@arapucajs/tsconfig/tsconfig.package.json`, dependência listada, e habilita `rewriteRelativeImportExtensions`. Ao mudar imports ou configuração, preserve a execução no Node e a reescrita compatível com o build.

## Estrutura de trabalho

1. Consulte [`AGENTS.md`](../../AGENTS.md) e a página de referência da área.
2. Confirme comportamento em `src/` e o contrato esperado em `tests/`.
3. Altere o código-fonte, não arquivos gerados dentro de `build/`.
4. Atualize a documentação detalhada em `docs/` na mesma alteração. Atualize o README quando impactar usuários ou a API.
5. Confira scripts e workflows antes de alterar dependências, build ou publicação.

## Comandos disponíveis

| Comando | Efeito |
| --- | --- |
| `npm run quick:test` | Executa `bin/test.ts` com `@jeftelabs/exec`. |
| `npm test` | Executa `pretest` (lint) e testes com relatório `c8`. |
| `npm run lint` | Executa ESLint no repositório. |
| `npm run typecheck` | Executa `tsc --noEmit`. |
| `npm run compile` | Executa lint, `tsdown` e emissão de declarações TypeScript. |
| `npm run build` | Encaminha para `compile`. |
| `npm run format` | Executa Prettier com escrita nos arquivos. |

Use apenas verificações pertinentes à alteração e não execute comandos de escrita sem necessidade.

## Testes

Os testes usam `@t8ngs/runner` e plugins de assertions configurados em `bin/test.ts`. O runner busca `tests/**/*.spec.ts`. `tests/exception.spec.ts` cobre metadados, defaults, subclasses, `cause`, stack trace, `toString`, `Symbol.toStringTag` e `createError`. Ao alterar qualquer contrato, atualize casos relevantes e esta documentação de testes.

## Build e distribuição

`tsdown` empacota `index.ts` em ESM sob `build/`; `tsc --emitDeclarationOnly --declaration` gera declarações. O `package.json` aponta `main` e `exports["."]` para `build/index.js`, e inclui arquivos `build` no pacote excluindo `build/bin` e `build/tests`.

## CI e publicação

- `.github/workflows/checks.yml` valida mensagens de commit no push/PR; embora seu nome seja checks, não executa testes nem build.
- `release.yml` aciona release-please em pushes para `main`; se criar release, constrói e anexa o tarball.
- `deploy.yml` publica em GitHub Packages e npm após release publicada.
- `labels.yml` e `stale.yml` são automações de gestão de issues/PRs.
- `.husky/pre-commit` roda `npm test` localmente; um commit pode, portanto, executar lint e testes com cobertura.

Reavalie estes detalhes nos arquivos de workflow e hooks se a configuração mudar. Credenciais e políticas de release pertencem à configuração do GitHub Actions, não a este pacote.
