# Assertions

O REST-assured usa **Hamcrest matchers** para validar respostas. São expressivos, poderosos e compostos.

## Status Code

```java
.then()
    .statusCode(200)        // exatamente 200
    .statusCode(201)        // criado
    .statusCode(204)        // sem conteúdo
    .statusCode(400)        // bad request
    .statusCode(404)        // não encontrado
    .statusCode(401)        // não autorizado
    .statusCode(500)        // erro interno

// Com matchers
import static org.hamcrest.Matchers.*;

.then()
    .statusCode(anyOf(is(200), is(201)))
    .statusCode(lessThan(300))
```

## Matchers de Corpo (Body)

### Igualdade e Conteúdo

```java
.body("nome", equalTo("João"))
.body("ativo", is(true))
.body("id", equalTo(42))
.body("descricao", containsString("produto"))
.body("email", endsWith("@email.com"))
.body("codigo", startsWith("BR-"))
```

### Nulidade e Presença

```java
.body("token", notNullValue())
.body("erros", nullValue())
.body("campo", not(emptyString()))
```

### Listas e Arrays

```java
// Tamanho da lista
.body("items", hasSize(3))
.body("items.size()", equalTo(3))

// Verificar item na lista
.body("tags", hasItem("java"))
.body("tags", hasItems("java", "api"))

// Verificar campo de todos os itens na lista
.body("posts.userId", everyItem(equalTo(1)))

// Verificar primeiro/último item
.body("[0].id", equalTo(1))
.body("[-1].id", notNullValue())
```

### Números

```java
.body("preco", greaterThan(0.0f))
.body("quantidade", lessThanOrEqualTo(100))
.body("total", closeTo(99.99, 0.01))
```

## JsonPath para Navegação

O JsonPath permite navegar em objetos JSON aninhados:

```java
// Objeto aninhado
.body("endereco.cidade", equalTo("São Paulo"))
.body("endereco.cep", matchesPattern("\\d{5}-\\d{3}"))

// Array de objetos
.body("pedidos[0].status", equalTo("PENDENTE"))
.body("pedidos.status", hasItem("APROVADO"))
```

## Múltiplas Assertions com `.and()`

```java
.then()
    .statusCode(200)
    .body("id", notNullValue())
    .and()
    .body("nome", equalTo("João"))
    .and()
    .body("email", containsString("@"));
```

## Referência Rápida de Matchers

| Matcher | Descrição |
|---------|-----------|
| `equalTo(valor)` | Igual ao valor |
| `is(valor)` | Alias para equalTo |
| `not(valor)` | Negação |
| `notNullValue()` | Não nulo |
| `nullValue()` | Nulo |
| `containsString(str)` | Contém a string |
| `startsWith(str)` | Começa com |
| `endsWith(str)` | Termina com |
| `hasSize(n)` | Coleção com n elementos |
| `hasItem(item)` | Coleção contém o item |
| `hasItems(a, b)` | Contém todos os itens |
| `greaterThan(n)` | Maior que n |
| `lessThan(n)` | Menor que n |
| `anyOf(a, b)` | Qualquer um dos matchers |
| `allOf(a, b)` | Todos os matchers |
| `everyItem(m)` | Todos os itens satisfazem m |

---

> Ir para: [Primeiro Teste](4-Primeiro-teste.md)
