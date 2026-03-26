# Como Contribuir

Obrigado por querer contribuir com o RESTassured4Noobs! Este documento explica como participar do projeto.

## Tipos de Contribuição

- Correção de erros de escrita ou código
- Melhoria em explicações existentes
- Adição de novos exemplos práticos
- Criação de novos módulos ou aulas
- Revisão de conteúdo técnico

## Fluxo de Trabalho

1. Faça um **fork** do repositório
2. Crie uma **branch** com o prefixo adequado:
   - `feat/nome-da-funcionalidade` — para novo conteúdo
   - `fix/descricao-do-erro` — para correções
   - `docs/descricao-da-melhoria` — para melhorias na documentação
   - `update/nome-do-conteudo` — para atualizações de conteúdo existente
3. Faça suas alterações
4. Crie um **commit** com mensagem descritiva (veja padrão abaixo)
5. Abra um **Pull Request** para a branch `main`

## Padrão de Commit

```
tipo: descrição curta em português

Exemplos:
feat: adiciona aula sobre autenticação OAuth2
fix: corrige exemplo de desserialização com Jackson
docs: melhora explicação sobre Hamcrest matchers
update: atualiza dependência para REST-assured 5.x
```

## Padrões de Conteúdo

- Todo conteúdo deve ser escrito em **Português (Brasil)**
- Exemplos de código devem usar **Java** com REST-assured 5.x
- Use **JUnit 5** (Jupiter) nos exemplos de teste
- Mantenha a numeração sequencial dos arquivos (ex: `1-`, `2-`, `3-`)
- Cada arquivo deve ter um rodapé de navegação apontando para o próximo conteúdo
- Inclua sempre um bloco de **boas práticas** quando relevante

## Padrões de Código Java

```java
// Use estática import para melhor legibilidade
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

// Prefira given/when/then para clareza
given()
    .contentType(ContentType.JSON)
    .body(payload)
.when()
    .post("/usuarios")
.then()
    .statusCode(201)
    .body("id", notNullValue());
```

## Dúvidas

Abra uma [issue](../../issues) no repositório antes de começar grandes alterações para alinharmos a abordagem.
