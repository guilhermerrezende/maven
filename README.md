# Atividade Prática MAVEN 🚀

Nesta atividade prática, o objetivo é compreender o funcionamento do Maven como ferramenta de automação de builds em projetos Java. A proposta é criar um projeto Maven do zero, implementar testes automatizados com JUnit, gerar relatórios e realizar o build automatizado da aplicação.

Ao final da atividade, você será capaz de:

Criar e estruturar um projeto Maven.

Automatizar o processo de build (compilação, empacotamento e testes).

Escrever e executar testes com JUnit.

Gerar relatórios HTML com os resultados dos testes.

Interpretar e analisar os relatórios gerados.

---

## 📦 O que é o Maven?
O Maven é uma ferramenta de automação de compilação e gerenciamento de dependências para projetos Java. Ele permite:
- Gerenciamento de dependências (JARs, bibliotecas etc.)
- Automatização de build e testes
- Geração de relatórios
- Integração com ferramentas de CI/CD como GitHub Actions

---

## 🛠️ Pré-requisitos

### 1. Instalar o Java (JDK 17 ou superior)
- Windows: https://www.oracle.com/java/technologies/javase/jdk17-archive-downloads.html
- Instalador do Windows x64	- https://download.oracle.com/java/17/archive/jdk-17.0.12_windows-x64_bin.exe

- Linux (Debian/Ubuntu):
```bash
sudo apt update
sudo apt install openjdk-17-jdk
```

Verifique:
```bash
java -version
```

### 2. Instalar o Maven
- Baixe: [apache-maven-3.9.9-bin.zip](https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.zip)
- Extraia o ZIP em uma pasta

---

## ⚙️ Configuração das Variáveis de Ambiente do Maven

Após extrair ou instalar o Maven, é essencial configurar as variáveis de ambiente para que você possa utilizar o comando `mvn` a partir de qualquer terminal.

### 🪟 **Windows**

Suponha que o Maven foi extraído em:
```
C:\CaminhoDOmaven\apache-maven-3.9.9
```

#### 🧩 1. Adicione a variável de sistema `MAVEN_HOME`

1. Pressione `Win + S` e digite: **variáveis de ambiente**
2. Clique em **“Editar variáveis de ambiente do sistema”**
3. Na aba **Avançado**, clique em **“Variáveis de ambiente…”**
4. Na seção **Variáveis do sistema**, clique em **“Novo…”**
   - **Nome da variável:** `MAVEN_HOME`
   - **Valor da variável:** `C:\CaminhoDOmaven\apache-maven-3.9.9` ATENÇÃO: Não colocar aspas "", que vem copiado com o caminho.
   - Clicar em  **OK** no final para salvar. 

#### 📌 2. Adicione o Maven ao Path

1. Ainda na seção **Variáveis do sistema**, encontre a variável `Path` e clique em **“Editar…”**
2. Clique em **“Novo”** e adicione o caminho:
   ```
   C:\CaminhoDOmaven\apache-maven-3.9.9\bin
   ```
 - Clicar em  **OK** no final para salvar.

#### ✅ 3. Verifique se está funcionando

Abra um novo terminal (CMD ou PowerShell) e digite:

```powershell
mvn -version
```

Você deve ver algo como:

```
Apache Maven 3.9.9
Maven home: C:\CaminhoDOmaven\apache-maven-3.9.9
Java version: 17.x.x, vendor: Oracle Corporation
```

### 🐧 **Linux (Ubuntu/Debian)**

Suponha que você extraiu o Maven em:
```
/home/seuusuario/apache-maven-3.9.9
```

#### 🧩 1. Adicione as variáveis no seu terminal

Abra o terminal e edite o arquivo de perfil:

```bash
nano ~/.bashrc
```
Ou se estiver usando Zsh:

```bash
nano ~/.zshrc
```

Adicione ao final do arquivo:

```bash
export MAVEN_HOME=/home/seuusuario/apache-maven-3.9.9
export PATH=$MAVEN_HOME/bin:$PATH
```

Salve com `Ctrl + O`, depois `Enter` e feche com `Ctrl + X`.

#### 🔄 2. Aplique as mudanças

```bash
source ~/.bashrc
```

#### ✅ 3. Verifique a instalação

```bash
mvn -version
```

Você verá algo assim:

```
Apache Maven 3.9.9
Maven home: /home/seuusuario/apache-maven-3.9.9
Java version: 17.x.x, vendor: OpenJDK
```

