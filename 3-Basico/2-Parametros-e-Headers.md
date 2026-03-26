# Parâmetros e Headers

## Query Parameters

Parâmetros de URL que aparecem após `?` (ex: `/posts?userId=1`):

```java
// .queryParam() para cada parâmetro
given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .queryParam("userId", 1)
.when()
    .get("/posts")
.then()
    .statusCode(200)
    .body("size()", greaterThan(0))
    .body("[0].userId", equalTo(1));

// Múltiplos query params
given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .queryParam("userId", 1)
    .queryParam("_limit", 3)
.when()
    .get("/posts")
.then()
    .statusCode(200)
    .body("size()", equalTo(3));
```

## Path Parameters

Parâmetros que fazem parte do caminho da URL (ex: `/posts/{id}`):

```java
int postId = 5;

given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .pathParam("id", postId)
.when()
    .get("/posts/{id}")
.then()
    .statusCode(200)
    .body("id", equalTo(postId));
```

## Headers

```java
// Header único
given()
    .baseUri("https://minha-api.com")
    .header("Authorization", "Bearer meu-token")
    .header("Accept-Language", "pt-BR")
.when()
    .get("/perfil")
.then()
    .statusCode(200);

// Múltiplos headers de uma vez
import io.restassured.http.Headers;
import io.restassured.http.Header;

Headers headers = new Headers(
    new Header("Authorization", "Bearer token"),
    new Header("X-Request-ID", "abc-123"),
    new Header("Accept-Language", "pt-BR")
);

given()
    .headers(headers)
.when()
    .get("/recurso");
```

## Content-Type

```java
import io.restassured.http.ContentType;

given()
    .contentType(ContentType.JSON)     // application/json
    .contentType(ContentType.XML)      // application/xml
    .contentType(ContentType.TEXT)     // text/plain
    .contentType(ContentType.URLENC)   // application/x-www-form-urlencoded
```

## Form Parameters

Para requisições com `application/x-www-form-urlencoded`:

```java
given()
    .baseUri("https://minha-api.com")
    .contentType(ContentType.URLENC)
    .formParam("username", "joao")
    .formParam("password", "senha123")
.when()
    .post("/login")
.then()
    .statusCode(200);
```

## Verificando Headers da Resposta

```java
given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .statusCode(200)
    .header("Content-Type", containsString("application/json"));
```

---

> Ir para: [Assertions](3-Assertions.md)
