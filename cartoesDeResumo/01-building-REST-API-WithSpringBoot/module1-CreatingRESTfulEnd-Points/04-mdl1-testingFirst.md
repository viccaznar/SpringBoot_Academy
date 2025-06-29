# 🧪 Testando Primeiro – TDD em Profundidade
Uma abordagem Test-First (TDD) guia o design do sistema pelo comportamento desejado, garantindo código enxuto, documentado e protegido contra regressões.


## 🤔 1. O que é TDD?  
Test Driven Development é a prática de **escrever testes antes de implementar funcionalidades**. Os testes definem especificações executáveis que guiam o design do código.  


  - **Exemplo lúdico (criança):** “Antes de construir um castelo de cartas, você desenha no papel cada torre e ponte, garantindo que elas se encaixem antes de empilhar as cartas de verdade.”  


  >>>  - **Exemplo prático & boas práticas:**  
    - Crie um teste JUnit/AssertJ no perfil `src/test` que falhe intencionalmente:  

    ```java
      @Test
      void deveRetornar404QuandoNaoEncontrado() {
        mockMvc.perform(get("/cashcards/999"))
              .andExpect(status().isNotFound());
      }
    ```  

    - Garanta que cada teste cubra apenas um comportamento bem definido.

  >>>


## ❓ 2. Por que aplicar TDD?  
- **Design orientado a comportamento**: você codifica para **satisfazer requisitos**, não para consertar bugs.  
- **Documentação viva**: a suíte de testes descreve como a API deve se comportar.  
- **Feedback rápido**: falhas imediatas apontam exatamente onde a implementação diverge da especificação.


  - **Exemplo lúdico:** “É como construir Lego no escuro com uma lanterna: os testes são seu farol, iluminando onde colocar cada peça.”  


>>>  - **Exemplo prático & boas práticas:**  
  
  - Defina critérios de aceite em tickets e converta-os em testes automatizados (ex.: Cucumber/Gherkin para BDD). 
  
  - Use cobertura de teste (JaCoCo) para garantir que funcionalidades críticas não fiquem sem validação.

>>>


## 👍 3. Benefícios do Test-First  
1. **Código enxuto**  
- Escreve-se apenas o necessário para passar no teste; evita classes/métodos supérfluos.

2. **Proteção contra regressões**  
- Testes falham se mudanças quebram contratos já estabelecidos.  

3. **Facilidade de refatoração**  
- Com uma suíte confiável, você pode reorganizar o código sem medo de quebrar funcionalidades.


  - **Exemplo lúdico:** “Colocar rodinhas num carrinho de brinquedo para aprender a andar sem cair — os testes são essas rodinhas.”  


  - **Exemplo prático & boas práticas:**  
    - Integre os testes no pipeline CI (GitHub Actions, Jenkins) para bloquear merge com falhas.  
    - Agrupe testes por feature e categorize-os (unit, integration, end-to-end).


## 🧱 4. A Pirâmide de Testes  
Equilibre **rapidez**, **confiabilidade** e **custo de manutenção** em três camadas:

  🔹 **Unit Tests** (70%)  
    - Focam em uma única classe ou método, isolados de dependências externas.  
    - São rápidos (<10ms cada) e fáceis de manter.  


  ```java
  @Test
  void calculaSaldoAposOperacaoSimples() {
      CashCard card = new CashCard(1L, 100.0);
      card.addAmount(50.0);
      assertEquals(150.0, card.getAmount());
  }
  ```


  >>> - **Boas práticas:**

    - Use mocks/stubs (Mockito) para isolar serviços e repositórios.

    - Mantenha testes determinísticos e sem I/O.

      🔸 Integration Tests (20%)

    - Validam fluxo entre componentes (por ex., Service + Repository + banco em memória).


    ```java
      @SpringBootTest
      @Transactional
      class CashCardIntegrationTest {
        @Autowired CashCardService service;
        @Test
        void salvaERecuperaDoBanco() {
          CashCard card = service.create(new CashCard(null, 200.0));
          assertTrue(service.findById(card.getId()).isPresent());
        }
      }
    ```

  >>>


  >>> - **Boas práticas:**

  - Utilize bancos em memória (H2) ou contêineres Docker (Testcontainers).

  - Isolar contexto de dados (limpar tabelas entre testes).

    🔺 **End-to-End Tests** (10%)
    - Simulam o uso real pela API inteira (MockMvc, Selenium).


    ```java
      mockMvc.perform(post("/cashcards")
            .contentType(APPLICATION_JSON)
            .content("{\"amount\":100.0}"))
            .andExpect(status().isCreated())
            .andExpect(header().exists("Location"));
    ```

  >>>


  >>> - **Boas práticas:**

    - Limite o número de E2E para cenários críticos.

    - Coordene ambientes estáveis (testes em ambiente isolado).


## 🔴🟢🔄 5. Loop Red–Green–Refactor
**Red** – escreva um teste que falha declarando o comportamento desejado.

**Green** – implemente o mínimo de código para passar no teste.

**Refactor** – limpe duplicações e melhore o design sem alterar o comportamento.


  - **Exemplo lúdico:** “É como plantar uma semente (test), regá-la até germinar (código mínimo) e depois podar os galhos secos (refactor) para florestar o jardim.”

  >>> - **Exemplo prático & boas práticas:**

  - Nomeie testes de forma descritiva (deveEnviarNotFoundParaIdInexistente).

  - Quebre refactors em commits separados, garantindo build verde a cada passo.

  - Use IDEs com suporte a TDD (IntelliJ/VSCode) para atalho “run test” ao salvar.

  >>>


## 🎯 Resumo
  -  TDD alinha especificação e implementação desde o início.

  -  A Pirâmide de Testes equilibra velocidade e segurança.

    O Red–Green–Refactor garante evolução contínua sem dívidas técnicas.

Agora, comece a escrever seus testes antes do código e experimente a robustez de um fluxo Test-First!