# Configuração no Windows

## Instalando o Java

1. Acesse [adoptium.net](https://adoptium.net/) e baixe o **Eclipse Temurin 21** (LTS) para Windows
2. Execute o instalador `.msi`
3. Marque a opção **Set JAVA_HOME variable** durante a instalação

Verifique no Prompt de Comando ou PowerShell:

```powershell
java -version
```

### Configurando JAVA_HOME manualmente (se necessário)

1. Abra **Painel de Controle > Sistema > Configurações Avançadas do Sistema**
2. Clique em **Variáveis de Ambiente**
3. Adicione nova variável de sistema: `JAVA_HOME` = `C:\Program Files\Eclipse Adoptium\jdk-21.x.x`
4. Edite a variável `Path` e adicione `%JAVA_HOME%\bin`

## Instalando o Maven

1. Baixe o [Apache Maven](https://maven.apache.org/download.cgi) (Binary zip archive)
2. Extraia em `C:\Program Files\Apache\maven`
3. Adicione `C:\Program Files\Apache\maven\bin` à variável `Path`

Verifique:

```powershell
mvn -version
```

## IDE Recomendada: IntelliJ IDEA

Baixe o [IntelliJ IDEA Community](https://www.jetbrains.com/idea/download/) — é gratuito e excelente para Java.

---

> Ir para: [Configuração no Linux](3-Ambiente-linux.md)
