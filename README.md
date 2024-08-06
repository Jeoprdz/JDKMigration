# JDKMigration
Detalhamento do processo de migração do JDK 8 para o JDK 21

Claro, vou expandir as descrições, fornecer mais detalhes sobre as mudanças e incluir referências à documentação oficial quando possível.

## 1. Introdução

### 1.1. O que é o JDK?
O Java Development Kit (JDK) é um ambiente de desenvolvimento de software usado para desenvolver aplicativos Java e applets. Inclui o Java Runtime Environment (JRE), um interpretador/loader (java), um compilador (javac), um arquivador (jar), um gerador de documentação (Javadoc) e outras ferramentas necessárias no desenvolvimento Java.

### 1.2. Histórico de versões
- **JDK 8**: Lançado em março de 2014, JDK 8 introduziu várias funcionalidades importantes, como lambdas, Streams API, e a nova API de Data e Hora (java.time).
- **JDK 21**: Lançado em setembro de 2024, JDK 21 traz uma série de aprimoramentos em desempenho, segurança, novas funcionalidades de linguagem e APIs.

### 1.3. Importância da Atualização
Atualizar para uma versão mais recente do JDK pode trazer melhorias significativas em termos de desempenho, segurança e novas funcionalidades. Além disso, a manutenção do suporte a longo prazo (LTS) garante que a versão recebida seja atualizada com correções de bugs e patches de segurança por vários anos.

## 2. Novas Funcionalidades e Melhorias

### 2.1. Linguagem e Sintaxe
#### 2.1.1. Var (Inferência de Tipo)
A inferência de tipo com `var` foi introduzida no JDK 10, permitindo que os desenvolvedores omitam o tipo explícito nas declarações locais, onde o compilador pode inferir o tipo com base no valor atribuído.

- **JDK 8**: Tipos explícitos
  ```java
  String mensagem = "Olá, mundo!";
  ```
- **JDK 21**: Inferência de tipo
  ```java
  var mensagem = "Olá, mundo!";
  ```
  **Detalhe:** `var` só pode ser usado para variáveis locais e não pode ser utilizado em declarações de variáveis de instância, parâmetros de método ou variáveis de retorno.

#### 2.1.2. Strings Multilinha
Os Text Blocks foram introduzidos no JDK 13 (como prévia) e estabilizados no JDK 15. Eles permitem a criação de strings multilinha de maneira mais legível e menos propensa a erros.

- **JDK 8**: Concatenação de strings
  ```java
  String texto = "Linha 1\n" +
                 "Linha 2\n" +
                 "Linha 3";
  ```
- **JDK 21**: Text blocks
  ```java
  String texto = """
                 Linha 1
                 Linha 2
                 Linha 3
                 """;
  ```
  **Detalhe:** Text blocks automaticamente gerenciam os caracteres de nova linha e preservam a formatação do texto, tornando-os ideais para JSON, XML, HTML ou qualquer outro formato de texto estruturado.

### 2.2. APIs e Bibliotecas
#### 2.2.1. Collections
O JDK 9 introduziu métodos de fábrica para coleções, simplificando a criação de listas, conjuntos e mapas imutáveis.

- **JDK 8**: Inicialização manual de coleções
  ```java
  List<String> lista = new ArrayList<>();
  lista.add("A");
  lista.add("B");
  lista.add("C");
  ```
- **JDK 21**: Factory methods para coleções
  ```java
  List<String> lista = List.of("A", "B", "C");
  ```
  **Detalhe:** As coleções criadas com os métodos `List.of`, `Set.of` e `Map.of` são imutáveis e lançam uma `UnsupportedOperationException` se houver uma tentativa de modificação.

#### 2.2.2. Streams e APIs Funcionais
Os Streams foram introduzidos no JDK 8 e receberam várias melhorias nas versões subsequentes.

- **JDK 8**: Introdução das Streams
  ```java
  List<String> lista = Arrays.asList("a", "b", "c");
  lista.stream()
       .map(String::toUpperCase)
       .forEach(System.out::println);
  ```
- **JDK 21**: Melhorias em Streams
  ```java
  List<String> lista = List.of("a", "b", "c");
  lista.stream()
       .map(String::toUpperCase)
       .forEach(System.out::println);
  ```
  **Detalhe:** A API de Streams recebeu melhorias de desempenho e novos métodos de conveniência, como `takeWhile`, `dropWhile`, `iterate` com limite, entre outros.

### 2.3. Concorência e Performance
#### 2.3.1. CompletableFuture
O `CompletableFuture` foi introduzido no JDK 8 como uma maneira poderosa e flexível de lidar com operações assíncronas e computações concorrentes.

- **JDK 8**: Introdução do CompletableFuture
  ```java
  CompletableFuture.supplyAsync(() -> "Resultado")
                   .thenAccept(System.out::println);
  ```
- **JDK 21**: Melhorias em CompletableFuture
  ```java
  CompletableFuture.supplyAsync(() -> "Resultado")
                   .thenAccept(System.out::println);
  ```
  **Detalhe:** As melhorias incluem novos métodos de fábrica, suporte aprimorado a encadeamento de tarefas assíncronas e integração com novas APIs de concorrência.

#### 2.3.2. Virtual Threads (Project Loom)
O Project Loom introduziu threads virtuais no JDK 21, permitindo a criação de milhões de threads leves e de baixo custo, melhorando a escalabilidade de aplicações concorrentes.

