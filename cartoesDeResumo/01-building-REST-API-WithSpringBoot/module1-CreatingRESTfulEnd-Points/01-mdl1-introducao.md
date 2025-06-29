# 🚀 Implementando o Endpoint GET de Forma Detalhada e Robusta

Aprofunde-se no REST, CRUD e HTTP, e veja como configurar um endpoint **GET** no Spring Boot para recuperar um recurso `CashCard`.


## 🌐 1. REST, CRUD e HTTP: Fundamentos e Boas Práticas

**REST (Representational State Transfer)**  
É um estilo arquitetural que trata cada entidade do domínio como um recurso endereçável por URI, manipulável via métodos HTTP.


  - **Exemplo lúdico (criança):** Imagine que cada brinquedo em uma prateleira tem seu próprio endereço, e você envia cartas (comandos) para dizer a ele o que fazer.  


  - **Exemplo prático e boas práticas:**  
    - Use URIs claras e consistentes (`/cashcards/{id}`).  
    - Respeite a semântica HTTP: GET para leitura, POST para criação etc.  
    - Documente no OpenAPI para que consumidores saibam exatamente como usar sua API.


  **CRUD mapeado em HTTP**  
  | Operação | Método HTTP | URI                         | Código de Resposta |  
  |----------|-------------|-----------------------------|--------------------|  
  | Create   | POST        | `/cashcards`                | 201 CREATED        |  
  | Read     | GET         | `/cashcards/{id}`           | 200 OK             |  
  | Update   | PUT         | `/cashcards/{id}`           | 204 NO CONTENT     |  
  | Delete   | DELETE      | `/cashcards/{id}`           | 204 NO CONTENT     |  


  - **Exemplo lúdico:**
    
    - **Create**: dar um presente (novo brinquedo) para a prateleira.  
    - **Read**: olhar o brinquedo e ver sua cor.  
    - **Update**: pintar o brinquedo de outra cor.  
    - **Delete**: tirar o brinquedo da prateleira.  
  
  
  - **Exemplo prático:** validar cada operação com testes automatizados (unit, integração, E2E).


## 📦 2. Estrutura da Requisição e Resposta

**Request**  

  - **Method**: verbo HTTP que define a ação.  
  - **URI**: recurso alvo (`/cashcards/42`).  
  - **Headers**: metadados como `Authorization`, `Content-Type`.  
  - **Body**: JSON para POST/PUT.  


**Response**

  - **Status Code**: indica sucesso (2xx) ou erro (4xx, 5xx).  
  - **Headers**: `Content-Type: application/json`, caching, CORS.  
  - **Body**: JSON ou mensagem de erro.  


  - **Exemplo lúdico:** enviar um cartão postal (request) e receber uma carta de volta (response) com status de “entregue”.
    
  - **Exemplo prático:**
    
    ```http
    GET /cashcards/123 HTTP/1.1
    Host: api.exemplo.com
    Authorization: Bearer <token>
    ```


## 💳 3. Cenário de GET para CashCard

**Objetivo:** recuperar o saldo do cartão de mesada do filho de ID 123.

  **Requisição:**

    - GET /cashcards/123
    - Body vazio


  **Respostas possíveis:**

    - 200 OK + JSON do cartão

    - 404 NOT FOUND se não existir

    - 401 UNAUTHORIZED se credenciais inválidas


  - **Exemplo lúdico:**A criança faz um pedido de mesada via varinha mágica (URI). 
  A varinha retorna o valor atual (response).

  - **Exemplo prático:** Teste unitário no serviço:


  ```java
  when(repo.findById(123L)).thenReturn(Optional.of(new CashCard(123L,25.0)));
  var resp = controller.getCashCardById(123L);
  assertEquals(200, resp.getStatusCodeValue());
  assertEquals(25.0, resp.getBody().getAmount());
  ```


## 🏗️ 4. Configurando REST no Spring Boot

### 🔍 4.1. IoC, Beans e Component Scan

No startup, o Spring faz Component Scan nos pacotes (ex.: example.cashcard) e registra classes anotadas como Beans.

  - Objetos são gerenciados pelo IoC Container, permitindo injeção de dependência.

  >>> - **Boas práticas:**

    - Organize seus pacotes por camada (controller, service, repository).
  
    - Use @Service para lógica de negócio e @Repository para acesso a dados.

  >>>

  - **Exemplo lúdico:** o Spring é como um jardim onde plantas (Beans) são cultivadas automaticamente onde receberam sementes (anotações).

  - **Exemplo prático:**

    ```java
      @Service
      public class CashCardService { /* ... */ }
    ```

### 📡 4.2. Definindo o Controller REST

Anote com @RestController e @RequestMapping("/cashcards") para agrupar endpoints relacionados.

  - Esta classe atua como ponto de entrada HTTP, delegando a lógica ao Service.

  ```java
  @RestController
  @RequestMapping("/cashcards")
  public class CashCardController {
      private final CashCardService service;
      public CashCardController(CashCardService service) {
          this.service = service;
      }
  }
  ```

 - **Exemplo lúdico:** O Controller é o concierge de um hotel, recebendo pedidos e encaminhando às equipes internas (Services).

 - **Exemplo prático:** Separe regras de autenticação/autorização no SecurityConfig, não no Controller.


### 🚦 4.3. Mapeando GET com @GetMapping e @PathVariable

  ```java
  @GetMapping("/{id}")
  public ResponseEntity<CashCard> getCashCardById(@PathVariable Long id) {
      return service.findById(id)
          .map(ResponseEntity::ok)
          .orElse(ResponseEntity.notFound().build());
  }
  @GetMapping("/{id}"): linka GET /cashcards/{id} a este método.

  @PathVariable: extrai valor do placeholder {id} na URI.
  ```

 - **Exemplo lúdico:** É como um totem mágico que, ao inserir o número do cartão, devolve seu conteúdo.

 - **Exemplo prático:** Valide id não negativo com @Min(1) e retorne 400 BAD REQUEST para valores inválidos.


## 📨 4.4. Construindo a Resposta com ResponseEntity

ResponseEntity permite controlar status, headers e corpo:

  ```java
  return ResponseEntity.status(HttpStatus.OK)
                      .header("X-Custom-Header", "value")
                      .body(cashCard);
  ```

  >>> - **Boas práticas:**

  - Sempre retorne JSON consistente (mesmos campos e formato).

  - Inclua cabeçalhos de cache se o recurso for estático ou pouco mutável.

  >>>

  - **Exemplo lúdico:** Embale o brinquedo (Body) em uma caixa (ResponseEntity) com etiqueta de “frágil” (headers).

  - **Exemplo prático:** Use Cache-Control e ETag para otimizar requests subsequentes.


# 🎯 Resumo

  REST transforma objetos em recursos HTTP.

  CRUD mapeia para métodos HTTP e URIs padronizadas.

  Spring Boot automatiza detecção de Beans e configuração de Controllers.

  Use @GetMapping, @PathVariable e ResponseEntity para construir endpoints robustos.

  Agora, implemente e teste seu GET em CashCardController com casos de sucesso e erro antes de prosseguir para POST, PUT e DELETE!