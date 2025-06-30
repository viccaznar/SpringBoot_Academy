# Testando Primeiro

## O que é Desenvolvimento Orientado a Testes?  
É comum equipes de desenvolvimento criarem suítes de testes automatizados para evitar regressões.

  >>> 

  Normalmente esses testes são escritos após o código da funcionalidade estar pronto. Vamos usar uma abordagem alternativa: 

    - Escreveremos testes antes de implementar o código da aplicação.
    
    - Isso se chama **Desenvolvimento Orientado a Testes (TDD**).

  >>>

## Por que aplicar TDD?
Ao afirmar o comportamento esperado antes de implementar a funcionalidade desejada, projetamos o sistema com base no que queremos que ele faça, e não apenas no que ele já faz.

  >>>

  - Outro benefício de “testar primeiro” é que os testes orientam você a escrever o mínimo de código necessário para satisfazer a implementação.
  
  
  - Quando os testes passam, você tem tanto uma implementação funcional (o código da aplicação) quanto uma proteção contra introduzir erros no futuro (os próprios testes).

  >>>


## A Pirâmide de Testes  
Diferentes testes podem ser escritos em diferentes níveis do sistema. Em cada nível, há um equilíbrio entre a velocidade de execução, o “custo” de manutenção do teste e a confiança na correção do sistema.

Essa hierarquia é representada como uma “pirâmide de testes”:

  >>>

  - **Testes de Unidade**: Exercitam uma pequena “unidade” do sistema isolada do restante.


    -  Devem ser simples e rápidos. 
    
    -  Quanto maior a proporção de testes de unidade na pirâmide, melhor — eles são a base para software altamente coeso e fracamente acoplado.
  

  - **Testes de Integração**: Exercitam um subconjunto maior do sistema, podendo agrupar várias unidades em um mesmo teste.


    - São mais complexos de escrever e manter, e executam mais lentamente que testes de unidade.  


  - **Testes de Ponta a Ponta**: Exercitam o sistema usando a mesma interface de um usuário, como um navegador web.


  - Apesar de extremamente completos, podem ser lentos e frágeis por simular interações em UIs complexas.
  
  - Use o mínimo necessário desses testes.

  >>>



## O Ciclo Red, Green, Refactor  
Equipes ágeis buscam desenvolver rapidamente. Como manter alta velocidade para sempre? Melhorando e simplificando o código continuamente — refatorando.

  - Mas a única forma segura de refatorar é ter uma suíte de testes confiável. Por isso o melhor momento para refatorar o código em foco é durante o ciclo TDD, também chamado de ciclo **Red, Green, Refactor**:

  >>>
  1. **Red**: escreva um teste que falhe para a funcionalidade desejada.  

  2. **Green**: implemente a solução mais simples capaz de fazer o teste passar.  

  3. **Refactor**: encontre oportunidades para simplificar, reduzir duplicações ou melhorar o código sem alterar seu comportamento.
  >>>

  4. Repita!

---

## Resumo  
O Desenvolvimento Orientado a Testes (TDD) é uma técnica comprovada para ajudar desenvolvedores a projetar software simples e robusto, além de prevenir regressões e bugs.
  
Em seguida, você implementará testes de unidade usando TDD para os contratos JSON que usaremos no desenvolvimento da API Family Cash Card. Viu? Nós avisamos que faríamos!
