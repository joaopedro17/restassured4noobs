# Autenticação

APIs reais exigem autenticação. REST-assured suporta os mecanismos mais comuns nativamente.

## Basic Authentication

```java
given()
    .baseUri("https://minha-api.com")
    .auth().basic("usuario", "senha")
.when()
    .get("/perfil")
.then()
    .statusCode(200);

// Preemptive — envia credenciais sem esperar o challenge 401
given()
    .auth().preemptive().basic("usuario", "senha")
.when()
    .get("/recurso-protegido");
```

## Bearer Token (JWT)

```java
String token = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...";

given()
    .baseUri("https://minha-api.com")
    .header("Authorization", "Bearer " + token)
.when()
    .get("/usuario/perfil")
.then()
    .statusCode(200)
    .body("nome", notNullValue());
```

## Obtendo o Token via Login

Na prática, você extrai o token de uma resposta de login:

```java
// Passo 1: Fazer login e extrair o token
String token =
    given()
        .baseUri("https://minha-api.com")
        .contentType(ContentType.JSON)
        .body("""
            {
                "email": "usuario@email.com",
                "senha": "Senha@123"
            }
            """)
    .when()
        .post("/auth/login")
    .then()
        .statusCode(200)
        .extract().path("token");  // extrai o campo "token" da resposta

// Passo 2: Usar o token nos próximos requests
given()
    .header("Authorization", "Bearer " + token)
.when()
    .get("/pedidos")
.then()
    .statusCode(200);
```

## OAuth2

```java
// OAuth2 com access token
given()
    .auth().oauth2("meu-access-token")
.when()
    .get("/recursos")
.then()
    .statusCode(200);
```

## Reutilizando Autenticação com RequestSpecification

```java
// Cria uma spec reutilizável com autenticação
RequestSpecification specAutenticada = new RequestSpecBuilder()
    .setBaseUri("https://minha-api.com")
    .addHeader("Authorization", "Bearer " + obterToken())
    .setContentType(ContentType.JSON)
    .build();

// Usa em vários testes
given()
    .spec(specAutenticada)
.when()
    .get("/usuarios")
.then()
    .statusCode(200);

given()
    .spec(specAutenticada)
.when()
    .get("/produtos")
.then()
    .statusCode(200);
```

## Armazenando Token em @BeforeAll

```java
@DisplayName("Testes autenticados")
public class TesteAutenticado {

    private static String token;

    @BeforeAll
    static void autenticar() {
        token =
            given()
                .baseUri("https://minha-api.com")
                .contentType(ContentType.JSON)
                .body("{\"email\":\"qa@empresa.com\",\"senha\":\"Qa@123\"}")
            .when()
                .post("/auth/login")
            .then()
                .statusCode(200)
                .extract().path("token");
    }

    @Test
    void deveAcessarDadosDoUsuario() {
        given()
            .baseUri("https://minha-api.com")
            .header("Authorization", "Bearer " + token)
        .when()
            .get("/me")
        .then()
            .statusCode(200)
            .body("email", equalTo("qa@empresa.com"));
    }
}
```

---

> Ir para: [Serialização](2-Serializacao.md)
