# 3: Adicionar o endpoint GET  

## Passo 1. Atualize o Controller.

Vamos atualizar nosso `CashCardController` para que ele seja configurado para escutar e tratar requisições HTTP em `/cashcards`:

   ```java
   @RestController
   @RequestMapping("/cashcards")
   class CashCardController {

      @GetMapping("/{requestedId}")
      private ResponseEntity<String> findById() {
            return ResponseEntity.ok("{}");
      }
   }
   ```

### Entenda as anotações do Spring Web.

Vamos revisar o que adicionamos:


- **`@RestController`:**
   
   Informa ao Spring que esta classe é um componente do tipo REST Controller, capaz de tratar requisições HTTP.


- **`@RequestMapping("/cashcards")`:**
   
   Indica o caminho base (URL) que deve ser usado para acessar este Controller.



- **`@GetMapping("/{requestedId}")`:**

   Marca o método como um handler para requisições GET que correspondam ao padrão /cashcards/{requestedId}.


## Passo 2. Execute os testes.

Eles passam!

   ```bash
   [~/exercises] $ ./gradlew test
   ...
   BUILD SUCCESSFUL in 6s
   ```

Finalmente temos um Controller e um método handler que correspondem à requisição feita no nosso teste. Show!