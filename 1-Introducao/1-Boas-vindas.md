# Bem-vindo ao RESTassured4Noobs!

## O que é o REST-assured?

O **REST-assured** é uma biblioteca Java criada por [Johan Haleby](https://github.com/johanhaleby) que simplifica drasticamente o teste de APIs REST e HTTP. Ele oferece uma DSL (Domain-Specific Language) em estilo BDD com a sintaxe **Given / When / Then**, tornando os testes de API expressivos, legíveis e fáceis de manter.

Com REST-assured você faz requisições HTTP, valida respostas e descreve o comportamento esperado da sua API em poucas linhas de Java.

## Por que usar o REST-assured?

Testar APIs em Java sem REST-assured significa lidar com `HttpURLConnection`, `HttpClient`, ou outras bibliotecas verbosas que exigem muito código boilerplate. O REST-assured elimina essa dor.

### Comparativo: REST-assured vs Alternativas

| Critério | REST-assured | Postman/Newman | Apache HttpClient | Spring MockMvc |
|----------|-------------|---------------|-------------------|---------------|
| Linguagem | Java | JavaScript | Java | Java |
| Estilo BDD | ✅ | Parcial | ❌ | ❌ |
| Integração com JUnit/TestNG | ✅ | Via CLI | ✅ | ✅ |
| Matchers com Hamcrest | ✅ | ❌ | ❌ | ✅ |
| JsonPath / XmlPath | ✅ nativo | Parcial | Manual | Parcial |
| Boilerplate | Mínimo | Mínimo | Alto | Médio |
| Ideal para | Testes de API | Exploração manual | HTTP genérico | Testes Spring |

## Vantagens do REST-assured

- **Sintaxe BDD**: testes legíveis que qualquer pessoa entende
- **Hamcrest integrado**: matchers poderosos para validar respostas
- **JsonPath e XmlPath**: navegar na resposta sem deserialização manual
- **Autenticação pronta**: Basic, Bearer, OAuth2 com poucas linhas
- **Integração com Java**: funciona com JUnit 5, TestNG, Maven e Gradle
- **Logging detalhado**: logs de request e response para debug
- **Serialização automática**: integração com Jackson e Gson

## O que você vai aprender neste curso?

Ao longo deste guia você vai aprender a:

1. Configurar um projeto Maven com REST-assured
2. Realizar requisições GET, POST, PUT, PATCH e DELETE
3. Validar respostas com Hamcrest e JsonPath
4. Trabalhar com autenticação (Basic, Bearer, OAuth2)
5. Serializar e deserializar POJOs com Jackson
6. Criar especificações reutilizáveis
7. Integrar com CI/CD e gerar relatórios

## Exemplo rápido

```java
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

// Teste de uma API de usuários
given()
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/users/1")
.then()
    .statusCode(200)
    .body("name", equalTo("Leanne Graham"))
    .body("email", containsString("@"))
    .body("address.city", notNullValue());
```

Simples, legível e poderoso!

---

> Ir para: [Comunicação](2-Comunicacao.md)
