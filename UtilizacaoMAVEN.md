
---

# Manual de Maven

## 1. Introdução ao Maven

**Apache Maven** é uma ferramenta de gerenciamento e automação de projetos, principalmente para projetos Java. Maven utiliza um arquivo de configuração XML chamado `pom.xml` (Project Object Model) para descrever o projeto, gerenciar dependências, configurar plugins e definir o ciclo de vida de construção.

## 2. Adicionando Dependências no Maven

### 2.1. Estrutura do `pom.xml`
O arquivo `pom.xml` é a base de um projeto Maven. Aqui está um exemplo básico:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>br.com.witzlerenergia</groupId>
    <artifactId>COW</artifactId>
    <version>1.0-SNAPSHOT</version>
    
    <dependencies>
        <!-- Dependências serão adicionadas aqui -->
    </dependencies>
</project>
```

### 2.2. Adicionando uma Dependência
Para adicionar uma dependência, você precisa saber seu `groupId`, `artifactId` e `version`. Esses detalhes geralmente são encontrados no [Maven Repository](https://mvnrepository.com/).

#### Exemplo de Adição de Dependência
Vamos adicionar a biblioteca `JUnit` ao nosso projeto.

1. **Encontrar Dependência no Maven Repository:**
    - Acesse [Maven Repository](https://mvnrepository.com/).
    - Pesquise por `JUnit`.
    - Selecione a versão desejada e copie a configuração Maven.

2. **Adicionar ao `pom.xml`:**
    ```xml
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    ```

### 2.3. Adicionando Múltiplas Dependências
Você pode adicionar quantas dependências forem necessárias ao `pom.xml`.

```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
        <version>2.5.4</version>
    </dependency>
</dependencies>
```

## 3. Mudando a Versão das Dependências

### 3.1. Atualizando a Versão
Para atualizar a versão de uma dependência, basta alterar o valor do elemento `<version>`.

#### Exemplo:
Vamos atualizar a versão do `JUnit` de `4.13.2` para `5.7.0`.

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.7.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 3.2. Usando Propriedades para Gerenciar Versões
Para facilitar a atualização de versões, você pode usar propriedades no `pom.xml`.

```xml
<properties>
    <junit.version>5.7.0</junit.version>
</properties>

<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## 4. Mudando a Versão do Java no Maven

### 4.1. Definindo a Versão do Java
A versão do Java pode ser definida no `pom.xml` através do plugin `maven-compiler-plugin`.

```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>

<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>${maven.compiler.source}</source>
                <target>${maven.compiler.target}</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### 4.2. Mudando a Versão do Java
Para mudar a versão do Java, altere os valores das propriedades `maven.compiler.source` e `maven.compiler.target`.

#### Exemplo:
Mudando de Java 11 para Java 17:
```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
```

## 5. Encontrando Dependências no Maven Repository

### 5.1. Acessando o Maven Repository
O Maven Repository é um repositório online onde você pode encontrar milhares de bibliotecas e dependências Maven. Acesse [Maven Repository](https://mvnrepository.com/).

### 5.2. Pesquisando Dependências
Digite o nome da biblioteca ou ferramenta que você deseja adicionar ao seu projeto na barra de pesquisa.

#### Exemplo:
Pesquisando por `Gson`:
1. Acesse [Maven Repository](https://mvnrepository.com/).
2. Digite `Gson` na barra de pesquisa e pressione `Enter`.
3. Selecione a biblioteca `Gson` desenvolvida por `Google`.
4. Escolha a versão desejada e copie a configuração Maven.

### 5.3. Adicionando Dependências Encontradas ao `pom.xml`
Após encontrar a dependência, adicione-a ao `pom.xml` conforme exemplificado anteriormente.

#### Exemplo de Adição de `Gson`:
```xml
<dependencies>
    <dependency>
        <groupId>com.google.code.gson</groupId>
        <artifactId>gson</artifactId>
        <version>2.8.8</version>
    </dependency>
</dependencies>
```

## 6. Exclusions em Maven: O que são e por que são importantes?
O que são Exclusions?
No Maven, o gerenciamento de dependências é uma funcionalidade essencial que facilita a inclusão automática de bibliotecas externas no seu projeto. Contudo, essas dependências muitas vezes trazem consigo outras bibliotecas (dependências transitivas) que podem não ser necessárias ou desejáveis. O elemento <exclusions> é usado no arquivo pom.xml para evitar a inclusão de dependências transitivas indesejadas.

Exemplo de Uso
Considere um projeto que depende da biblioteca A, que por sua vez, depende da biblioteca B. Se B não for necessária para o seu projeto, ou se houver um conflito com outra versão de B que você já está usando, você pode excluir B da seguinte forma:

```xml
Copiar código
<dependencies>
    <dependency>
        <groupId>br.com.exemplo</groupId>
        <artifactId>biblioteca-A</artifactId>
        <version>1.0.0</version>
        <exclusions>
            <exclusion>
                <groupId>br.com.exemplo</groupId>
                <artifactId>biblioteca-B</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
</dependencies>
```
Neste exemplo, a exclusão impede que biblioteca-B seja incluída no seu projeto, mesmo que biblioteca-A a utilize.

Aplicações para Exclusions
- Evitar Conflitos de Versão: Excluir dependências transitivas pode ser necessário quando há conflitos entre versões de bibliotecas que seu projeto usa diretamente e aquelas trazidas por dependências transitivas.
- Redução do Tamanho do Projeto: Excluir dependências desnecessárias pode reduzir o tamanho do seu projeto, melhorando o desempenho e reduzindo o uso de recursos.
- Aumentar o Controle: Usando exclusões, você tem maior controle sobre as bibliotecas que são incluídas no seu projeto, garantindo que apenas as dependências necessárias sejam usadas.
- Resolução de Problemas de Segurança: Dependências transitivas podem introduzir vulnerabilidades de segurança. Ao excluir bibliotecas que você não precisa, você minimiza a superfície de ataque e reduz o risco de incorporar dependências vulneráveis.

IMPORTANTE: O Exclusions foi feito com um propósito e não deve se tornar um cacoete para escovação de código. Se o desenvolvedor colocou ele no projeto disponibilizado, espera-se que o mesmo o fez por um bom motivo. Use esta funcionalidade apenas quando for realmente necessário.

## 7. Conclusão

### 7.1. Resumo
Neste manual, abordamos:
- Como adicionar dependências no Maven.
- Como atualizar a versão das dependências.
- Como mudar a versão do Java no Maven.
- Como encontrar dependências no Maven Repository e adicioná-las ao `pom.xml`.
- Como e quando utilizar Exclusions.

### 7.2. Recursos Adicionais
- [Documentação oficial do Maven](https://maven.apache.org/guides/index.html)
- [Maven Repository](https://mvnrepository.com/)
- [Tutoriais e guias de Maven](https://www.baeldung.com/maven)
