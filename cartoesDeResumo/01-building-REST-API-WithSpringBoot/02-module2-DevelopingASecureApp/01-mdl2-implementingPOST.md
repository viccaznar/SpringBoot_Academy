# Implementando o POST  

Nossa API REST agora pode buscar Cash Cards com um ID específico. Nesta lição, você adicionará o endpoint de criação (Create) à API.

Quatro perguntas que precisaremos responder enquanto fazemos isso:


  - **1.** Quem especifica o ID – o cliente ou o servidor?  
  
  - **2.** Na requisição da API, como representamos o objeto a ser criado?  
  
  - **3.** Qual método HTTP devemos usar na requisição?  
  
  - **4.** O que a API envia como resposta?  


**Vamos começar respondendo à primeira pergunta:**
 “Quem especifica o ID?”


  >>>

  Na prática, isso depende do criador da API!


  - REST não é exatamente um padrão rígido;

  - é apenas uma forma de usar o HTTP para realizar operações sobre dados. 
  
  - REST contém várias diretrizes, muitas das quais estamos seguindo neste curso.

  >>>



Aqui, vamos optar por deixar que o servidor crie o ID. Por quê?


  - Porque é a solução mais simples, e bancos de dados são eficientes no gerenciamento de IDs únicos. 



No entanto, para ficarmos completos, vejamos nossas alternativas:


  - Poderíamos exigir que o cliente forneça o ID. Isso faria sentido se já existisse um ID único pré-existente, mas não é o nosso caso.


  - Poderíamos permitir que o cliente forneça o ID opcionalmente (e criar um no servidor caso o cliente não o envie).
   
  - Porém, não há requisito para isso, e complicaria nossa aplicação. Se você acha que pode precisar disso “por via das dúvidas”, o artigo do YAGNI (link em Referências) pode te convencer do contrário.  


Antes de responder à terceira pergunta, “Qual método HTTP deve ser usado na requisição?”, vamos falar sobre o conceito relevante de idempotência.


## Idempotência e HTTP  

>>>

Uma **operação idempotente** é aquela que, 


  - **se executada mais de uma vez, produz o mesmo resultado.** 


  Em uma API REST,


  - Uma **operação idempotente**:


    É aquela que, mesmo que seja chamada várias vezes, deixa os dados no servidor no mesmo estado de ter sido chamada apenas uma vez.

>>>


O padrão HTTP especifica para cada método se ele é idempotente ou não. 


  - `GET, PUT e DELETE` são idempotentes, 


  - enquanto `POST e PATCH` não são.


Como decidimos que o servidor criará IDs para cada operação de Create, nossa operação de criação na API NÃO é idempotente. Como o servidor gera um novo ID a cada requisição Create, se você chamar Create duas vezes — mesmo enviando o mesmo conteúdo — você terá dois objetos distintos com o mesmo conteúdo, mas com IDs diferentes. 


  >>>

  - **Em resumo:** cada requisição Create gera um novo ID, logo, não há idempotência.

  >>>


Isso nos deixa com as opções POST e PATCH. Como REST permite usar POST para operações de criação, usaremos esse método. Revisaremos PATCH em uma lição posterior.


## A requisição e a resposta POST  

Agora, vamos falar sobre o conteúdo da requisição POST e da resposta.


### A Requisição  

O método POST aceita um corpo (Body), então vamos usá-lo para enviar uma representação JSON do objeto:

**Requisição:**

  ```bash
  - Método: POST  
  - URI: /cashcards/  
  - Body:
    ```json
    {
        "amount": 123.45
    }
  ```


Em contraste, se você se lembra da lição anterior, a operação GET inclui o ID do Cash Card na URI, mas não no Body da requisição.
 

  >>>

**Por que não há ID na requisição?**


  - Porque decidimos deixar que o servidor crie o ID. Assim, o contrato de dados para a operação Read é diferente do contrato de Create.

  >>>
 

### A Resposta

Agora, vamos à resposta. Em caso de criação bem-sucedida,

**Qual código de status HTTP deve ser enviado?**


  >>>

  - Poderíamos usar 200 OK (mesmo código do Read), mas há um código mais específico e preciso para APIs REST: 
  
    - 201 CREATED.


  - **O nome CREATED já indica que algo foi criado, mas há um motivo técnico:** 
   
    O código 200 OK não responde à pergunta “Houve alguma alteração nos dados do servidor?”. Ao retornar 201 CREATED, a API comunica explicitamente que dados foram adicionados ao armazenamento no servidor.

  >>>


Você já aprendeu que uma resposta HTTP contém dois elementos: 
  
  
  - Um Status Code e um Body. Mas não é só isso!
  
  - A resposta também inclui Headers. Cada header tem um nome e um valor.
   
  - O padrão HTTP especifica que o header Location em uma resposta 201 CREATED deve conter a URI do recurso criado.
  
  - Isso é útil porque permite ao cliente recuperar facilmente o novo recurso usando o endpoint GET (que já implementamos).


Aqui está a resposta completa:


**Resposta:**

  ```bash
  Status Code: 201 CREATED

  Header: Location=/cashcards/42
  ```


## Métodos de conveniência do Spring Web

No laboratório, você verá que o Spring Web oferece métodos voltados ao uso recomendado de HTTP e REST.

Por exemplo, usaremos o método `ResponseEntity.created(uriOfCashCard)` para criar a resposta acima. Este método exige que:


  >>>

  - Você especifique a localização, 

  - garante que a URI seja bem-formada (usando a classe URI), 

  - adiciona o header Location e já define o Status Code para você. 

  - Isso nos poupa de escrever algo mais verboso. 
  
  >>>


Por exemplo, estes dois trechos de código são equivalentes (desde que uriOfCashCard não seja nulo):


  ```java
  return ResponseEntity
          .created(uriOfCashCard)
          .build();
  ```


  **Versus:**


  ```java
  return ResponseEntity
          .status(HttpStatus.CREATED)
          .header(HttpHeaders.LOCATION, uriOfCashCard.toASCIIString())
          .build();
  ```


  - Não é ótimo que o Spring Web forneça o método de conveniência .created()?

## Resumo

Tomamos algumas decisões sobre como nossa API suportará a criação de Family Cash Cards:

  - A API aceitará requisições POST para criar um Cash Card.

  - O servidor criará IDs para todos os Cash Cards.

  - Em caso de sucesso, a API retornará uma resposta com Status Code 201 CREATED, contendo a URI (location) do novo recurso Cash Card nos headers da resposta.

No laboratório, vamos implementar essas decisões!