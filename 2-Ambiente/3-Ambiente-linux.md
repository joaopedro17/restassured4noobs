# Configuração no Linux

## Instalando o Java

### Ubuntu / Debian

```bash
sudo apt-get update
sudo apt-get install -y openjdk-21-jdk
```

Configure o JAVA_HOME:

```bash
echo 'export JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64' >> ~/.bashrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

Verifique:

```bash
java -version
```

### Fedora / RHEL / CentOS

```bash
sudo dnf install java-21-openjdk-devel
```

### Usando SDKMAN (qualquer distro)

```bash
curl -s "https://get.sdkman.io" | bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk install java 21-tem
```

## Instalando o Maven

```bash
sudo apt-get install -y maven
```

Ou via SDKMAN:

```bash
sdk install maven
```

Verifique:

```bash
mvn -version
```

---

> Ir para: [Editor e Início](4-Editor-e-inicio.md)
