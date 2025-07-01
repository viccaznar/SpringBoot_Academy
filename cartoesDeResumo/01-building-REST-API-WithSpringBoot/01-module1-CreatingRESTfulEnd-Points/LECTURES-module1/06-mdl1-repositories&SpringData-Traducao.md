# Repositórios & Spring Data  

Até agora, temos um sistema que retorna um registro de Cash Card codificado diretamente no Controller. No entanto, o que realmente queremos é fornecer dados reais, vindos de um banco de dados. Então, vamos continuar nosso Steel Thread voltando nossa atenção para o banco de dados!

  >>>

  - O Spring Data funciona com o Spring Boot para simplificar a integração com bancos de dados.
  
  - Antes de mergulharmos na prática, vamos falar brevemente sobre a arquitetura do Spring Data.

  >>>

## Arquitetura Controller–Repository  

**O princípio da Separação de Responsabilidades (Separation of Concerns)** diz que:

- Um software bem projetado deve ser modular, com cada módulo tocando em responsabilidades distintas de qualquer outro.

  >>>

  Até agora, nosso código retorna apenas uma resposta fixa diretamente do Controller.


  - Isso quebra a Separação de Responsabilidades, pois mistura a lógica de interface web `(Controller)` com a lógica de leitura/gravação de dados `(acesso ao banco)`.


  - Para resolver isso, usaremos um padrão comum de arquitetura: o **Repository**.

  >>>


Em arquiteturas em camadas `(Layered Architecture)`, cada camada cumpre uma função — por exemplo, camada de apresentação, camada de negócio e camada de dados.

  >>>

  Podemos pensar no Controller e no Repository como duas camadas:  


  - O **Controller** fica próximo ao cliente (lida com requisições web).  


  - O **Repository** fica próximo ao banco de dados (lê e grava dados).  

  >>>


O Repository é a interface entre a aplicação e o banco de dados, fornecendo uma abstração comum que facilita a troca de implementações de banco, se necessário.

  - E a boa notícia é que o Spring Data já entrega implementações robustas do padrão Repository.


## Escolhendo um Banco de Dados  
Para nosso projeto, usaremos um banco de dados embarcado e em memória.  

  
  - **Embarcado** significa que é uma biblioteca Java adicionada como dependência.  

  
  - **Em memória** significa que os dados ficam apenas na RAM, não em armazenamento persistente.  



Ainda assim, esse banco em memória é compatível com bancos relacionais de produção `(RDBMS)` como MySQL, SQL Server etc.

Ele usa:

  - `JDBC (biblioteca Java padrão para acesso a dados)` 
  
  e 
  
  - `SQL (linguagem padrão de consulta)`.



Há prós e contras nessa escolha:

  - **Prós:**
    Em memória dispensa a instalação de um RDBMS separado e garante que, a cada execução de teste, o banco comece vazio.



  - **Contras:**  
    Mas em produção você precisará de um banco persistente, gerando uma diferença entre desenvolvimento e produção (**Dev-Prod Parity**).


  Vamos usar o H2, um banco em memória amplamente compatível, minimizando esse descompasso.


## Auto Configuração  
No laboratório, basta adicionar duas dependências para ter funcionalidade completa de banco. Isso demonstra um dos maiores trunfos do Spring Boot: a Auto Configuração.


  - Sem ela, teríamos que configurar manualmente o Spring Data para se comunicar com o H2. 


  - Mas, incluindo a dependência do Spring Data e do H2, o Spring Boot faz todo o resto automaticamente.



## CrudRepository do Spring Data  
Para nosso Repository, usaremos o `CrudRepository` do Spring Data. À primeira vista parece mágica, mas veja como é simples:


  ```java
  interface CashCardRepository extends CrudRepository<CashCard, Long> {
  }
  ```

  Com este único trecho, você herda métodos prontos para CRUD, como:


    ```java
    CashCard card = cashCardRepository.findById(99);
    ```


Você pode se perguntar: onde está a implementação de findById()?

  -O CrudRepository é uma interface sem código concreto, mas o Spring Data, no início da aplicação, gera a implementação automaticamente e registra o repositório como um Bean no container IoC.

  - Há trade-offs: o CrudRepository gera SQL padrão, ótimo na maioria dos casos, mas às vezes é preciso SQL customizado. Por ora, vamos aproveitar os métodos out-of-the-box e seguir para o laboratório.

## Neste artigo

  - Arquitetura Controller–Repository

  - Escolhendo um Banco de Dados

  - Auto Configuração

  - CrudRepository do Spring Data



## Resumo

A maioria das aplicações é inútil sem os dados que os usuários precisam gerenciar. 

Felizmente, o Spring Data se integra perfeitamente ao Spring e ao Spring Boot para tornar o armazenamento e gestão de dados muito mais fáceis. 


  - Repositórios plug-and-play reduzem a necessidade de escrever grande quantidade de código de acesso a dados. 

  
  - Além disso, o potente mecanismo de Auto Configuração do Spring facilita a implementação de uma arquitetura em camadas.

Agora, vamos ao laboratório — onde você adicionará conectividade com banco de dados à sua API REST usando Spring Data Repositories.