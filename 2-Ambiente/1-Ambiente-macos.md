# Configuração no macOS

## Instalando o Java

### Usando Homebrew (recomendado)

Se você não tem o Homebrew instalado:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Instale o Java 21 (LTS):

```bash
brew install openjdk@21
```

Configure o JAVA_HOME:

```bash
echo 'export JAVA_HOME=$(brew --prefix openjdk@21)' >> ~/.zshrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.zshrc
source ~/.zshrc
```

Verifique a instalação:

```bash
java -version
# java version "21.x.x" ...
```

### Usando SDKMAN (alternativa)

```bash
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java 21-tem
```

## Instalando o Maven

```bash
brew install maven
```

Verifique:

```bash
mvn -version
# Apache Maven 3.x.x ...
```

## Próximos Passos

Com Java e Maven instalados, você está pronto para criar seu projeto REST-assured.

---

> Ir para: [Configuração no Windows](2-Ambiente-windows.md)
