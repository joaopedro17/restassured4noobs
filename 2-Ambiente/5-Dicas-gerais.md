# Dicas Gerais

## Configuração Global com RestAssured

Crie uma classe base de configuração para evitar repetição:

```java
// src/test/java/com/meuapp/config/ConfiguracaoBase.java
package com.meuapp.config;

import io.restassured.RestAssured;
import io.restassured.builder.RequestSpecBuilder;
import io.restassured.http.ContentType;
import org.junit.jupiter.api.BeforeAll;

public class ConfiguracaoBase {

    @BeforeAll
    static void configurar() {
        RestAssured.baseURI = "https://jsonplaceholder.typicode.com";
        RestAssured.requestSpecification = new RequestSpecBuilder()
            .setContentType(ContentType.JSON)
            .setAccept(ContentType.JSON)
            .build();
    }
}
```

Suas classes de teste herdam dessa classe:

```java
public class UsuarioTest extends ConfiguracaoBase {
    // testes aqui
}
```

## Logging para Debug

Ative logs globais para depuração durante o desenvolvimento:

```java
RestAssured.filters(new RequestLoggingFilter(), new ResponseLoggingFilter());
```

Ou ative por requisição:

```java
given()
    .log().all()   // loga a requisição
.when()
    .get("/users/1")
.then()
    .log().all()   // loga a resposta
    .statusCode(200);
```

## Desabilitando Validação SSL (apenas em desenvolvimento)

```java
RestAssured.useRelaxedHTTPSValidation();
```

> Nunca use isso em produção! Apenas em ambientes de desenvolvimento/testes internos.

## Timeout de Conexão

```java
RestAssured.config = RestAssured.config()
    .httpClient(HttpClientConfig.httpClientConfig()
        .setParam("http.connection.timeout", 5000)
        .setParam("http.socket.timeout", 5000));
```

## Variáveis de Ambiente para Credenciais

Nunca deixe credenciais hardcoded. Use variáveis de ambiente:

```java
String token = System.getenv("API_TOKEN");
String baseUrl = System.getenv("API_BASE_URL");
```

No `~/.bashrc` ou `.env`:

```bash
export API_TOKEN=meu-token-secreto
export API_BASE_URL=https://api.meuapp.com
```

---

> Ir para: [Verbos HTTP](../3-Basico/1-Verbos-HTTP.md)
