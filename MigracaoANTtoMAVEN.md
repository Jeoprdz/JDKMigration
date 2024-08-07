## 1. Introdução

### 1.1. O que é ANT?
- **Apache ANT** é uma ferramenta de automação de compilação utilizada em projetos Java. ANT usa arquivos XML para definir tarefas e depende de scripts para execução.

### 1.2. O que é MAVEN?
- **Apache Maven** é uma ferramenta de gerenciamento de projetos e automação de compilação. MAVEN usa arquivos POM (Project Object Model) em XML para gerenciar o ciclo de vida do projeto, dependências, relatórios e mais.

### 1.3. Histórico e Importância
- **ANT** foi amplamente utilizado antes do Maven por sua flexibilidade e simplicidade.
- **Maven** se tornou popular devido à sua capacidade de gerenciar dependências automaticamente, facilitar a configuração do projeto e promover boas práticas de desenvolvimento.

## 2. Comparação entre ANT e MAVEN

### 2.1. Configuração do Projeto

#### 2.1.1. Estrutura de Arquivos
- **ANT**: Estrutura de arquivos personalizável.
  ```xml
  <project name="COW" default="compile" basedir=".">
      <property name="src" location="src"/>
      <property name="build" location="build"/>
      
      <target name="init">
          <mkdir dir="${build}"/>
      </target>
      
      <target name="compile" depends="init">
          <javac srcdir="${src}" destdir="${build}"/>
      </target>
  </project>
  ```
- **MAVEN**: Estrutura de diretórios padrão (src/main/java, src/test/java).
  ```xml
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <groupId>br.com.witzlerenergia</groupId>
      <artifactId>COW</artifactId>
      <version>1.0-SNAPSHOT</version>
      
      <dependencies>
          <!-- Dependências do projeto -->
      </dependencies>
      
      <build>
          <plugins>
              <!-- Plugins de construção -->
          </plugins>
      </build>
  </project>
  ```

#### 2.1.2. Dependências
- **ANT**: Gerenciamento manual de dependências.
  ```xml
  <target name="resolve-dependencies">
      <mkdir dir="lib"/>
      <get src="https://repo1.maven.org/maven2/junit/junit/4.12/junit-4.12.jar" dest="lib/junit-4.12.jar"/>
  </target>
  ```
- **MAVEN**: Gerenciamento automático de dependências através do repositório central Maven.
  ```xml
  <dependencies>
      <dependency>
          <groupId>junit</groupId>
          <artifactId>junit</artifactId>
          <version>4.12</version>
          <scope>test</scope>
      </dependency>
  </dependencies>
  ```

### 2.2. Plugins e Extensões

#### 2.2.1. ANT
- **ANT** usa tarefas customizadas definidas em XML.
  ```xml
  <target name="run-tests">
      <junit>
          <classpath>
              <pathelement location="build"/>
              <pathelement location="lib/junit-4.12.jar"/>
          </classpath>
          <test name="br.com.witzlerenergia.COWTest"/>
      </junit>
  </target>
  ```

#### 2.2.2. MAVEN
- **MAVEN** possui uma ampla gama de plugins disponíveis no repositório central.
  ```xml
  <build>
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-surefire-plugin</artifactId>
              <version>2.22.2</version>
              <configuration>
                  <includes>
                      <include>**/*Test.java</include>
                  </includes>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```

### 2.3. Ciclo de Vida do Projeto

#### 2.3.1. ANT
- **ANT** não possui um ciclo de vida de construção predefinido. As etapas da construção são definidas pelo desenvolvedor.
  ```xml
  <target name="build">
      <depends name="compile"/>
      <jar destfile="dist/COW.jar" basedir="build"/>
  </target>
  ```

#### 2.3.2. MAVEN
- **MAVEN** possui um ciclo de vida predefinido (validate, compile, test, package, verify, install, deploy).
  ```bash
  mvn clean install
  ```

## 3. Migração de ANT para MAVEN

### 3.1. Passo a Passo da Migração

#### 3.1.1. Preparação
1. **Analisar o build.xml do ANT**: Identificar tarefas, dependências e processos customizados.
2. **Criar um arquivo pom.xml básico**: Definir o projeto, groupId, artifactId e version.

#### 3.1.2. Conversão das Tarefas
- **Compile Task**
  ```xml
  <!-- ANT -->
  <target name="compile">
      <javac srcdir="${src}" destdir="${build}"/>
  </target>
  ```
  ```xml
  <!-- MAVEN -->
  <build>
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <version>3.8.1</version>
              <configuration>
                  <source>21</source>
                  <target>21</target>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```

