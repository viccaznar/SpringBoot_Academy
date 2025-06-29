# Implementando GET

Nesta aula, você vai aprender o que é REST e como usar o Spring Boot para implementar um único endpoint RESTful.

## REST, CRUD e HTTP

**REST significa Representational State Transfer**.

  - Em um sistema RESTful, os objetos de dados são chamados Resource Representations (representações de recurso).


  - **O objetivo de uma API RESTful é gerenciar o estado (state) desses recursos.**


    >>>

    - **“state”** pode ser entendido como “valor”

    e 
    
    - **“Resource Representation”** como um “objeto” ou “entidade”.
    
    >>>


- **REST** é apenas uma forma de gerenciar os valores de coisas que são expostas via API e, normalmente, armazenadas em um repositório persistente, como um banco de dados.


Um conceito frequentemente citado ao falar de REST é CRUD:

  >>>

  - Create (Criar), 
  
  - Read (Ler), 
  
  - Update (Atualizar) e 
  
  - Delete (Excluir). 
  
  >>>


São as quatro operações básicas sobre objetos em um repositório.
Veremos que o REST define diretrizes específicas para cada uma delas.



## Outro conceito ligado a REST é o Protocolo de Transferência de Hipertexto (HTTP).

Em HTTP,
  

  **1)** Um cliente envia uma Requisição (Request) a um URI.
  
  **2)** O servidor web recebe essa requisição e a direciona a um handler.
  
  **3)** O handler gera uma Resposta (Response) que é devolvida ao cliente.


  - Componentes da Requisição e da Resposta:


    >> Requisição  


      - Método (Verb)  

      - URI (Endpoint)  

      - Corpo (Body)  


    >> Resposta


    - Código de Status (Status Code)  

    - Corpo (Body)  


  - Para se aprofundar em métodos de requisição e resposta, consulte o padrão HTTP.


## O poder do REST está em como ele referencia um recurso e em como as requisições e respostas devem ser formatadas para cada operação CRUD. 


Veja como nossa API ficará quando concluirmos este curso:


  - **CREATE:** usar o método POST  
  
  - **READ:** usar o método GET  
  
  - **UPDATE:** usar o método PUT  
  
  - **DELETE:** usar o método DELETE  


O URI base para Cash Card é `/cashcards`.


Operações de **READ, UPDATE e DELETE** exigem um identificador único do recurso (ID), para que a aplicação saiba exatamente em qual objeto atuar.


  - Por exemplo, ler, atualizar ou excluir o Cash Card de ID 42 seria feito em `/cashcards/42`.


Note que em CREATE não fornecemos ID, pois o próprio sistema criará um novo identificador para o recurso.



### Mapa de Operações RESTful

| Operação | Endpoint da API       | Método HTTP | Status de Resposta    |
|----------|-----------------------|-------------|-----------------------|
| Create   | /cashcards            | POST        | 201 (CREATED)         |
| Read     | /cashcards/{id}       | GET         | 200 (OK)              |
| Update   | /cashcards/{id}       | PUT         | 204 (NO CONTENT)      |
| Delete   | /cashcards/{id}       | DELETE      | 204 (NO CONTENT)      |



## Corpo da Requisição  
Para criar ou atualizar um recurso,


  - é preciso enviar um corpo (request body) com os dados necessários.


    Por exemplo, um novo Cash Card pode ter um valor inicial, e uma operação de UPDATE altera esse valor.



## Exemplo de Cash Card  
Para a operação READ, o endpoint é `/cashcards/{id}`, onde `{id}` deve ser substituído pelo identificador do Cash Card. O método é GET e o corpo da requisição fica vazio. Exemplo:


  ```text
  Requisição:
    Método: GET
    URL: http://cashcard.example.com/cashcards/123
    Corpo: (vazio)
  ```


A resposta de sucesso inclui um código 200 (OK) e o corpo com a representação JSON do recurso:


  ```text
  Resposta:
    Código de Status: 200
    Corpo:
    {
      "id": 123,
      "amount": 25.00
    }
  ```


Ao longo do curso, você aprenderá a implementar todas as operações CRUD restantes.



## REST no Spring Boot

### Anotações do Spring e Varredura de Componentes

O Spring configura e instancia objetos chamados Spring Beans, normalmente criando-os em vez do uso de new.

Você pode instruir o Spring a criar Beans de várias formas.


  - Nesta aula, vamos usar anotações que indicam ao Spring, durante a fase de Component Scan (varredura de componentes) na inicialização da aplicação, que ele deve instanciar aquela classe e guardá-la no container IoC.



### Controladores no Spring Web

Em Spring Web, requisições são tratadas por Controllers.
Para APIs REST, usamos `@RestController`:


  ```java
    @RestController
    class CashCardController {
    }
  ```


  - Isso informa ao Spring para registrar um controller REST.
  
  - O Spring Web roteia as requisições recebidas para os métodos desse controller.


Para definir um método que atenda requisições de READ via GET, fazemos:


  ```java
    @GetMapping("/cashcards/{requestedId}")
    private CashCard findById(@PathVariable Long requestedId) {
    }
  ```


  - Aqui, `@PathVariable` indica ao Spring que o valor de {requestedId} na URI seja injetado no parâmetro requestedId.



  - REST exige que a resposta traga o recurso no corpo e o status 200 (OK). Usamos ResponseEntity para criar essa resposta:


  ```java
    @RestController
    class CashCardController {
      @GetMapping("/cashcards/{requestedId}")
      private ResponseEntity<CashCard> findById(@PathVariable Long requestedId) {
        CashCard cashCard = /* código para recuperar o CashCard */;
        return ResponseEntity.ok(cashCard);
      }
  }
  ```



### Resumo

  - Nesta aula você aprendeu como REST usa HTTP para definir operações CRUD, como criar um controller no Spring Boot para implementar o endpoint READ e como usar o container IoC e as anotações do Spring para processar requisição e resposta de forma simples.

  - Agora, vamos implementar nosso primeiro endpoint REST usando Spring e Spring Boot!