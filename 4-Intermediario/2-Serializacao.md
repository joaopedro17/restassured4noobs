# Serialização e Deserialização

REST-assured integra com Jackson para converter objetos Java em JSON (serialização) e JSON em objetos Java (deserialização).

## Criando POJOs

```java
// src/main/java/com/meuapp/model/Usuario.java
package com.meuapp.model;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)  // ignora campos extras do JSON
public class Usuario {

    private Integer id;
    private String nome;
    private String email;
    private String username;

    // Construtores
    public Usuario() {}

    public Usuario(String nome, String email) {
        this.nome = nome;
        this.email = email;
    }

    // Getters e Setters
    public Integer getId() { return id; }
    public void setId(Integer id) { this.id = id; }

    public String getNome() { return nome; }
    public void setNome(String nome) { this.nome = nome; }

    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }

    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
}
```

## Serialização: Java → JSON (Request Body)

```java
Usuario novoUsuario = new Usuario("João Silva", "joao@email.com");

given()
    .baseUri("https://jsonplaceholder.typicode.com")
    .contentType(ContentType.JSON)
    .body(novoUsuario)  // REST-assured serializa automaticamente para JSON
.when()
    .post("/users")
.then()
    .statusCode(201)
    .body("name", equalTo("João Silva"));
```

## Deserialização: JSON → Java (Response Body)

```java
// .as(Classe.class) para um único objeto
Usuario usuario =
    given()
        .baseUri("https://jsonplaceholder.typicode.com")
    .when()
        .get("/users/1")
    .then()
        .statusCode(200)
        .extract().as(Usuario.class);

System.out.println(usuario.getNome());   // "Leanne Graham"
System.out.println(usuario.getEmail());  // "Sincere@april.biz"
```

## Deserializando uma Lista

```java
import java.util.List;

List<Usuario> usuarios =
    given()
        .baseUri("https://jsonplaceholder.typicode.com")
    .when()
        .get("/users")
    .then()
        .statusCode(200)
        .extract().jsonPath().getList(".", Usuario.class);

System.out.println(usuarios.size());        // 10
System.out.println(usuarios.get(0).getNome()); // "Leanne Graham"
```

## Objeto Aninhado

```java
// src/main/java/com/meuapp/model/Endereco.java
public class Endereco {
    private String rua;
    private String cidade;
    private String cep;
    // getters e setters...
}

// src/main/java/com/meuapp/model/UsuarioCompleto.java
public class UsuarioCompleto {
    private Integer id;
    private String nome;
    private Endereco endereco;  // objeto aninhado
    // getters e setters...
}

// Deserialização
UsuarioCompleto usuario =
    given()
        .baseUri("https://jsonplaceholder.typicode.com")
    .when()
        .get("/users/1")
    .then()
        .extract().as(UsuarioCompleto.class);

System.out.println(usuario.getEndereco().getCidade()); // "Gwenborough"
```

## Configurando o ObjectMapper

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.PropertyNamingStrategies;

// Para APIs com snake_case (ex: "nome_completo")
ObjectMapper mapper = new ObjectMapper()
    .setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE);

RestAssured.config = RestAssured.config()
    .objectMapperConfig(new ObjectMapperConfig()
        .jackson2ObjectMapperFactory((cls, charset) -> mapper));
```

---

> Ir para: [Especificações](3-Especificacoes.md)
