# Relatórios

Relatórios bem configurados facilitam a identificação de falhas e comunicação com stakeholders.

## Allure Reports

O [Allure](https://allurereport.org/) gera relatórios HTML ricos e interativos.

### Dependências

```xml
<!-- pom.xml -->
<dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-rest-assured</artifactId>
    <version>2.25.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-junit5</artifactId>
    <version>2.25.0</version>
    <scope>test</scope>
</dependency>
```

```xml
<!-- Plugin para gerar relatório -->
<plugin>
    <groupId>io.qameta.allure</groupId>
    <artifactId>allure-maven</artifactId>
    <version>2.12.0</version>
</plugin>
```

### Configurando o Listener REST-assured

```java
import io.qameta.allure.restassured.AllureRestAssured;

// Adicione o filtro globalmente
RestAssured.filters(new AllureRestAssured());

// Ou por requisição
given()
    .filter(new AllureRestAssured())
    .baseUri("https://jsonplaceholder.typicode.com")
.when()
    .get("/posts/1")
.then()
    .statusCode(200);
```

### Anotações do Allure

```java
import io.qameta.allure.*;

@Epic("API de Posts")
@Feature("Gerenciamento de Posts")
public class PostTest {

    @Test
    @Story("Buscar post existente")
    @Severity(SeverityLevel.CRITICAL)
    @Description("Verifica que um post existente é retornado corretamente")
    void deveRetornarPostPorId() {
        // ...
    }
}
```

### Gerando o Relatório

```bash
# Executar testes
mvn test

# Abrir relatório no navegador
mvn allure:serve

# Gerar relatório estático
mvn allure:report
# Relatório em: target/site/allure-maven-plugin/index.html
```

## Relatório de Cobertura com JaCoCo

```xml
<!-- pom.xml -->
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.11</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

```bash
mvn test
# Relatório em: target/site/jacoco/index.html
```

## Boas Práticas de Relatórios

- Use `@DisplayName` descritivos para todos os testes
- Organize por `@Epic`, `@Feature` e `@Story` no Allure
- Configure a severidade (`@Severity`) para priorizar falhas críticas
- Integre o relatório ao CI com upload de artifacts

---

> Parabéns! Você concluiu o RESTassured4Noobs! Volte ao [README](../README.md) para revisar o conteúdo.
