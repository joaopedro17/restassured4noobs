# API Object Pattern

O **API Object Pattern** é o equivalente do Page Object Model para testes de API. Encapsula os endpoints em classes de serviço, tornando os testes mais legíveis e manuteníveis.

## Por que usar?

- **Reutilização**: a lógica de chamada fica em um lugar só
- **Manutenibilidade**: mudanças na API afetam apenas a classe de serviço
- **Legibilidade**: os testes expressam o que está sendo testado, não como
- **Separação de responsabilidades**: testes focam nas asserções

## Criando uma Classe de Serviço

```java
// src/test/java/com/meuapp/service/PostService.java
package com.meuapp.service;

import com.meuapp.model.Post;
import io.restassured.http.ContentType;
import io.restassured.response.Response;
import io.restassured.specification.RequestSpecification;

import java.util.List;

import static io.restassured.RestAssured.given;

public class PostService {

    private final RequestSpecification spec;

    public PostService(RequestSpecification spec) {
        this.spec = spec;
    }

    public List<Post> listarTodos() {
        return given()
            .spec(spec)
        .when()
            .get("/posts")
        .then()
            .statusCode(200)
            .extract().jsonPath().getList(".", Post.class);
    }

    public Post buscarPorId(int id) {
        return given()
            .spec(spec)
            .pathParam("id", id)
        .when()
            .get("/posts/{id}")
        .then()
            .statusCode(200)
            .extract().as(Post.class);
    }

    public Post criar(Post post) {
        return given()
            .spec(spec)
            .contentType(ContentType.JSON)
            .body(post)
        .when()
            .post("/posts")
        .then()
            .statusCode(201)
            .extract().as(Post.class);
    }

    public Post atualizar(int id, Post post) {
        return given()
            .spec(spec)
            .pathParam("id", id)
            .contentType(ContentType.JSON)
            .body(post)
        .when()
            .put("/posts/{id}")
        .then()
            .statusCode(200)
            .extract().as(Post.class);
    }

    public Response deletar(int id) {
        return given()
            .spec(spec)
            .pathParam("id", id)
        .when()
            .delete("/posts/{id}")
        .then()
            .statusCode(200)
            .extract().response();
    }
}
```

## Usando nos Testes

```java
// src/test/java/com/meuapp/tests/PostTest.java
package com.meuapp.tests;

import com.meuapp.model.Post;
import com.meuapp.service.PostService;
import io.restassured.RestAssured;
import io.restassured.builder.RequestSpecBuilder;
import io.restassured.http.ContentType;
import org.junit.jupiter.api.*;

import java.util.List;

import static org.assertj.core.api.Assertions.assertThat;

@DisplayName("Testes de Posts via API Object")
public class PostTest {

    private static PostService postService;

    @BeforeAll
    static void configurar() {
        var spec = new RequestSpecBuilder()
            .setBaseUri("https://jsonplaceholder.typicode.com")
            .setContentType(ContentType.JSON)
            .build();

        postService = new PostService(spec);
    }

    @Test
    @DisplayName("Deve listar 100 posts")
    void deveListarCemPosts() {
        List<Post> posts = postService.listarTodos();

        assertThat(posts).hasSize(100);
        assertThat(posts.get(0).getId()).isEqualTo(1);
    }

    @Test
    @DisplayName("Deve buscar post por ID")
    void deveBuscarPostPorId() {
        Post post = postService.buscarPorId(1);

        assertThat(post.getId()).isEqualTo(1);
        assertThat(post.getTitulo()).isNotBlank();
    }

    @Test
    @DisplayName("Deve criar novo post")
    void deveCriarPost() {
        Post novoPost = new Post("Meu Título", "Conteúdo do post", 1);
        Post postCriado = postService.criar(novoPost);

        assertThat(postCriado.getId()).isNotNull();
        assertThat(postCriado.getTitulo()).isEqualTo("Meu Título");
    }
}
```

## Boas Práticas

- Use **AssertJ** (`assertThat`) nos testes com API Objects — é mais expressivo que Hamcrest direto
- A classe de serviço deve apenas fazer a chamada e retornar o resultado
- Não coloque lógica de negócio nas classes de serviço
- Injete a `RequestSpecification` via construtor para facilitar testes com diferentes perfis

---

> Ir para: [BDD com Cucumber](../5-Avancado/1-BDD-Cucumber.md)