- **Run Tests Task**
  ```xml
  <!-- ANT -->
  <target name="run-tests">
      <junit>
          <classpath>
              <pathelement location="build"/>
              <pathelement location="lib/junit-4.12.jar"/>
          </classpath>
          <test name="br.com.witzlerenergia.COWTest"/>
      </junit>
  </target>
  ```
  ```xml
  <!-- MAVEN -->
  <build>
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-surefire-plugin</artifactId>
              <version>2.22.2</version>
              <configuration>
                  <includes>
                      <include>**/*Test.java</include>
                  </includes>
              </configuration>
          </plugin>
      </plugins>
  </build>
  ```

#### 3.1.3. Gerenciamento de Dependências
Adicionar dependências ao pom.xml.
```xml
<dependencies>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### 3.2. Exemplos de Código

#### 3.2.1. Estrutura do Projeto

**ANT:**
```plaintext
COW/
├── build.xml
├── src/
│   └── br/
│       └── com/
│           └── witzlerenergia/
│               └── COW.java
└── lib/
    └── junit-4.12.jar
```

**MAVEN:**
```plaintext
COW/
├── pom.xml
├── src/
│   ├── main/
│   │   └── java/
│   │       └── br/
│   │           └── com/
│   │               └── witzlerenergia/
│   │                   └── COW.java
│   └── test/
│       └── java/
│           └── br/
│               └── com/
│                   └── witzlerenergia/
│                       └── COWTest.java
└── target/
```

### 3.3. Benefícios da Migração

#### 3.3.1. Melhor Gerenciamento de Dependências
Maven simplifica o gerenciamento de dependências através de seu repositório central e transitive dependencies.

#### 3.3.2. Padronização e Ciclo de Vida Integrado
Maven promove uma estrutura de projeto padronizada e fornece um ciclo de vida de construção predefinido que simplifica o processo de build e deploy.

#### 3.3.3. Extensibilidade com Plugins
A vasta gama de plugins disponíveis para Maven facilita a automação de tarefas comuns de construção, teste e deploy.

### 3.4. Configuração Avançada do Maven


#### 3.4.1. Perfil de Build
Maven permite a configuração de perfis de build para diferentes ambientes (desenvolvimento, teste, produção).

```xml
<profiles>
    <profile>
        <id>development</id>
        <properties>
            <environment>development</environment>
        </properties>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                    <configuration>
                        <source>21</source>
                        <target>21</target>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
    <profile>
        <id>production</id>
        <properties>
            <environment>production</environment>
        </properties>
        <build>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.8.1</version>
                    <configuration>
                        <source>21</source>
                        <target>21</target>
                    </configuration>
                </plugin>
            </plugins>
        </build>
    </profile>
</profiles>
```

Para ativar um perfil específico durante o build, use o seguinte comando:
```bash
mvn clean install -Pdevelopment
```

#### 3.4.2. Gerenciamento de Dependências Transitivas
Maven gerencia dependências transitivas automaticamente. Se o seu projeto depende de uma biblioteca que, por sua vez, depende de outras bibliotecas, Maven resolverá essas dependências para você.

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <version>2.7.1</version>
</dependency>
```

Ao adicionar essa dependência, Maven também incluirá todas as dependências necessárias para o Spring Boot Starter Web.

#### 3.4.3. Plugins Populares do Maven
Maven possui uma vasta gama de plugins para diversas tarefas. Aqui estão alguns exemplos populares:

- **maven-compiler-plugin**: Compila o código fonte do projeto.
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.8.1</version>
        <configuration>
            <source>21</source>
            <target>21</target>
        </configuration>
    </plugin>
    ```

- **maven-surefire-plugin**: Executa testes unitários.
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.22.2</version>
        <configuration>
            <includes>
                <include>**/*Test.java</include>
            </includes>
        </configuration>
    </plugin>
    ```

- **maven-assembly-plugin**: Cria arquivos JAR, ZIP, etc.
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <version>3.3.0</version>
        <configuration>
            <descriptors>
                <descriptor>src/assembly/descriptor.xml</descriptor>
            </descriptors>
        </configuration>
    </plugin>
    ```

- **maven-dependency-plugin**: Gerencia dependências, incluindo cópia e análise de dependências.
    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <version>3.1.2</version>
        <executions>
            <execution>
                <id>copy-dependencies</id>
                <phase>process-resources</phase>
                <goals>
                    <goal>copy-dependencies</goal>
                </goals>
                <configuration>
                    <outputDirectory>${project.build.directory}/libs</outputDirectory>
                </configuration>
            </execution>
        </executions>
    </plugin>
    ```

#### 4. Utilizando Bibliotecas Internas com Maven
4.1. Utilizando Bibliotecas Internas com Maven
Em alguns casos, pode ser necessário utilizar bibliotecas internas que não estão disponíveis no repositório central do Maven ou em qualquer outro repositório remoto. Para isso, é possível adicionar essas bibliotecas manualmente no repositório local do Maven (.m2), que fica no seu diretório de usuário. 
A intenção é sempre ter o ambiente o mais atualizado possível e, preferencialmente um repositório compartilhado, porém é valido entender como podemos adicionar outros projetos que criamos como biblioteca no Maven, o que, geralmente, é a primeira barreira para um fair-trade e causa desistência por parte da comunidade, ou, em outros casos, reclamação do ecossistema que envolve o Java.

