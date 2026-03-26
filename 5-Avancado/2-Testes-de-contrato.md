# Testes de Contrato

Testes de contrato garantem que as interfaces entre serviços (APIs) não quebrem quando são alteradas — essencial em arquiteturas de microsserviços.

## O Problema

Imagine dois times:
- **Time A** consome a API do **Time B**
- O Time B muda o campo `userName` para `username` (minúsculo)
- Os testes unitários do Time B passam
- A aplicação do Time A quebra em produção

Testes de contrato resolvem isso.

## Consumer-Driven Contracts com Pact

O [Pact](https://docs.pact.io/) é a ferramenta mais popular para contract testing.

### Dependências

```xml
<dependency>
    <groupId>au.com.dius.pact.consumer</groupId>
    <artifactId>junit5</artifactId>
    <version>4.6.7</version>
    <scope>test</scope>
</dependency>
```

### Escrevendo o Contrato (Consumer)

```java
// src/test/java/com/meuapp/contract/UsuarioContractTest.java
package com.meuapp.contract;

import au.com.dius.pact.consumer.dsl.PactDslWithProvider;
import au.com.dius.pact.consumer.junit5.PactConsumerTestExt;
import au.com.dius.pact.consumer.junit5.PactTestFor;
import au.com.dius.pact.core.model.RequestResponsePact;
import au.com.dius.pact.core.model.annotations.Pact;
import org.junit.jupiter.api.Test;
import org.junit.jupiter.api.extension.ExtendWith;

import static io.restassured.RestAssured.given;
import static org.hamcrest.Matchers.*;

@ExtendWith(PactConsumerTestExt.class)
@PactTestFor(providerName = "UserService")
public class UsuarioContractTest {

    @Pact(consumer = "FrontendApp")
    public RequestResponsePact buscarUsuarioPorId(PactDslWithProvider builder) {
        return builder
            .given("usuário com ID 1 existe")
            .uponReceiving("buscar usuário por ID 1")
                .path("/usuarios/1")
                .method("GET")
            .willRespondWith()
                .status(200)
                .body(new PactDslJsonBody()
                    .integerType("id", 1)
                    .stringType("nome", "João Silva")
                    .stringType("email", "joao@email.com"))
            .toPact();
    }

    @Test
    @PactTestFor(pactMethod = "buscarUsuarioPorId")
    void deveRetornarUsuarioConforme(MockServer mockServer) {
        given()
            .baseUri(mockServer.getUrl())
        .when()
            .get("/usuarios/1")
        .then()
            .statusCode(200)
            .body("id", equalTo(1))
            .body("nome", notNullValue())
            .body("email", notNullValue());
    }
}
```

## Verificação Simples sem Pact

Para um approach mais simples, você pode verificar o contrato manualmente:

```java
@Test
@DisplayName("Deve respeitar o contrato da API de usuários")
void deveRespeitarContratoUsuario() {
    given()
        .baseUri("https://api.meuapp.com")
    .when()
        .get("/usuarios/1")
    .then()
        .statusCode(200)
        // Campos obrigatórios do contrato
        .body("id", notNullValue())
        .body("nome", instanceOf(String.class))
        .body("email", matchesPattern(".+@.+\\..+"))
        .body("ativo", instanceOf(Boolean.class))
        // Campos que NÃO devem existir (dados sensíveis)
        .body("senha", nullValue())
        .body("token", nullValue());
}
```

## Boas Práticas

- Defina os campos obrigatórios e seus tipos, não apenas valores exatos
- Verifique que dados sensíveis não são expostos na resposta
- Execute testes de contrato em PRs antes de deploy
- Versione os contratos junto com o código

---

> Ir para: [CI/CD](3-CI-CD.md)
