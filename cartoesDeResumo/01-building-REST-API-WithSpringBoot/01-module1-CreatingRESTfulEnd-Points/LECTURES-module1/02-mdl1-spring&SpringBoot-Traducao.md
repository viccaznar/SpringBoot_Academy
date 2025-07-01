# Spring e Spring Boot  
Spring e Spring Boot são ambos frameworks Java usados para construir aplicações.


    - Pense neles como conjuntos de ferramentas que ajudam desenvolvedores a estruturar e organizar seu código de forma eficiente e escalável.  


  ## Spring  
  O Spring é um framework completo que oferece vários módulos para criar diferentes tipos de aplicações.


    - É como ter uma caixa de ferramentas gigante com cada ferramenta que você possa precisar para construir qualquer coisa. 


  Ao desenvolver nossa API Family Cash Card, usaremos Spring MVC para a parte web, Spring Data para acesso a dados e Spring Security para autenticação e autorização.  

  Essa versatilidade tem um custo.


    - Configurar uma aplicação Spring exige muita configuração manual, e os desenvolvedores precisam ajustar vários componentes do framework para colocar a aplicação em funcionamento.  


  ## Spring Boot  
  Felizmente, o Spring Boot simplifica bastante o trabalho com o Spring.
    - Ele é uma versão mais “opinativa” do Spring, que já traz muitas configurações e dependências pré-ajustadas, prontas para uso em aplicações comuns. 
    
    
    - Isso facilita o início rápido de um projeto, sem precisar configurar tudo do zero. Além disso, o Spring Boot inclui um servidor web embutido, permitindo criar e rodar aplicações web sem depender de um servidor externo.  


  Em resumo, o Spring é um framework poderoso e abrangente, que oferece muita flexibilidade, mas pode ser complexo de configurar. O Spring Boot é uma versão mais enxuta e orientada por convenções, com diversos recursos incorporados para acelerar o desenvolvimento.  


## Container de Inversão de Controle do Spring  
O Spring Boot aproveita o container de Inversão de Controle (IoC) do Spring Core. Você utilizará esse recurso com frequência ao desenvolver a aplicação Family Cash Card. Embora exista vasta documentação sobre o assunto, aqui manteremos a explicação simples.  


O Spring Boot permite definir como e quando as dependências serão fornecidas à sua aplicação em tempo de execução. Isso dá a você controle sobre o comportamento da aplicação em diferentes cenários.  


Por exemplo, você pode querer usar um banco de dados diferente em desenvolvimento local e outro em produção. Seu código não deve se preocupar com essa distinção — caso contrário, você teria que codificar cada cenário no próprio aplicativo. Em vez disso, o Spring Boot permite fornecer configurações externas que determinam quais dependências são usadas em cada situação.  


Inversão de controle é muitas vezes chamada de injeção de dependência (DI), embora não seja estritamente a mesma coisa. A injeção de dependência é uma forma de implementar IoC, e desenvolvedores Spring costumam dizer que as dependências são “injetadas” em tempo de execução. Fazemos essa distinção porque várias linguagens e frameworks adotam IoC sem usar o termo “injeção de dependência”. Contudo, na comunidade Spring, os termos são frequentemente intercambiáveis.  


Saiba mais sobre o container IoC do Spring na [documentação oficial do Spring](https://spring.io/projects/spring-framework).  


## Spring Initializr  
Ao começar uma nova aplicação Spring Boot, o Spring Initializr é o ponto de partida recomendado. Pense no Initializr como um carrinho de compras para todas as dependências que seu projeto precisará. Ele gera rápida e facilmente uma aplicação Spring Boot completa e pronta para rodar.  


No próximo laboratório, usaremos o Spring Initializr. O fluxo geral consiste em preencher metadados, adicionar dependências relevantes e gerar o novo projeto. Para a aplicação Family Cash Card, forneceremos todas as instruções necessárias.  


Nas próximas aulas, continuaremos adicionando funcionalidades à aplicação gerada pelo Initializr.  

---


## Resumo  
Agora que você entendeu o que são Spring e Spring Boot, vamos ao laboratório do Initializr para dar o pontapé inicial no nosso projeto Spring Boot com todas as dependências básicas.
