# Verbos HTTP

O REST-assured suporta todos os verbos HTTP. A sintaxe BDD `given().when().then()` é usada em todos eles.

Usaremos a API pública [JSONPlaceholder](https://jsonplaceholder.typicode.com/) nos exemplos.

## GET — Buscar recursos

```java
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

// GET simples
given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .statusCode(200)
    .body("id", equalTo(1))
    .body("title", notNullValue());

// GET com lista
given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts")
.then()
    .statusCode(200)
    .body("size()", equalTo(100));
```

## POST — Criar recursos

```java
String novoPost = """
    {
        "title": "Meu Post",
        "body": "Conteúdo do post",
        "userId": 1
    }
    """;

given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .contentType("application/json")
    .body(novoPost)
.when()
    .post("/posts")
.then()
    .statusCode(201)
    .body("id", notNullValue())
    .body("title", equalTo("Meu Post"));
```

## PUT — Atualizar recurso completo

```java
String postAtualizado = """
    {
        "id": 1,
        "title": "Título Atualizado",
        "body": "Novo conteúdo",
        "userId": 1
    }
    """;

given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .contentType("application/json")
    .body(postAtualizado)
.when()
    .put("/posts/1")
.then()
    .statusCode(200)
    .body("title", equalTo("Título Atualizado"));
```

## PATCH — Atualizar recurso parcialmente

```java
String atualizacaoParcial = """
    {
        "title": "Apenas o título mudou"
    }
    """;

given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .contentType("application/json")
    .body(atualizacaoParcial)
.when()
    .patch("/posts/1")
.then()
    .statusCode(200)
    .body("title", equalTo("Apenas o título mudou"));
```

## DELETE — Remover recurso

```java
given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .delete("/posts/1")
.then()
    .statusCode(200);
```

## Extraindo a Resposta

Às vezes você precisa guardar a resposta para usar em outro lugar:

```java
import io.restassured.response.Response;

Response response = given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .extract().response();

int id = response.jsonPath().getInt("id");
String title = response.jsonPath().getString("title");
int statusCode = response.getStatusCode();
```

---

> Ir para: [Parâmetros e Headers](2-Parametros-e-Headers.md)
