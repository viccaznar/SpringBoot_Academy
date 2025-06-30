6: Use o CashCardRepository para Gerenciamento de Dados  
Você finalmente está pronto para usar o `CashCardRepository`!

### Encontre o CashCard usando `findById`  
A interface `CrudRepository` fornece diversos métodos úteis, incluindo `findById(ID id)`.  
Atualize o `CashCardController` para usar esse método do `CashCardRepository` e ajuste a lógica; não esqueça de importar `java.util.Optional`:

```java
import java.util.Optional;
...
@GetMapping("/{requestedId}")
private ResponseEntity<CashCard> findById(@PathVariable Long requestedId) {
    Optional<CashCard> cashCardOptional = cashCardRepository.findById(requestedId);
    if (cashCardOptional.isPresent()) {
        return ResponseEntity.ok(cashCardOptional.get());
    } else {
        return ResponseEntity.notFound().build();
    }
}
```

Entenda as mudanças
Optional<CashCard> cashCardOptional = cashCardRepository.findById(requestedId); Chamamos CrudRepository.findById, que retorna um Optional. Esse objeto pode ou não conter o CashCard buscado.

cashCardOptional.isPresent() / cashCardOptional.get() Usamos isPresent() para verificar se o repositório encontrou o CashCard. Se sim, recuperamos o objeto com get(), caso contrário retornamos 404 NOT_FOUND.

Execute os testes
Ao rodar os testes, vemos uma falha com o status 500 INTERNAL_SERVER_ERROR:

CashCardApplicationTests > shouldReturnACashCardWhenDataIsSaved() FAILED
   org.opentest4j.AssertionFailedError:
   expected: 200 OK
     but was: 500 INTERNAL_SERVER_ERROR
Isso significa que a API de Cash Card “quebrou”.

Precisamos de mais detalhes...
Vamos configurar o build.gradle para exibir todos os logs de saída dos testes, ajustando showStandardStreams para true:

groovy
test {
  testLogging {
      events "passed", "skipped", "failed"
      showExceptions true
      exceptionFormat "full"
      showCauses true
      showStackTraces true

      // Alterado de false para true:
      showStandardStreams = true
  }
}
Rerun dos testes:

A saída agora é muito mais detalhada. Buscando pelo erro, encontramos:

org.h2.jdbc.JdbcSQLSyntaxErrorException: Table "CASH_CARD" not found (this database is empty); SQL statement:
 SELECT "CASH_CARD"."ID" AS "ID", "CASH_CARD"."AMOUNT" AS "AMOUNT" FROM "CASH_CARD" WHERE "CASH_CARD"."ID" = ? [42104-214]
A causa das falhas é clara: a tabela "CASH_CARD" não existe — nosso banco está vazio e sem dados.