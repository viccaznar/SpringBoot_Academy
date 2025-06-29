# 2: Criar um REST Controller  

## Criando os Controllers do Spring Web

Controllers do Spring Web são projetados para lidar e responder a requisições HTTP.

### Passo 1. **Criar o Controller.**


  - Crie o arquivo **`CashCardController.java`** 


  - Crie a classe Controller em `src/main/java/example/cashcard/CashCardController.java`.
    
    ```java
      package example.cashcard;

      import org.springframework.http.ResponseEntity;
      import org.springframework.web.bind.annotation.GetMapping;
      import org.springframework.web.bind.annotation.PathVariable;
      import org.springframework.web.bind.annotation.RequestMapping;
      import org.springframework.web.bind.annotation.RestController;

      class CashCardController {
      }
      ```



### Passo 2. Adicione o método handler

Implemente um método **`findById()`** para tratar requisições HTTP recebidas.


  ```java
  class CashCardController {
    private ResponseEntity<String> findById() {
        return ResponseEntity.ok("{}");
    }
  }
  ```


### Passo 3. Agora, reexecute o teste.

O que esperamos que aconteça ao reexecutar os testes?


  ```bash
  esperado: 200 OK

  mas foi: 404 NOT_FOUND
  ```

  
Mesmo resultado! Por quê?
  

  - Apesar do nome, CashCardController não é realmente um Controller do Spring Web; é apenas uma classe com “Controller” no nome. 
  

  - Portanto, ela não está “ouvindo” nossas requisições HTTP. Por isso, precisamos informar ao Spring para disponibilizar esse Controller como um Controller Web capaz de tratar requisições para as URLs cashcards/*.