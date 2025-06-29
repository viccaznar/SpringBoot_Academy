# MÓDULO 1: LAB - IMPLEMENTANDO O GET 

## 1: Escreva um teste Spring Boot para o endpoint GET  

Assim como em um projeto real, vamos usar desenvolvimento guiado por testes para implementar nosso primeiro endpoint de API.

### 1.1 Escreva o teste.

Vamos começar implementando um teste usando o @SpringBootTest do Spring.

  - **Passo 1:**
  
  Atualize o arquivo `src/test/java/example/cashcard/CashCardApplicationTests.java` com o
  seguinte:


    ```java
      package example.cashcard;

      import com.jayway.jsonpath.DocumentContext;
      import com.jayway.jsonpath.JsonPath;
      import org.junit.jupiter.api.Test;
      import org.springframework.beans.factory.annotation.Autowired;
      import org.springframework.boot.test.context.SpringBootTest;
      import org.springframework.boot.test.web.client.TestRestTemplate;
      import org.springframework.http.HttpStatus;
      import org.springframework.http.ResponseEntity;

      import static org.assertj.core.api.Assertions.assertThat;

      @SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
      class CashCardApplicationTests {
          @Autowired
          TestRestTemplate restTemplate;

          @Test
          void shouldReturnACashCardWhenDataIsSaved() {
              ResponseEntity<String> response = restTemplate.getForEntity("/cashcards/99", String.class);

              assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
          }
      }
    ```



#### Entenda o teste.

Vamos analisar vários elementos importantes neste teste.


  - **`@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)`**:
    
    Isso vai iniciar nossa aplicação Spring Boot e deixá-la disponível para que o teste envie requisições a ela.



  - **`@Autowired TestRestTemplate restTemplate;`**
  
    Estamos pedindo ao Spring para injetar um helper de teste que permite fazer requisições HTTP à aplicação rodando localmente.


    - **Observação:** Embora `@Autowired` seja uma forma de injeção de dependência do Spring, seu uso é recomendado somente em testes.



  - **`ResponseEntity<String> response = restTemplate.getForEntity("/cashcards/99", String.class);`**
  
    Aqui usamos o restTemplate para fazer uma requisição HTTP GET ao endpoint /cashcards/99 da nossa aplicação.


  - O **`restTemplate`** retorna um **`ResponseEntity`**, que capturamos na variável response.
  
    `ResponseEntity` é outro objeto do Spring que traz informações valiosas sobre o resultado da requisição. Usaremos esses dados ao longo dos testes deste curso.


  - **`assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);`**
  
    Podemos inspecionar vários aspectos da resposta, incluindo o código de status HTTP, que esperamos seja 200 OK.



#### Agora, execute o teste.

O que você acha que acontecerá ao rodar o teste? Ele irá falhar, como esperado. Por quê?


  - **Como aprendemos na prática de “test-first”:** descrevemos nossas expectativas antes de implementar o código que as satisfaz.


Agora, vamos rodar o teste. Note que usaremos `./gradlew test` em cada execução.


  ```bash
    [~/exercises] $ ./gradlew test
    Ele falha! Procure no output por:

    CashCardApplicationTests > shouldReturnACashCardWhenDataIsSaved() FAILED
      org.opentest4j.AssertionFailedError:
      expected: 200 OK
      but was: 404 NOT_FOUND
  ```


Mas por que recebemos esse erro específico?

#### Entenda a falha do teste.

Como explicamos, esperávamos que o teste falhasse no momento. 
Por que ele está falhando com um código HTTP 404 NOT_FOUND inesperado?


  - **Resposta:** como ainda não instruímos o Spring Web sobre como tratar GET /cashcards/99, o Spring Web responde automaticamente com NOT_FOUND.


Obrigado por fazer isso por nós, Spring Web!

A seguir, vamos fazer nossa aplicação funcionar corretamente.