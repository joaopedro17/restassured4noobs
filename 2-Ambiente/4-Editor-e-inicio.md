# Editor e Início do Projeto

## IDE Recomendada: IntelliJ IDEA

O [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download/) é gratuito e oferece excelente suporte para Java e Maven.

## Criando um Projeto Maven

### Via IntelliJ IDEA

1. Abra o IntelliJ IDEA
2. Clique em **New Project**
3. Selecione **Maven Archetype**
4. Preencha:
   - **GroupId**: `com.meuapp`
   - **ArtifactId**: `api-testes`
   - **Version**: `1.0-SNAPSHOT`
5. Clique em **Create**

### Via linha de comando

```bash
mvn archetype:generate \
  -DgroupId=com.meuapp \
  -DartifactId=api-testes \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DarchetypeVersion=1.4 \
  -DinteractiveMode=false
```

## Configurando o pom.xml

Substitua o conteúdo do `pom.xml` com as dependências necessárias:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.meuapp</groupId>
    <artifactId>api-testes</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!-- REST-assured -->
        <dependency>
            <groupId>io.rest-assured</groupId>
            <artifactId>rest-assured</artifactId>
            <version>5.4.0</version>
            <scope>test</scope>
        </dependency>

        <!-- JUnit 5 -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.2</version>
            <scope>test</scope>
        </dependency>

        <!-- Jackson para serialização/deserialização -->
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.17.0</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <!-- Plugin para rodar JUnit 5 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.5</version>
            </plugin>
        </plugins>
    </build>
</project>
```

## Estrutura do Projeto

```
api-testes/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/meuapp/
│   │           └── model/          ← POJOs (modelos de dados)
│   └── test/
│       └── java/
│           └── com/meuapp/
│               ├── config/         ← Configuração base
│               ├── service/        ← Classes de serviço (API Objects)
│               └── tests/          ← Classes de teste
├── pom.xml
└── README.md
```

## Executando os Testes

```bash
# Rodar todos os testes
mvn test

# Rodar uma classe específica
mvn test -Dtest=UsuarioTest

# Rodar um método específico
mvn test -Dtest=UsuarioTest#deveRetornarUsuarioPorId
```

---

> Ir para: [Dicas Gerais](5-Dicas-gerais.md)
