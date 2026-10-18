# Referência de pacote, configuração e distribuição

## Identidade e entry point

[`package.json`](../../package.json) declara `@jeftelabs/exception`, versão inicial `1.0.0`, como pacote ESM (`type: module`). `main` e `exports["."]` apontam para `build/index.js`. O único entry point de fonte é [`index.ts`](../../index.ts), que reexporta `src/exception.ts`.

O campo `files` inclui `build`, excluindo `build/bin` e `build/tests`. O pacote publicado deve conter a saída JavaScript e declarações geradas pelo build.

## Configuração TypeScript

[`tsconfig.json`](../../tsconfig.json) estende `@arapucajs/tsconfig/tsconfig.package.json`, define `rootDir` como `./`, `outDir` como `./build` e ativa `rewriteRelativeImportExtensions`. A opção permite que imports relativos com `.ts` usados no desenvolvimento sejam convertidos para extensão JavaScript na saída.

O runner `@jeftelabs/exec` também lê o `tsconfig`. O `extends` precisa resolver para a dependência instalada e a opção de reescrita precisa permanecer ativa para o runner aceitar imports relativos `.ts`.

## Build

O campo `tsdown` define `index.ts` como entrada, `build` como saída, limpeza antes da compilação, formato ESM e target `esnext`. `npm run compile` executa `tsdown` e depois `tsc --emitDeclarationOnly --declaration`; `npm run build` encaminha para `compile`. `precompile` executa lint.

## Dependências de desenvolvimento

As dependências em `devDependencies` apoiam lint/format (`eslint`, `prettier` e configs ArapucaJS), teste (`@t8ngs/*`, `c8`), execução TypeScript (`@jeftelabs/exec`), compilação (`tsdown`, `typescript`) e Conventional Commits (`commitlint`, `commitizen`, `husky`). O código de runtime importa `node:util` do Node.js para `format`.

## Arquivos operacionais

- `.husky/pre-commit`: executa `npm test` antes de cada commit local.
- `.github/workflows/checks.yml`: valida Conventional Commits em pushes e PRs.
- `.github/workflows/release.yml`: release-please, build e upload do tarball quando uma release é criada.
- `.github/workflows/deploy.yml`: publicação após uma release publicada.
- `.npmrc`: desabilita geração de lockfile pelo npm no estado atual do repositório.

Workflows de labels e stale servem à administração de GitHub; consulte os próprios arquivos se essa automação for alvo de alteração.
