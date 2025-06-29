# 5: Usando o @PathVariable  

Até agora, ignoramos o `requestedId` no método handler do Controller. Vamos usar essa variável de caminho no nosso Controller para garantir que retornemos o Cash Card correto.

## Passo 1. Adicione um novo método de teste.

Vamos escrever um teste que espere não retornar Cash Cards cujo `id` não seja 99. Use 1000, como fizemos nos testes anteriores:


   ```java
   @Test
   void shouldNotReturnACashCardWithAnUnknownId() {
   ResponseEntity<String> response = restTemplate.getForEntity("/cashcards/1000", String.class);

   assertThat(response.getStatusCode()).isEqualTo(HttpStatus.NOT_FOUND);
   assertThat(response.getBody()).isBlank();
   }
   ```


Observe que esperamos um código de status HTTP semântico 404 NOT_FOUND. Se requisitarmos um Cash Card que não existe, ele deve realmente ser “não encontrado”.

   ### Execute o teste e anote o resultado:

   ```bash
   expected: 404 NOT_FOUND
   but was: 200 OK
   ```


## Passo 2. Adicione o @PathVariable.

Vamos fazer o teste passar, retornando o Cash Card específico somente se submetermos o identificador correto.

   Para isso, primeiro torne o Controller ciente da variável de caminho que enviamos, adicionando a anotação @PathVariable ao argumento do método handler:

   ```java
   @GetMapping("/{requestedId}")
   private ResponseEntity<CashCard> findById(@PathVariable Long requestedId) {
      CashCard cashCard = new CashCard(99L, 123.45);
      return ResponseEntity.ok(cashCard);
   }
   ```

   - O `@PathVariable` faz com que o Spring Web reconheça o requestedId fornecido na requisição HTTP. Agora ele está disponível para usarmos dentro do método handler.



## Passo 3. Utilize o @PathVariable.

Atualize o método handler para retornar uma resposta vazia com status NOT_FOUND a menos que o requestedId seja 99:


   ```java
   @GetMapping("/{requestedId}")
   private ResponseEntity<CashCard> findById(@PathVariable Long requestedId) {
      if (requestedId.equals(99L)) {
         CashCard cashCard = new CashCard(99L, 123.45);
         return ResponseEntity.ok(cashCard);
      } else {
         return ResponseEntity.notFound().build();
      }
   }
   ```


## Passo 4. Rerun dos testes.

Excelente! Todos passam!

   ```bash
   [~/exercises] $ ./gradlew test
   ...
   BUILD SUCCESSFUL in 6s
   ```