# 4: Completar o endpoint GET  

Até agora, nosso teste apenas verifica se a requisição teve sucesso conferindo o status 200 OK. A seguir, vamos testar se a resposta contém os valores corretos.

## Passo 1. Atualize o teste:

  ```java
  assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);

  DocumentContext documentContext = JsonPath.parse(response.getBody());
  Number id = documentContext.read("$.id");
  assertThat(id).isNotNull();
  ```

### Entenda as adições:


  - **`DocumentContext documentContext = JsonPath.parse(response.getBody());`**
  
    Converte a String de resposta em um objeto “consciente” de JSON, com vários métodos auxiliares.


  - **`Number id = documentContext.read("$.id");  assertThat(id).isNotNull();`**

    Esperamos que, ao solicitar um Cash Card de id 99, seja retornado um objeto JSON contendo algum valor no campo id. Por ora, garantimos apenas que id não seja nulo.


## Passo 2. Execute o teste — e note a falha:

Como retornamos um JSON vazio {}, não surpreende que o campo id esteja ausente.

  ```bash
  CashCardApplicationTests > shouldReturnACashCardWhenDataIsSaved() FAILED
      com.jayway.jsonpath.PathNotFoundException: No results for path: $['id']
  ```

  ### Retorne um Cash Card no Controller:

  Vamos fazer o teste passar, mas retornando algo propositalmente errado (1000L). Você entenderá por quê depois.

  Também vamos usar a classe de domínio `CashCard` criada anteriormente `(veja src/main/java/example/cashcard/CashCard.java)`.


    ```java
    @GetMapping("/{requestedId}")
    private ResponseEntity<CashCard> findById() {
      CashCard cashCard = new CashCard(1000L, 0.0);
      return ResponseEntity.ok(cashCard);
    }
    ```

  ### Execute o teste:

  Ele passa! Mas será que está correto? Não muito.
    

    - Um teste passando com dados incorretos não faz sentido. Pediu-se esse 1000L de propósito para ilustrar um ponto: **os testes devem passar ou falhar pelo motivo certo**.


## Passo 3. Atualize o teste:


  ```java
  DocumentContext documentContext = JsonPath.parse(response.getBody());
  Number id = documentContext.read("$.id");
  assertThat(id).isEqualTo(99);
  ```


### Rerun dos testes e observe a nova mensagem de falha:

  ```bash
  expected: 99
  but was: 1000
  ```

  Agora o teste falha pelo motivo correto: **não retornamos o id certo**.


## Passo 4. Corrija o CashCardController:

  ```java
  @GetMapping("/{requestedId}")
  private ResponseEntity<CashCard> findById() {
    CashCard cashCard = new CashCard(99L, 0.0);
    return ResponseEntity.ok(cashCard);
  }
  ```

  ### Execute o teste:

  Oba, passou! 🎉

  Teste o amount:

    ```java
    DocumentContext documentContext = JsonPath.parse(response.getBody());
    Number id = documentContext.read("$.id");
    assertThat(id).isEqualTo(99);

    Double amount = documentContext.read("$.amount");
    assertThat(amount).isEqualTo(123.45);
    Rerun dos testes e veja a falha:
    ```

    ```bash
    expected: 123.45
    but was: 0.0
    ```

  ### Retorne o amount correto:

    ```java
    @GetMapping("/{requestedId}")
    private ResponseEntity<CashCard> findById() {
      CashCard cashCard = new CashCard(99L, 123.45);
      return ResponseEntity.ok(cashCard);
    }
    ```

  ### Rerun dos testes:

    Eles passam! Excelente.

    BUILD SUCCESSFUL in 6s