- **JDK 21**: Introdução de virtual threads
  ```java
  try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
      IntStream.range(0, 100)
               .forEach(i -> executor.submit(() -> {
                   Thread.sleep(1000);
                   return i;
               }));
  }
  ```
  **Detalhe:** Threads virtuais são gerenciadas pelo JVM de forma mais eficiente que as threads tradicionais, reduzindo a sobrecarga de criação e troca de contexto.

### 2.4. Segurança
#### 2.4.1. Criptografia e TLS
O JDK 8 suportava TLS 1.2, enquanto o JDK 21 traz suporte a TLS 1.3 e melhorias nos algoritmos de criptografia, proporcionando maior segurança e desempenho em conexões de rede.

### 2.5. Ferramentas e Diagnósticos
#### 2.5.1. JFR (Java Flight Recorder)
O Java Flight Recorder (JFR) era uma ferramenta comercial até o JDK 11, quando foi incluída no OpenJDK.

- **JDK 8**: JFR como ferramenta comercial
- **JDK 21**: JFR incluído no OpenJDK, permitindo profiling e diagnóstico detalhado de desempenho sem impacto significativo na performance.

## 3. Mudanças Deprecadas e Removidas

### 3.1. Remoção de APIs Obsoletas
Com cada nova versão do JDK, algumas APIs são deprecadas e eventualmente removidas. A lista completa de APIs removidas pode ser encontrada na documentação oficial do JDK 21.

### 3.2. Ajustes Necessários no Código
Para compatibilidade, é necessário ajustar o código para usar APIs alternativas e remover o uso de funcionalidades deprecadas.

## 4. Exemplos Práticos

### 4.1. Atualização de Projeto
Passo a passo para atualizar um projeto do JDK 8 para o JDK 21:
1. Atualizar o arquivo de configuração do projeto (pom.xml, build.gradle, etc.) para usar o JDK 21.
2. Compilar o projeto e identificar erros de compilação relacionados a APIs deprecadas ou removidas.
3. Ajustar o código conforme necessário, usando as novas APIs e funcionalidades.
4. Testar o projeto extensivamente para garantir que todas as funcionalidades funcionem conforme esperado.

### 4.2. Exemplos de Benefícios
Demonstração de melhorias de desempenho e segurança:
- Comparar tempo de execução de tarefas concorrentes usando threads tradicionais vs. threads virtuais.
- Medir a latência de conexões de rede usando TLS 1.2 vs. TLS 1.3.

## 5. Conclusão

### 5.1. Vantagens da Atualização
- Melhor desempenho e escalabilidade
- Melhorias de segurança
- Novas funcionalidades e sintaxe mais concisa
- Suporte a longo prazo

### 5.2. Desafios e Considerações
- Possíveis incompatibilidades com bibliotecas e frameworks
- Necessidade de ajustes no código legado

## 6. Referências
- [Documentação Oficial do JDK 21](https://docs.oracle.com/en/java/javase/21/)
- [Guia de Migração para o JDK 21](https://docs.oracle.com/en/java/javase/21/migrate/overview.html)
- [Java Flight Recorder](https://docs.oracle.com/en/java/javase/21/jfrock/overview.html)
- [Project Loom](https://openjdk.java.net/projects/loom/)

---

### Exemplos Detalhados de Código

#### Inferência de Tipo com `var`

**JDK 8:**
```java


public class Exemplo {
    public static void main(String[] args) {
        String mensagem = "Olá, mundo!";
        System.out.println(mensagem);
    }
}
```

**JDK 21:**
```java
public class Exemplo {
    public static void main(String[] args) {
        var mensagem = "Olá, mundo!";
        System.out.println(mensagem);
    }
}
```

#### Text Blocks para Strings Multilinha

**JDK 8:**
```java
public class Exemplo {
    public static void main(String[] args) {
        String texto = "Linha 1\n" +
                       "Linha 2\n" +
                       "Linha 3";
        System.out.println(texto);
    }
}
```

**JDK 21:**
```java
public class Exemplo {
    public static void main(String[] args) {
        String texto = """
                       Linha 1
                       Linha 2
                       Linha 3
                       """;
        System.out.println(texto);
    }
}
```

#### Melhorias em Streams

**JDK 8:**
```java
import java.util.Arrays;
import java.util.List;

public class Exemplo {
    public static void main(String[] args) {
        List<String> lista = Arrays.asList("a", "b", "c");
        lista.stream()
             .map(String::toUpperCase)
             .forEach(System.out::println);
    }
}
```

**JDK 21:**
```java
import java.util.List;

public class Exemplo {
    public static void main(String[] args) {
        List<String> lista = List.of("a", "b", "c");
        lista.stream()
             .map(String::toUpperCase)
             .forEach(System.out::println);
    }
}
```

#### Virtual Threads (Project Loom)

**JDK 21:**
```java
import java.util.concurrent.Executors;
import java.util.stream.IntStream;

public class Exemplo {
    public static void main(String[] args) throws InterruptedException {
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            IntStream.range(0, 100)
                     .forEach(i -> executor.submit(() -> {
                         Thread.sleep(1000);
                         return i;
                     }));
        }
    }
}
```

Esses exemplos demonstram as diferenças e benefícios das novas funcionalidades introduzidas no JDK 21 em comparação com o JDK 8. As referências fornecem documentação adicional e guias para ajudar na transição.