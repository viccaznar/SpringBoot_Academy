## 2: Revisão do Padrão Atual de Gerenciamento de Dados

Nossa API REST Family Cash Card atualmente depende de dados de `CashCard` codificados diretamente em nosso `CashCardController`. Nossos testes em `CashCardApplicationTests` assumem que esses dados estão presentes.


  >>>


  - Sabemos que um Controller web não deveria gerenciar dados. 
   
  - Isso viola o princípio da Separação de Responsabilidades (Separation of Concerns). 
   
  - Tráfego web é tráfego web, dados são dados, e um software saudável possui arquiteturas dedicadas a cada área.


  >>>


### Passo 1. Revisite o `CashCardController`.**

Observe linhas como esta:


  ```java
  ...
  if (requestedId.equals(99L)) {
     CashCard cashCard = new CashCard(99L, 123.45);
     return ResponseEntity.ok(cashCard);
  ...
  ```


  - **Isso é gerenciamento de dados.** 
  

    >>>

    - Nosso Controller não deveria se preocupar em checar IDs ou criar dados.

    >>>



### Passo 2. Revisite o `CashCardApplicationTests`.

Curiosamente, embora nossos testes façam asserções sobre os dados, eles não dependem nem especificam como esses dados são criados ou gerenciados.


  - Esse desacoplamento é importante, pois nos permite fazer as mudanças necessárias.


  - Prepare-se para Refatorar usando um Repositório e um Banco de Dados


**Refatoração** 
  
  >>>

  - é o ato de alterar a implementação de um sistema de software sem modificar suas entradas, saídas ou comportamento.

  >>>


Nossos testes permitirão que mudemos a implementação do gerenciamento de dados da nossa API de Cash Card — de dados codificados diretamente no Controller — para a utilização de um Repositório conectado a um banco de dados.

  >>>


  - Este laboratório é um exemplo contínuo do ciclo de desenvolvimento Red, Green, Refactor que aprendemos em uma lição anterior.


  - À medida que refatoramos, nossos testes falharão periodicamente. Saberemos que removemos com sucesso todos os dados codificados do Controller e migramos esse gerenciamento de dados para um repositório persistido em banco de dados quando nossos testes voltarem a passar.


  >>>