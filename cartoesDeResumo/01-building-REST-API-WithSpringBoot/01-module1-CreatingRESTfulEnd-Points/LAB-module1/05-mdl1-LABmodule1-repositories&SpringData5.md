5: Injetar o CashCardRepository  
Embora já tenhamos configurado as classes `CashCard` e `CashCardRepository`, ainda não usamos o `CashCardRepository` para gerenciar os dados de `CashCard`. Vamos fazer isso agora.

**Injete o CashCardRepository no CashCardController.**

Edite o `CashCardController` para receber um `CashCardRepository` via construtor:

```java
@RestController
@RequestMapping("/cashcards")
class CashCardController {
   private final CashCardRepository cashCardRepository;

   private CashCardController(CashCardRepository cashCardRepository) {
      this.cashCardRepository = cashCardRepository;
   }
   ...
}
```

Execute os testes. Se você rodar os testes agora, todos passarão, mesmo sem outras mudanças no código, graças ao novo construtor CashCardController(CashCardRepository cashCardRepository).

BUILD SUCCESSFUL in 7s
Como isso é possível? Presencie o poder da Auto Configuração e da Injeção por Construtor! A Auto Configuração do Spring usa seu framework de injeção de dependência (DI), especificamente injeção por construtor, para fornecer ao CashCardController a implementação correta de CashCardRepository em tempo de execução. Mágica!

Momento de aprendizado: Remova a DI
Acabamos de testemunhar a glória da auto configuração e da injeção por construtor. Mas o que acontece se desativarmos esse recurso?

Altere temporariamente o CashCardRepository para remover a extensão de CrudRepository:

java
interface CashCardRepository {
}
Compile o projeto e observe a falha:

[~/exercises] $ ./gradlew build
...
org.springframework.beans.factory.NoSuchBeanDefinitionException: No qualifying bean of type 'example.cashcard.CashCardRepository' available: expected at least 1 bean which qualifies as autowire candidate. Dependency annotations: {}
Mensagens como NoSuchBeanDefinitionException, “No qualifying bean” e “expected at least 1 bean which qualifies as autowire candidate” indicam que o Spring tentou injetar uma dependência, mas não encontrou nenhum bean qualificado.

Satisfaça o requisito de DI restaurando a extensão de CrudRepository:

java
interface CashCardRepository extends CrudRepository<CashCard, Long> {
}
Não se esqueça de desfazer as alterações temporárias em CashCardRepository antes de continuar.