# 3: Adicionar Dependências do Spring Data  

Este projeto foi originalmente criado usando o `Spring Initializr`, que nos permitiu adicionar dependências automaticamente ao projeto. No entanto, agora devemos adicioná-las manualmente.

## Passo 1. Adicione dependências para o Spring Data e um banco de dados.

Em `build.gradle`:

   ```groovy
   dependencies {
      implementation 'org.springframework.boot:spring-boot-starter-web'
      testImplementation 'org.springframework.boot:spring-boot-starter-test'

      // Adicione as duas dependências abaixo
      implementation 'org.springframework.data:spring-data-jdbc'
      implementation 'com.h2database:h2'
   }
   ```


## Passo 2. Entendendo as dependências.

### As duas dependências que adicionamos estão relacionadas, mas são diferentes.


- **implementation 'org.springframework.data:spring-data-jdbc':**


   >>>

   - O Spring Data possui várias implementações para diversas tecnologias de banco de dados relacionais e não relacionais. 


   - Ele também oferece abstrações sobre essas tecnologias, comumente chamadas de `Object-Relational Mapping (ORM)`. 


   - Aqui vamos optar pelo `Spring Data JDBC`. Da documentação do Spring Data JDBC: 

   >>>


   - > “Spring Data JDBC aims at being conceptually easy... > This makes Spring Data JDBC a simple, limited, opinionated ORM.”



- **implementation 'com.h2database:h2'**


   >>>

  -  Frameworks de gerenciamento de banco de dados só funcionam se tiverem um banco vinculado. 
  
  
  -  H2 é um banco SQL “muito rápido, open source, com API JDBC” implementado em Java. 
  
  
  -  Ele funciona perfeitamente com o `Spring Data JDBC`.

   >>>



   >>>



   - **Execute os testes**. Isso instalará as dependências e verificará se sua adição não quebrou nada. 
   

  - Sempre usaremos `./gradlew test` para executar nossos testes.


   ```bash
   [~/exercises] $ ./gradlew test
    ...
   BUILD SUCCESSFUL in 4s
   ```


As dependências agora estão instaladas! Você pode notar uma saída adicional em comparação com laboratórios anteriores, como “Shutting down embedded database”. 

  - A Auto Configuração do Spring agora está iniciando e configurando um banco H2 para usarmos nos testes. Ótimo!