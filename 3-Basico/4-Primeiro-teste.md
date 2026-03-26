# Primeiro Teste

Vamos criar uma suite de testes completa usando a API pública [JSONPlaceholder](https://jsonplaceholder.typicode.com/).

## Estrutura da Classe de Teste

```java
// src/test/java/com/meuapp/tests/PostTest.java
package com.meuapp.tests;

import io.restassured.RestAssured;
import io.restassured.http.ContentType;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

@DisplayName("Testes da API de Posts")
public class PostTest {

    @BeforeAll
    static void configurar() {
        RestAssured.baseURI = "https://jsonplaceholder.typicode.com";
    }

    @Test
    @DisplayName("Deve retornar lista de posts")
    void deveRetornarListaDePosts() {
        given()
        .when()
            .get("/posts")
        .then()
            .statusCode(200)
            .body("size()", equalTo(100))
            .body("[0].id", equalTo(1))
            .body("[0].userId", notNullValue());
    }

    @Test
    @DisplayName("Deve retornar post por ID")
    void deveRetornarPostPorId() {
        given()
            .pathParam("id", 1)
        .when()
            .get("/posts/{id}")
        .then()
            .statusCode(200)
            .body("id", equalTo(1))
            .body("title", notNullValue())
            .body("body", not(emptyString()));
    }

    @Test
    @DisplayName("Deve criar novo post")
    void deveCriarNovoPost() {
        String payload = """
            {
                "title": "Meu primeiro post",
                "body": "Conteúdo do post",
                "userId": 1
            }
            """;

        given()
            .contentType(ContentType.JSON)
            .body(payload)
        .when()
            .post("/posts")
        .then()
            .statusCode(201)
            .body("id", notNullValue())
            .body("title", equalTo("Meu primeiro post"))
            .body("userId", equalTo(1));
    }

    @Test
    @DisplayName("Deve atualizar post existente")
    void deveAtualizarPost() {
        String payload = """
            {
                "id": 1,
                "title": "Título atualizado",
                "body": "Conteúdo atualizado",
                "userId": 1
            }
            """;

        given()
            .contentType(ContentType.JSON)
            .body(payload)
            .pathParam("id", 1)
        .when()
            .put("/posts/{id}")
        .then()
            .statusCode(200)
            .body("title", equalTo("Título atualizado"));
    }

    @Test
    @DisplayName("Deve deletar post")
    void deveDeletarPost() {
        given()
            .pathParam("id", 1)
        .when()
            .delete("/posts/{id}")
        .then()
            .statusCode(200);
    }

    @Test
    @DisplayName("Deve retornar 404 para post inexistente")
    void deveRetornar404ParaPostInexistente() {
        given()
            .pathParam("id", 999999)
        .when()
            .get("/posts/{id}")
        .then()
            .statusCode(404);
    }
}
```

## Executando os Testes

```bash
# Todos os testes
mvn test

# Classe específica
mvn test -Dtest=PostTest

# Método específico
mvn test -Dtest=PostTest#deveRetornarListaDePosts
```

## Boas Práticas

- Use `@DisplayName` para descrições legíveis
- Use `@BeforeAll` para configurar a URL base uma única vez
- Cada teste deve verificar **um comportamento específico**
- Nomeie os métodos descritivamente: `deveFazerX`, `naoDevePermitirY`
- Verifique tanto o status code quanto o corpo da resposta

---

> Ir para: [Autenticação](../4-Intermediario/1-Autenticacao.md)
