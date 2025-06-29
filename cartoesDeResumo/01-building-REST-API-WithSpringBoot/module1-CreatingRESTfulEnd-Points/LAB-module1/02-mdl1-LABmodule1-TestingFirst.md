# 📝 Escrevendo um Teste Que Falha – Guia Estruturado

Este fluxo orientado a testes (TDD) mostra como criar um teste que falha (Red), corrigi-lo (Green) e preparar-se para refatorar (Refactor).

---

## 🚀 1. Introdução ao TDD, JUnit e Gradle  
**Descrição:** Você vai usar JUnit para escrever testes e o Gradle para compilar/rodar tudo, adotando a abordagem Test-First (TDD).  
- Exemplo lúdico (criança): “Antes de montar um castelo de cartas, você desenha cada torre no papel para ter certeza de que não vai desabar.”  
- Exemplo prático & boas práticas:  
  - No `build.gradle`, adicione:
    ```groovy
    dependencies {
      testImplementation platform('org.junit:junit-bom:5.9.2')
      testImplementation 'org.junit.jupiter:junit-jupiter'
      testImplementation 'org.assertj:assertj-core:3.24.2'
    }
    test {
      useJUnitPlatform()
    }
    ```
  - Use BOM para manter versões consistentes e `useJUnitPlatform()` para ativar JUnit 5.

---

## 🗂️ 2. Estrutura de Pastas de Teste  
**Descrição:** Testes devem ficar em `src/test/java`, espelhando o mesmo pacote da aplicação.  
- Exemplo lúdico: “É como guardar carrinhos vermelhos na prateleira vermelha e carros azuis na azul — cada cor no seu lugar.”  
- Exemplo prático & boas práticas:  
  
  ```
  src/ ├── main/java/example/cashcard/… └── test/java/example/cashcard/…
  ```

  - Mantenha o mesmo `package` nos testes para acesso direto a classes de domínio.

---

## 🖊️ 3. Criando a Classe de Teste  
**Descrição:** Crie `CashCardJsonTest.java` em `src/test/java/example/cashcard`.  
- Exemplo lúdico: “Escrever um bilhete de pedido à Fada do Dente antes de ela visitar sua casa.”  
- Exemplo prático & boas práticas:  
- Use o IDE para gerar a classe e o método de teste com `@Test`.  
- Nomeie a classe com sufixo `Test` e o método de forma descritiva:
  ```java
  package example.cashcard;

  import org.junit.jupiter.api.Test;
  import static org.assertj.core.api.Assertions.assertThat;

  class CashCardJsonTest {

      @Test
      void deveFalharQuandoValorDiferente() {
          assertThat(1).isEqualTo(42);
      }
  }
  ```
- Prefira nomes em português/inglês claros (`deveFalharQuando…`).

---

## 🔍 4. Por Dentro do Teste – @Test e AssertJ  
**Descrição:**  
- `@Test`: JUnit identifica o método de teste.  
- `assertThat()`: AssertJ gera mensagens de erro legíveis e fluentes.  
- Convenção: métodos de teste em estilo BDD (`deve…Quando…`).  
- Exemplo lúdico: “Batizar seu ursinho de ‘Testinho’ para lembrar que ele só faz testes.”  
- Exemplo prático & boas práticas:  
- Use `assertThatThrownBy()` para cenários de exceção.  
- Aplique Static Imports para manter o código limpo.

---

## ▶️ 5. Executando Testes – Red (Falha Intencional)  
**Descrição:** Rode `./gradlew test` e veja seu teste falhar, confirmando que ele realmente verifica algo.  
- Exemplo lúdico: “O alarme toca porque a porta está aberta – exatamente o que combinamos no aviso.”  
- Exemplo prático & boas práticas:
```bash
./gradlew test


Saída esperada:

CashCardJsonTest > deveFalharQuandoValorDiferente() FAILED
  org.opentest4j.AssertionFailedError:
  expected: 42
   but was: 1
🛠️ 6. Corrigindo o Teste – Green (Fazendo Passar)
Descrição: Ajuste a asserção para corresponder ao comportamento desejado.

Exemplo lúdico: “Trocar a peça errada do quebra-cabeça por aquela que realmente encaixa.”

Exemplo prático & boas práticas:

java
@Test
void deveFalharQuandoValorDiferente() {
    assertThat(42).isEqualTo(42);
}
bash
./gradlew test
# CashCardJsonTest > deveFalharQuandoValorDiferente() PASSED
# BUILD SUCCESSFUL
Faça um commit claro: fix(test): corrige valor esperado no teste.

🎯 7. Comandos e Saída Esperada
Rodar todos os testes:

bash
./gradlew clean test
Ignorar testes (apenas build):

bash
./gradlew build -x test
Saída CI/CD:

BUILD SUCCESSFUL in 4s
5 actionable tasks: 5 executed
✨ 8. Próximos Passos – Refactor e Evolução
Descrição: Agora que o ciclo Red–Green está fechado, é hora de refatorar e melhorar o design sem alterar testes.

Refactor: extraia métodos, renomeie variáveis e aplique princípios SOLID.

Evolução: escreva novos testes para validar contratos JSON, integração com banco e endpoints REST.

Exemplo lúdico: “Podar o jardim após as primeiras flores desabrocharem, deixando a paisagem mais organizada.”

Exemplo prático & boas práticas:

Use IDE para renomear símbolos de forma segura.

Meça cobertura de testes (JaCoCo) e mantenha acima de 80%.

Integre TDD ao fluxo de trabalho no Git (pre-commit hooks).

🎉 Resumo: Você aprendeu a criar um teste que falha propositalmente, corrigi-lo e estruturar seu projeto para evoluir em TDD. Agora siga para testar seu contrato de dados e implementar os primeiros endpoints da API!