#### 4.2. Disponibilizando a dependência interna
Exportar a Biblioteca: Primeiro, certifique-se de que a biblioteca interna que você deseja utilizar está buildada em formato JAR. 

#### 4.2.1 Via código
Adicionar o JAR ao Repositório Local: Utilize o comando abaixo para instalar o JAR no repositório local do Maven. Este código terá o mesmo efeito que colar o arquivo dentro da pasta m2 do computador e, na verdade, se trata de apenas abastecer o local onde o Maven busca as bibliotecas. (Quando adicionamos no pom.xml uma dependência, nada mais fazemos que sinalizar o maven que ele precisa ir até o maven repo e trazer uma cópia do JAR de dependência para dentro da nossa pasta m2 e, a partir daí, consumir de lá a biblioteca)


 ```bash
mvn install:install-file -Dfile=/caminho/para/biblioteca-interna.jar -DgroupId=com.exemplo -DartifactId=biblioteca-interna -Dversion=1.0.0 -Dpackaging=jar
```
Nesse comando:

-Dfile: Caminho para o arquivo JAR.
-DgroupId: Grupo ao qual o artefato pertence, normalmente segue a estrutura de pacotes do Java.
-DartifactId: Nome da biblioteca.
-Dversion: Versão do JAR.
-Dpackaging: Tipo de embalagem, que é "jar" na maioria dos casos.
Adicionar a Dependência no pom.xml: Depois que o JAR estiver no repositório local, adicione a dependência ao seu pom.xml:

```xml
<dependencies>
    <dependency>
        <groupId>br.com.exemplo</groupId>
        <artifactId>biblioteca-interna</artifactId>
        <version>1.0.0</version>
    </dependency>
</dependencies>
```

#### 4.2.2 Manualmente
```xml
<dependencies>
    <dependency>
        <groupId>br.com.exemplo</groupId>
        <artifactId>biblioteca-interna</artifactId>
        <version>1.0.0</version>
    </dependency>
</dependencies>
```
Sabe aquele caminho que aparece no groupId? Aquele caminho deve ser o caminho em árvore até o seu JAR a partir da sua pasta m2, que está no seu usuário do Sistema Operacional.
Nesse caso, acessaríamos a pasta m2 e criaríamos as pastas:
├── .m2/
│   └── repository/
│       └── br/
│         └── com/
│           └── witzler/
│             └── nome_do_projeto/
│               └── 1.0/ (versão)
│                 └── nome_do_projeto-VERSION.JAR


#### 4.3. Benefícios de Utilizar Bibliotecas Internas
- Controle Total: Utilizar bibliotecas internas permite maior controle sobre as versões e atualizações do código.
- Segurança: Manter bibliotecas internamente pode ser uma medida de segurança, impedindo o uso de bibliotecas que podem não estar disponíveis publicamente.
- Dependências Personalizadas: Facilita o uso de bibliotecas customizadas que são específicas para um projeto ou uma organização.
- Interação entre projetos internos: Se o produto final a ser entregue se refere a um projeto que depende de outras bibliotecas que são desenvolvidas a parte, seja por um outro time, por outro colaborador ou, separada até mesmo por tema, será necessário ter essas bibliotecas adicionadas no seu projeto, logo é importante ter o domínio dessa técnica.

#### 4.4. Considerações Importantes
Manutenção do Repositório Local: Adicionar bibliotecas manualmente ao repositório local exige cuidado na manutenção, especialmente se houver múltiplas versões ou se a equipe de desenvolvimento for grande.
Portabilidade: Quando outras pessoas precisam trabalhar no mesmo projeto, você deve assegurar que elas também tenham essas bibliotecas instaladas localmente, ou considere configurar um repositório Maven interno compartilhado.

## 5. Conclusão

### 5.1. Vantagens da Atualização para Maven
- Simplificação do gerenciamento de dependências
- Ciclo de vida de construção predefinido
- Estrutura de projeto padronizada
- Extensibilidade com uma ampla gama de plugins

### 5.2. Desafios e Considerações
- Curva de aprendizado para novos usuários
- Possíveis ajustes necessários para customizações específicas de projetos

### 5.3. Passos Finais
- Testar a migração em um ambiente de desenvolvimento antes de mover para produção
- Aproveitar a documentação extensa e comunidade ativa do Maven para suporte e melhores práticas

Com esses detalhes adicionais, o documento fornece uma visão abrangente e prática sobre a migração de ANT para Maven, especialmente focando nas vantagens e facilidades que Maven oferece, incluindo a estrutura de pacotes `br.com.witzlerenergia.COW.java`.
