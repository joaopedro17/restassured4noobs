# CI/CD

Integrar testes REST-assured ao pipeline garante que APIs sejam verificadas automaticamente a cada mudança.

## GitHub Actions

```yaml
# .github/workflows/testes-api.yml
name: Testes de API com REST-assured

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  testes:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout do código
        uses: actions/checkout@v4

      - name: Configurar Java 21
        uses: actions/setup-java@v4
        with:
          java-version: '21'
          distribution: 'temurin'
          cache: maven

      - name: Executar testes
        run: mvn test
        env:
          API_BASE_URL: ${{ secrets.API_BASE_URL }}
          API_TOKEN: ${{ secrets.API_TOKEN }}

      - name: Publicar resultados dos testes
        uses: dorny/test-reporter@v1
        if: always()
        with:
          name: Resultados JUnit
          path: target/surefire-reports/*.xml
          reporter: java-junit

      - name: Upload do relatório de cobertura
        uses: actions/upload-artifact@v4
        if: always()
        with:
          name: relatorio-testes
          path: target/surefire-reports/
```

## Configuração do Maven Surefire para CI

```xml
<!-- pom.xml -->
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.5</version>
    <configuration>
        <!-- Formato de relatório para CI -->
        <reportFormat>xml</reportFormat>

        <!-- Continua mesmo com falhas para coletar todos os resultados -->
        <testFailureIgnore>false</testFailureIgnore>

        <!-- Passa variáveis de ambiente para os testes -->
        <systemPropertyVariables>
            <api.base.url>${env.API_BASE_URL}</api.base.url>
            <api.token>${env.API_TOKEN}</api.token>
        </systemPropertyVariables>
    </configuration>
</plugin>
```

## Testes Paralelos no CI

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.5</version>
    <configuration>
        <!-- Paralelismo por classes -->
        <parallel>classes</parallel>
        <threadCount>4</threadCount>

        <!-- Ou por métodos -->
        <!-- <parallel>methods</parallel> -->
        <!-- <threadCount>8</threadCount> -->
    </configuration>
</plugin>
```

## Multi-ambiente

```yaml
# Executar em múltiplos ambientes com matrix
jobs:
  testes:
    strategy:
      matrix:
        ambiente: [staging, producao]

    steps:
      - name: Executar testes em ${{ matrix.ambiente }}
        run: mvn test
        env:
          API_BASE_URL: ${{ secrets[format('API_URL_{0}', matrix.ambiente)] }}
          API_TOKEN: ${{ secrets[format('API_TOKEN_{0}', matrix.ambiente)] }}
```

## Jenkins

```groovy
// Jenkinsfile
pipeline {
    agent any

    tools {
        maven 'Maven 3.9'
        jdk 'Java 21'
    }

    stages {
        stage('Testes de API') {
            steps {
                withCredentials([
                    string(credentialsId: 'api-token', variable: 'API_TOKEN'),
                    string(credentialsId: 'api-url', variable: 'API_BASE_URL')
                ]) {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
    }
}
```

---

> Ir para: [Relatórios](4-Relatorios.md)
