# 🎯 Testando a Desserialização

**Tradução**  
Desserialização é o processo inverso da serialização. Ela transforma dados de um arquivo ou fluxo de bytes de volta em um objeto da sua aplicação. Assim, um objeto serializado em uma plataforma pode ser desserializado em outra (por exemplo, cliente no Windows e backend no Linux).

  Serialização e desserialização trabalham juntas para converter ou recriar objetos de dados de/para um formato portátil — o mais popular é JSON.

  Vamos agora criar um segundo teste que desserializa JSON em um objeto Java, usando a abordagem Test-First. Primeiro, adicionamos este método em `src/test/java/example/cashcard/CashCardJsonTest.java`:

  ```java
  @Test
  void cashCardDeserializationTest() throws IOException {
    String expected = """
            {
                "id":99,
                "amount":123.45
            }
            """;
    assertThat(json.parse(expected))
            .isEqualTo(new CashCard(1000L, 67.89));
    assertThat(json.parseObject(expected).id()).isEqualTo(1000);
    assertThat(json.parseObject(expected).amount()).isEqualTo(67.89);
  }
  ```

  Execute o teste com:

  ```bash
    ./gradlew test
  ```

  Você verá que ele falha, pois espera id=1000, amount=67.89, mas obtém id=99, amount=123.45.

  Corrija asserções trocando para os valores corretos:

  ```java
    assertThat(json.parse(expected))
            .isEqualTo(new CashCard(99L, 123.45));
    assertThat(json.parseObject(expected).id()).isEqualTo(99);
    assertThat(json.parseObject(expected).amount()).isEqualTo(123.45);
  ```
  
  Re-run:

  ```bash
    ./gradlew test
  ```
  
  Agora o teste passa!

  Pronto — você tem um par de testes de serialização e desserialização funcionando.

# 📑 Resumo em Tópicos

## 🌀 1. O que é Desserialização?
Inverte a serialização: converte bytes/JSON de volta em objeto Java.

 - **Exemplo lúdico:** como desmontar um modelo de LEGO para guardar as peças.

 - **Exemplo prático:** json.parse(expectedJson) usando JacksonTester.



## 🔄 2. Serialização ↔ Desserialização
Trabalham juntas para portar dados entre plataformas.

 - **Exemplo lúdico:** enviar cartas em envelopes (JSON) e abri-las no destino.

 - **Exemplo prático:** json.write(obj) e json.parse(string) no mesmo teste.



## 💡 3. Escrevendo o Teste de Desserialização
Test-First: criar teste que falha para guiar a implementação.

 - **Exemplo lúdico:** adivinhar a senha errada de propósito para ter certeza que o sistema bloqueia.

 - **Exemplo prático:** método cashCardDeserializationTest() em CashCardJsonTest.java.



## ▶️ 4. Executando e Corrigindo o Teste
Rode ./gradlew test, observe falha e mensagem de comparação.

Ajuste os valores esperados (id e amount) no teste.

 - **Exemplo lúdico:** afinar o alvo até acertar o centro do alvo.

 - **Exemplo prático:** editar asserções e re-executar até BUILD SUCCESSFUL.




## 🏁 5. Par de Testes Concluídos
Agora você tem testes de serialização e desserialização garantindo o contrato JSON.

 - **Exemplo lúdico:** ensaiar as falas de um diálogo dos dois lados para não errar na apresentação.

 - **Exemplo prático:** pipeline CI passa nos testes de JSON antes de avançar no desenvolvimento.