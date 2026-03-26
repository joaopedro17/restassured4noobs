# Logging

Logs detalhados de requisições e respostas são essenciais para depurar falhas nos testes.

## Logging de Requisição

```java
// Log completo da requisição (método, URL, headers, body)
given()
    .log().all()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .statusCode(200);

// Log apenas do body da requisição
given()
    .log().body()

// Log apenas dos headers
given()
    .log().headers()

// Log apenas dos parâmetros
given()
    .log().params()
```

## Logging de Resposta

```java
given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .log().all()    // loga toda a resposta
    .statusCode(200);

// Log apenas do body da resposta
.then()
    .log().body()

// Log apenas dos headers da resposta
.then()
    .log().headers()

// Log apenas do status
.then()
    .log().status()
```

## Log Condicional (Recomendado para CI)

Loga apenas quando o teste falha — ideal para não poluir logs de sucesso:

```java
given()
    .log().ifValidationFails()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .log().ifValidationFails()
    .statusCode(200);
```

## Log por Status Code

```java
// Loga a resposta se o status for 400
.then()
    .log().ifStatusCodeIsEqualTo(400)

// Loga se o status for >= 400
.then()
    .log().ifStatusCodeMatches(greaterThanOrEqualTo(400))
```

## Logging Global

Configure logging para todos os testes de uma vez:

```java
@BeforeAll
static void configurarLogs() {
    // Habilita log global para falhas
    RestAssured.filters(
        new RequestLoggingFilter(),
        new ResponseLoggingFilter()
    );
}
```

Ou apenas quando há falha:

```java
@BeforeAll
static void configurarLogs() {
    RestAssured.config = RestAssured.config()
        .logConfig(LogConfig.logConfig()
            .enableLoggingOfRequestAndResponseIfValidationFails());
}
```

## Redirecionando Logs para Arquivo

```java
PrintStream logFile = new PrintStream(new File("logs/test-run.log"));

given()
    .filter(new RequestLoggingFilter(logFile))
    .filter(new ResponseLoggingFilter(logFile))
.when()
    .get("/recurso");
```

---

> Ir para: [API Object Pattern](5-API-object.md)
