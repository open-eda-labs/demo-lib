# demo-lib

Biblioteca compartilhada do app de demonstração: `labs.demo:demo-lib`. Reúne o código que usa as dependências vulneráveis e não depende de servlet nem de Jetty (pacote `labs.demo.lib`, com `chaos`, `Jwt`, `Metrics`, `Config`, `Downstream`) e o `scenarios.yaml`. Só dados sintéticos ou públicos.

## Estado

Versão 1.0.0, parent `demo-parent` 1.0.0. Etapas do plano: P5 (N1). Decisões relacionadas: D-29, D-39, ADR-0005.

## Contexto

Este repositório faz parte do laboratório open-eda-labs: workflows agênticos disparados por eventos, com Argo Workflows e padrões abertos. Cada peça trocável é um repositório que só depende do contrato versionado em `triage-contract` (D-01, D-27).

A fonte da verdade é o [Plano Mestre](https://github.com/open-eda-labs/open-eda-labs.github.io/blob/main/docs/PLANO_MESTRE.md).

As regras do laboratório valem para todos os repositórios e estão no [perfil da organização](https://github.com/open-eda-labs).

## Regra de ouro (ADR-0005)

Nenhuma versão de terceiro é declarada aqui: vêm do [demo-parent](https://github.com/open-eda-labs/demo-parent). A correção de vulnerabilidade nasce no parent ou nesta biblioteca; o [demo-service](https://github.com/open-eda-labs/demo-service) só sobe a versão que consome. As versões estão vulneráveis de propósito: ignore Dependabot e alertas do GitHub e não corrija fora do pipeline do vuln fixer.

O pacote Java é `labs.demo.lib` (e `labs.demo.lib.chaos`), distinto do `labs.demo` do serviço, que tem `App` e `Routes`.

## Construir e publicar

JDK 21 e Maven 3.9. Instale o parent antes:

```sh
(cd ../demo-parent && mvn -B -ntp install)
mvn -B -ntp install
```

O push de uma tag `vX.Y.Z` roda `.github/workflows/release.yml` e publica em `https://maven.pkg.github.com/open-eda-labs/demo-lib`.

O release resolve o `demo-parent` do GitHub Packages com o `GITHUB_TOKEN` (`setup-java` com `server-id: github`). O CI instala o `demo-parent` da `main` (checkout do repositório público) no `~/.m2` do runner antes do build, então só fica verde depois de o PR do parent estar na `main`.
