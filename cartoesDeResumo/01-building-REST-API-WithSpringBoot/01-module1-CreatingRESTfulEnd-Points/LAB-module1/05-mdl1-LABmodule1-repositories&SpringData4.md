# 4: Crie o CashCardRepository  

## Passo 1. Crie o CashCardRepository.

Crie o arquivo `src/main/java/example/cashcard/CashCardRepository.java` e faça-o estender `CrudRepository`.


  ```java
  package example.cashcard;

  import org.springframework.data.repository.CrudRepository;

  interface CashCardRepository extends CrudRepository {
  }
  ```


- **Entenda o extends CrudRepository.**


  É aqui que aproveitamos a “mágica” do Spring Data e seu padrão de repositório de dados.


    - CrudRepository é uma interface fornecida pelo Spring Data. 


    - Quando a estendemos (ou estendemos outras subinterfaces de Repository do Spring Data), o Spring Boot e o Spring Data trabalham juntos para gerar automaticamente os métodos CRUD necessários para interagir com um banco de dados. 


    - Usaremos um desses métodos CRUD, findById, mais adiante no laboratório.


## Passo 2. Execute os testes. 

Vemos que tudo compila, mas nossa aplicação falha ao iniciar. Nas mensagens de erro, encontramos:


  ```bash
  Caused by:
  java.lang.IllegalArgumentException: Could not resolve domain type of interface example.cashcard.CashCardRepository
  ...
  ```


Esse erro críptico significa que não indicamos qual objeto de domínio o CashCardRepository deve gerenciar. Para nossa aplicação, o “tipo de domínio” desse repositório será CashCard.


## Passo 3. Configure o CashCardRepository.


  - Edite o CashCardRepository para especificar que ele gerencia dados de CashCard e que o tipo de dado do ID é Long.


    ```java
    interface CashCardRepository extends CrudRepository<CashCard, Long> {
    }
    ```


## Passo 4. Configure o CashCard. 
  
Ao configurarmos o repositório como `CrudRepository<CashCard, Long>`, indicamos que o ID de CashCard é Long. 
  

  - Ainda assim, precisamos dizer ao Spring Data qual campo é o ID. Edite a classe CashCard para marcar o campo id com `@Id`:

  ```java
  package example.cashcard;

  // Adicione este import
  import org.springframework.data.annotation.Id;

  record CashCard(@Id Long id, Double amount) {
  }
  ```


## Passo 5. Execute os testes:

  ```bash
  [~/exercises] $ ./gradlew test
  ...
  BUILD SUCCESSFUL in 4s
  ```

  - Os testes passam, mas ainda não fizemos nenhuma mudança significativa no código... ainda!