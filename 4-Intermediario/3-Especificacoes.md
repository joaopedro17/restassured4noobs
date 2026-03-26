# Especificações Reutilizáveis

`RequestSpecification` e `ResponseSpecification` permitem centralizar configurações comuns, evitando repetição nos testes.

## RequestSpecification

Define configurações da requisição que serão reutilizadas:

```java
import io.restassured.builder.RequestSpecBuilder;
import io.restassured.specification.RequestSpecification;

// Cria a especificação base
RequestSpecification reqSpec = new RequestSpecBuilder()
    .setBaseUri("https://jsonplaceholder.typicode.com")
    .setContentType(ContentType.JSON)
    .addHeader("Accept", "application/json")
    .addHeader("X-Request-Source", "testes-automatizados")
    .build();

// Usa nos testes
given()
    .spec(reqSpec)
.when()
    .get("/posts/1")
.then()
    .statusCode(200);

given()
    .spec(reqSpec)
    .body(payload)
.when()
    .post("/posts")
.then()
    .statusCode(201);
```

## ResponseSpecification

Define validações da resposta que serão reutilizadas:

```java
import io.restassured.builder.ResponseSpecBuilder;
import io.restassured.specification.ResponseSpecification;

// Validações comuns a todas as respostas de sucesso
ResponseSpecification respostaOk = new ResponseSpecBuilder()
    .expectStatusCode(200)
    .expectContentType(ContentType.JSON)
    .expectHeader("Content-Type", containsString("application/json"))
    .build();

// Usa nos testes
given()
    .spec(reqSpec)
.when()
    .get("/posts/1")
.then()
    .spec(respostaOk)  // valida status 200, content-type JSON
    .body("id", equalTo(1));
```

## Combinando as Duas

```java
public class ConfiguracaoBase {

    protected static RequestSpecification reqSpec;
    protected static ResponseSpecification respostaOk;

    @BeforeAll
    static void configurar() {
        reqSpec = new RequestSpecBuilder()
            .setBaseUri("https://jsonplaceholder.typicode.com")
            .setContentType(ContentType.JSON)
            .build();

        respostaOk = new ResponseSpecBuilder()
            .expectStatusCode(200)
            .expectContentType(ContentType.JSON)
            .build();
    }
}

public class PostTest extends ConfiguracaoBase {

    @Test
    void deveRetornarPost() {
        given()
            .spec(reqSpec)
        .when()
            .get("/posts/1")
        .then()
            .spec(respostaOk)
            .body("title", notNullValue());
    }
}
```

## Especificação com Autenticação

```java
RequestSpecification specAdmin = new RequestSpecBuilder()
    .setBaseUri("https://minha-api.com")
    .setContentType(ContentType.JSON)
    .addHeader("Authorization", "Bearer " + tokenAdmin)
    .build();

RequestSpecification specUsuario = new RequestSpecBuilder()
    .setBaseUri("https://minha-api.com")
    .setContentType(ContentType.JSON)
    .addHeader("Authorization", "Bearer " + tokenUsuario)
    .build();

// Testa acesso de admin
given().spec(specAdmin).when().get("/admin/relatorios").then().statusCode(200);

// Testa acesso negado para usuário comum
given().spec(specUsuario).when().get("/admin/relatorios").then().statusCode(403);
```

---

> Ir para: [Logging](4-Logging.md)
