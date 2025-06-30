# 🗂️ Testando o Contrato de Dados – Guia Passo a Passo

Este fluxo mostra como definir e validar seu contrato JSON usando TDD: do teste inicial que falha até o build verde final.

---

## 1️⃣ Escrevendo o Teste de Contrato JSON  
**O que acontece:** Criamos um teste especializado em JSON que garante que nossa classe `CashCard` serializa exatamente conforme o contrato (`expected.json`).  

```java
@JsonTest
class CashCardJsonTest {
  @Autowired JacksonTester<CashCard> json;

  @Test
  void deveSerializarCorretamente() throws IOException {
    CashCard card = new CashCard(99L, 123.45);
    // verifica igualdade estrita com JSON de exemplo
    assertThat(json.write(card)).isStrictlyEqualToJson("expected.json");
    // assegura existência e valor correto de cada campo
    assertThat(json.write(card)).hasJsonPathNumberValue("@.id")
                                .satisfies(v -> assertThat(v).isEqualTo(99));
    assertThat(json.write(card)).hasJsonPathNumberValue("@.amount")
                                .satisfies(v -> assertThat(v).isEqualTo(123.45));
  }
}
```

🎨 Exemplo lúdico (criança): “É como pedir para um artista pintar um desenho copiando fielmente cada cor e detalhe de um rascunho — só aceitamos quando for igualzinho!”

⚙️ Exemplo prático e boas práticas:

Use @JsonTest + JacksonTester para isolar testes de serialização sem carregar toda a aplicação.

Organize seus arquivos de contrato em src/test/resources/example/cashcard/expected.json.

2️⃣ Anotações e Ferramentas de Teste
@JsonTest: inicializa apenas o contexto de JSON (sem servidor web).

JacksonTester<T>: wrapper de Jackson que simplifica assertivas de JSON.

@Autowired: injeta instâncias gerenciadas pelo Spring.

🎨 Exemplo lúdico: “Imagine um palco de teatro onde as luzes (anotações) acendem só na área de ensaio dos atores de uma cena específica.”

⚙️ Exemplo prático e boas práticas:

Configure spring.jackson no application-test.yml para parametrizar datas e formatos gerais.

Centralize configurações de ObjectMapper em @TestConfiguration.

3️⃣ Primeiro Run: Falha por Classe Ausente
Ao rodar:

bash
./gradlew test
Você verá:

cannot find symbol: class CashCard
🎨 Exemplo lúdico: “É como tentar entregar um presente antes de saber para quem ele é — não há destinatário!”

⚙️ Exemplo prático e boas práticas:

O build falha rápido quando faltam classes.

Mantenha o feedback de compilação visível em IDE/CI para corrigir dependências ausentes imediatamente.

4️⃣ Criando a Classe CashCard
No diretório src/main/java/example/cashcard, crie:

java
package example.cashcard;

record CashCard(Long id, Double amount) {}
🎨 Exemplo lúdico: “Sculpimos a estátua de um herói antes de iluminá-la no parque — agora temos a forma básica!”

⚙️ Exemplo prático e boas práticas:

Utilize record em Java 17+ para diminuir verbose de getters, equals e hashCode.

Documente cada campo com @Schema(description = "...") se usar OpenAPI.

5️⃣ Segundo Run: Falha por expected.json Ausente
Rerun do teste:

bash
./gradlew test
Erro:

Unable to load JSON from class path resource [example/cashcard/expected.json]
🎨 Exemplo lúdico: “É como ter o mapa do tesouro mas sem o ‘X’ que indica onde cavar!”

⚙️ Exemplo prático e boas práticas:

Sempre crie a estrutura de pastas em src/test/resources antes do teste.

Versione contratos JSON junto ao código para rastreabilidade e revisão.

6️⃣ Criando expected.json Vazio
Em src/test/resources/example/cashcard/expected.json, crie:

json
{}
🎨 Exemplo lúdico: “Desenha só o contorno de um quadrinho antes de colorir os personagens.”

⚙️ Exemplo prático e boas práticas:

Mantenha contratos JSON minimalistas no início e complemente conforme a necessidade.

Use validação de JSON Schema para garantir estrutura.

7️⃣ Terceiro Run: Falha por Campos Faltantes
Ao rodar:

bash
./gradlew test
A comparação JSON falha:

Unexpected: id
Unexpected: amount
🎨 Exemplo lúdico: “É como organizar uma festa de aniversários e esquecer de convidar o aniversariante!”

⚙️ Exemplo prático e boas práticas:

Leitura clara do erro ajuda a ajustar contratos com agilidade.

Mantenha logs de teste detalhados para acelerar diagnósticos.

8️⃣ Finalizando o Contrato de Dados
Altere expected.json para:

json
{
  "id": 99,
  "amount": 123.45
}
🎨 Exemplo lúdico: “Encaixe as últimas peças do quebra-cabeça e veja o desenho completo!”

⚙️ Exemplo prático e boas práticas:

Mantenha identação e ordenação de campos consistente para facilitar diffs.

Use prettier ou plugins de formatação JSON no CI.

9️⃣ Quarto Run: Sucesso
Rode de novo:

bash
./gradlew test
Saída:

BUILD SUCCESSFUL
🎨 Exemplo lúdico: “Aplauda como um campeão ao alcançar a medalha de ouro!”

⚙️ Exemplo prático e boas práticas:

Integre esses testes de contrato ao pipeline CI (GitHub Actions, Jenkins) para garantir build green.

Marque contratos como gatekeeper antes de merges em branches principais.

🎉 Resumo Final: Você definiu um contrato de dados JSON, escreveu testes que falham e evoluiu até o build verde. Esse processo assegura alinhamento entre código e especificação, servindo de base para contratos mais complexos e testes de integração futuros!