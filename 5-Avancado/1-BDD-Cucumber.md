# BDD com Cucumber

Integrar REST-assured com Cucumber permite escrever testes de API em linguagem natural (Gherkin), facilitando a comunicação entre times técnicos e de negócio.

## Dependências

Adicione ao `pom.xml`:

```xml
<!-- Cucumber JUnit Platform -->
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-java</artifactId>
    <version>7.15.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-junit-platform-engine</artifactId>
    <version>7.15.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>org.junit.platform</groupId>
    <artifactId>junit-platform-suite</artifactId>
    <version>1.10.2</version>
    <scope>test</scope>
</dependency>
```

## Feature File (Gherkin)

```gherkin
# src/test/resources/features/posts.feature
# language: pt

Funcionalidade: Gerenciamento de Posts
  Como um usuário da API
  Quero gerenciar posts
  Para organizar meu conteúdo

  Cenário: Buscar post por ID válido
    Dado que a API está disponível em "https://jsonplaceholder.typicode.com"
    Quando faço uma requisição GET para "/posts/1"
    Então o status code da resposta deve ser 200
    E o campo "id" da resposta deve ser igual a 1
    E o campo "title" da resposta não deve ser vazio

  Cenário: Criar novo post
    Dado que a API está disponível em "https://jsonplaceholder.typicode.com"
    E tenho um payload com título "Meu Post BDD" e userId 1
    Quando faço uma requisição POST para "/posts"
    Então o status code da resposta deve ser 201
    E o campo "title" da resposta deve ser "Meu Post BDD"

  Esquema do Cenário: Buscar post inexistente
    Dado que a API está disponível em "https://jsonplaceholder.typicode.com"
    Quando faço uma requisição GET para "/posts/<id>"
    Então o status code da resposta deve ser 404

    Exemplos:
      | id     |
      | 99999  |
      | 100001 |
```

## Step Definitions

```java
// src/test/java/com/meuapp/steps/PostSteps.java
package com.meuapp.steps;

import io.cucumber.java.pt.*;
import io.restassured.response.Response;

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class PostSteps {

    private Response resposta;
    private String baseUri;
    private String payload;

    @Dado("que a API está disponível em {string}")
    public void apiDisponivel(String uri) {
        this.baseUri = uri;
    }

    @Dado("tenho um payload com título {string} e userId {int}")
    public void criarPayload(String titulo, int userId) {
        this.payload = String.format(
            "{\"title\": \"%s\", \"body\": \"conteudo\", \"userId\": %d}",
            titulo, userId
        );
    }

    @Quando("faço uma requisição GET para {string}")
    public void get(String endpoint) {
        resposta = given()
            .baseUri(baseUri)
        .when()
            .get(endpoint)
        .then()
            .extract().response();
    }

    @Quando("faço uma requisição POST para {string}")
    public void post(String endpoint) {
        resposta = given()
            .baseUri(baseUri)
            .contentType("application/json")
            .body(payload)
        .when()
            .post(endpoint)
        .then()
            .extract().response();
    }

    @Então("o status code da resposta deve ser {int}")
    public void verificarStatus(int statusEsperado) {
        resposta.then().statusCode(statusEsperado);
    }

    @E("o campo {string} da resposta deve ser igual a {int}")
    public void campoIgualA(String campo, int valor) {
        resposta.then().body(campo, equalTo(valor));
    }

    @E("o campo {string} da resposta deve ser {string}")
    public void campoIgualString(String campo, String valor) {
        resposta.then().body(campo, equalTo(valor));
    }

    @E("o campo {string} da resposta não deve ser vazio")
    public void campoNaoVazio(String campo) {
        resposta.then().body(campo, not(emptyString()));
    }
}
```

## Runner

```java
// src/test/java/com/meuapp/runner/CucumberRunner.java
package com.meuapp.runner;

import org.junit.platform.suite.api.*;

@Suite
@IncludeEngines("cucumber")
@SelectClasspathResource("features")
@ConfigurationParameter(key = "cucumber.glue", value = "com.meuapp.steps")
@ConfigurationParameter(key = "cucumber.plugin", value = "pretty, html:target/cucumber-report.html")
public class CucumberRunner {}
```

---

> Ir para: [Testes de Contrato](2-Testes-de-contrato.md)