---

## 📁 Criando o Projeto

Execute o seguinte comando para criar seu projeto Maven:
```bash
mvn archetype:generate `
  "-DgroupId=com.exemplo" `
  "-DartifactId=atividade-maven" `
  "-DarchetypeArtifactId=maven-archetype-quickstart" `
  "-DinteractiveMode=false"

```

### Explicação dos parâmetros:
- `groupId`: pacote base (ex: com.exemplo)
- `artifactId`: nome do projeto
- `archetypeArtifactId`: modelo de projeto
- `interactiveMode=false`: evita prompts interativos

Depois, entre na pasta criada:
```bash
cd atividade-maven
```

---

## 📄 Estrutura do Projeto (Padrão MAVEN) 
```
atividade-maven/
 ├── pom.xml
 └── src/
     ├── main/java/com/exemplo/App.java
     └── test/java/com/exemplo/AppTest.java
```

---

## 📂 Código Exemplo: Calculadora

### `src/main/java/com/exemplo/Calculadora.java`
```java
package com.exemplo;

public class Calculadora {
    public int somar(int a, int b) {
        return a + b;
    }

    public int multiplicar(int a, int b) {
        return a * b;
    }
}

```
### `src/main/java/com/exemplo/CalculadoraService.java`

```java
package com.exemplo;

public class CalculadoraService {
    private final Calculadora calculadora;

    public CalculadoraService(Calculadora calculadora) {
        this.calculadora = calculadora;
    }

    public int calcularDobroDaSoma(int a, int b) {
        int soma = calculadora.somar(a, b);
        return calculadora.multiplicar(soma, 2);
    }
}

```

### Teste Unitário – `CalculadoraTest.java`
```java
package com.exemplo;

import org.junit.Test;
import static org.junit.Assert.*;

public class CalculadoraTest {
    @Test
    public void testSomar() {
        Calculadora calc = new Calculadora();
        assertEquals(5, calc.somar(2, 3));
    }

    @Test
    public void testMultiplicar() {
        Calculadora calc = new Calculadora();
        assertEquals(20, calc.multiplicar(4, 5));
    }
}

```

### Teste de Integração – `CalculadoraIntegracaoTest.java`
```java
package com.exemplo;

import org.junit.Test;
import static org.junit.Assert.*;

public class CalculadoraIntegracaoTest {
    @Test
    public void testCalcularDobroDaSoma() {
        Calculadora calculadora = new Calculadora();
        CalculadoraService service = new CalculadoraService(calculadora);

        int resultado = service.calcularDobroDaSoma(2, 3); // (2 + 3) * 2 = 10
        assertEquals(10, resultado);
    }
}

```

Crie este arquivo em:
```
src/test/java/com/exemplo/CalculadoraIntegracaoTest.java
```
## Estrutura do Projeto
```css

atividade-maven/
├── pom.xml
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/
│   │           └── exemplo/
│   │               ├── Calculadora.java
│   │               └── CalculadoraService.java
│   └── test/
│       └── java/
│           └── com/
│               └── exemplo/
│                   ├── CalculadoraTest.java
│                   └── CalculadoraIntegracaoTest.java

```

## 🔧 Arquivo `pom.xml` completo
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.exemplo</groupId>
    <artifactId>atividade-maven</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.5</version>
                <configuration>
                    <reportsDirectory>${project.build.directory}/surefire-reports</reportsDirectory>
                </configuration>
            </plugin>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-site-plugin</artifactId>
                <version>3.12.1</version>
            </plugin>
        </plugins>
    </build>

    <reporting>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-report-plugin</artifactId>
                <version>3.1.2</version>
            </plugin>
        </plugins>
    </reporting>
</project>

```

---

## 🔄 Compilando e Rodando os Testes

```bash
mvn clean install
```
- `clean`: limpa diretórios antigos
- `install`: compila, testa e instala localmente

Para executar apenas os testes:
```bash
mvn test
```

---

## 📊 Gerando Relatórios de Testes

### 1. Relatórios de Console
São exibidos diretamente no terminal após `mvn test`

### 2. Relatórios HTML com Surefire
```bash
mvn site
```
Abra o relatório:
```
target/site/surefire-report.html
```
---

Pronto! Agora você tem um ambiente Maven completo para desenvolvimento, build automatizada e testes profissionais em Java. ✅

