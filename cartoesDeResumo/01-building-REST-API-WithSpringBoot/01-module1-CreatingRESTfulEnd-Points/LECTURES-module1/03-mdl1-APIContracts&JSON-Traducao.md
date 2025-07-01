# Contratos de API e JSON  
Estamos desenvolvendo uma API. Isso levanta várias questões sobre como a API deve se comportar:

  - Como os consumidores da API devem interagir com ela?  
  - Que dados os consumidores precisam enviar em diferentes cenários?  
  - Que dados a API deve retornar aos consumidores e em que situações?  
  - O que a API comunica quando é usada incorretamente (ou algo dá errado)?


Sempre que possível, provedores e consumidores de API devem discutir esses cenários e chegar a acordos.


  - Melhor ainda, esses acordos devem ser documentados não apenas em um sistema compartilhado, mas também de forma que suporte testes automatizados que passem (ou falhem) com base nessas decisões.



## Contratos de API  
A indústria de software adotou vários padrões para capturar o comportamento acordado de uma API em documentação e código. 

  - **Esses acordos são chamados de “contratos”**. Dois exemplos incluem:
  

    >>> 

    - **Consumer Driven Contracts**
    
    e 
    
    - **Provider Driven Contracts**.
  
  
    >>>


Forneceremos recursos sobre esses padrões, mas não os discutiremos em detalhes neste curso. Em vez disso, falaremos de um conceito mais leve chamado:

  >>> 

  - **contratos de API**.

  - Definimos um contrato de API como:
    
    - Um acordo formal entre um provedor de software e um consumidor que comunica, de forma abstrata, como interagir um com o outro. 
    
    - Esse contrato define como provedores e consumidores interagem, como ocorrem as trocas de dados e como comunicar casos de sucesso e falha.

  >>> 

O provedor e os consumidores não precisam usar a mesma linguagem de programação, apenas os mesmos contratos de API.


Para o domínio Family Cash Card, vamos supor que existe um contrato entre o serviço de Cash Card e todos os serviços que o utilizam.


Abaixo está um exemplo desse primeiro contrato de API:

  ```text
  Request
    URI: /cashcards/{id}
    HTTP Verb: GET
    Body: None
  
  Response:
    HTTP Status:
      200 OK se o usuário estiver autorizado e o Cash Card for recuperado com sucesso  
      401 UNAUTHORIZED se o usuário estiver não autenticado ou não autorizado  
      404 NOT FOUND se o usuário estiver autenticado e autorizado, mas o Cash Card não for encontrado  
    Tipo de Body de Resposta: JSON  
    Exemplo de Body de Resposta:
      {
        "id": 99,
        "amount": 123.45
      }
  ```

## Por que contratos de API são importantes?
Contratos de API comunicam o comportamento de uma API REST e detalham os dados serializados (ou desserializados) em cada troca.

  >>>

  - Eles são escritos de modo a serem facilmente traduzidos em funcionalidades de provedores e consumidores e em testes automatizados.


  - Nos laboratórios, você implementará tanto a lógica do provedor quanto testes que verifiquem esses contratos.

  >>>



## O que é JSON?
JSON (JavaScript Object Notation) é um formato de intercâmbio de dados que representa informações de um objeto de forma legível.

  - Usaremos JSON para trocar dados na API Family Cash Card.

  **Exemplo:**

  ```json
    {
      "id": 99,
      "amount": 123.45
    }
  ```


Outros formatos populares são YAML e XML.


  - Comparado ao XML, o JSON é mais rápido de ler/escrever, mais simples e ocupa menos espaço.
  
  - Funciona em quase todas as linguagens e plataformas, além de se integrar perfeitamente a aplicações JavaScript.
  
  - Por essas razões, JSON superou XML como o formato mais usado em APIs REST para aplicações web.



## Resumo
  Contratos de API são uma forma de provedores e consumidores concordarem sobre o comportamento de uma API.
  
  - Eles vão desde documentação compartilhada até frameworks sofisticados de validação de contratos. Aliados ao JSON, os contratos tornam o desenvolvimento e teste de APIs mais robustos.