# Retornando uma lista com GET  

Agora que nossa API pode criar Cash Cards, faz sentido aprender como buscar todos (ou alguns!) os Cash Cards.

Nesta lição, vamos implementar o endpoint `“Read Many”` e entender como essa operação difere substancialmente do endpoint de leitura simples que criamos anteriormente.


## Passo 1. Solicitando uma lista de Cash Cards  

Podemos esperar que cada usuário do Family Cash Card tenha vários cartões: 

  - Imagine um para cada membro da família e talvez alguns para presentes.
  
  - A API deve conseguir retornar múltiplos Cash Cards em resposta a uma única requisição REST.
  
  - Quando você faz uma requisição para vários Cash Cards, idealmente faz apenas uma chamada que retorna uma lista de Cash Cards.
  
  - Então precisamos de um novo contrato de dados. Em vez de um único Cash Card, o novo contrato deve especificar que a resposta é um Array JSON de objetos Cash Card:


  ```json
  [
    {
      "id": 1,
      "amount": 123.45
    },
    {
      "id": 2,
      "amount": 50.0
    }
  ]
  ```


Felizmente, nosso velho amigo `CrudRepository` tem um método `findAll()` que podemos usar para buscar facilmente todos os Cash Cards no banco de dados. À primeira vista, parece bastante simples:


  ```java
  @GetMapping
  private ResponseEntity<Iterable<CashCard>> findAll() {
    return ResponseEntity.ok(cashCardRepository.findAll());
  }
  ```


No entanto, há muito mais nessa operação do que simplesmente retornar todos os Cash Cards do banco. 


**Surgem algumas perguntas:**

  >>>

  - Como retornar apenas os Cash Cards que o usuário possui? (Ótima pergunta! Discutiremos isso na lição de Spring Security.)

  - E se houver centenas (ou milhares?!) de Cash Cards? A API deve retornar um número ilimitado de resultados ou retorná-los em “pedaços”? Isso é conhecido como paginação.


  - Os Cash Cards devem ser retornados em uma ordem específica (isto é, devem ser ordenados)?

  >>>

Vamos deixar a primeira pergunta para depois, mas responder as questões de paginação e ordenação nesta lição. Vamos em frente!



## Passo 2. Paginação e Ordenação

Para começar com paginação e ordenação, usaremos uma versão especializada do `CrudRepository`, chamada `PagingAndSortingRepository`. Como você deve imaginar, ele faz exatamente o que o nome sugere. 


Mas primeiro, vamos falar sobre a funcionalidade de “Paging” (paginação).


  - Mesmo que seja improvável que usuários tenham milhares de Cash Cards, nunca sabemos como o produto será usado.
  


  >>>


  - **Idealmente:**
  
  - Uma API não deve gerar uma resposta de tamanho ilimitado, pois isso pode sobrecarregar a memória do cliente ou do servidor, sem contar o tempo de resposta!


  >>>



Para garantir que uma resposta da API não inclua um número astronômico de Cash Cards, vamos usar a paginação do Spring Data. 


**`Paginação em Spring` (e em muitos outros frameworks):** 


  - Consiste em especificar o tamanho da página (por exemplo, 10 itens) e o índice da página (começando em 0). 


  - Por exemplo, se um usuário tiver 25 Cash Cards e você solicitar a segunda página com 10 itens por página, fará uma requisição com page size = 10 e page index = 1.


Legal, né? Mas isso levanta outro desafio. Para que a paginação produza o conteúdo correto, os itens devem estar em uma ordem específica. Por quê? Imagine que temos Cash Cards com os seguintes saldos:


  - $0.19 (quase zerado)

  - $1.000,00 (fundo de emergência de um estudante universitário)

  - $50,00

  - $20,00

  - $10,00 (presente de aniversário para sua sobrinha)


Agora, vamos usar um tamanho de página igual a 3. Como há 5 Cash Cards, faremos duas requisições para retornar todos. 


  >>>

  - A página 1 (índice 0) contém três itens,

  - A página 2 (índice 1, a última) contém dois. 

  >>>


Mas quais itens ficam em cada página? Se especificarmos que deve ser ordenado por saldo em ordem decrescente, a paginação fica assim:


- **Página 1:**

  $1.000,00

  $50,00

  $20,00


- **Página 2:**

  $10,00

  $0.19

## Passo 3. Sobre Consultas Sem Ordenação
Embora o Spring permita uma estratégia de ordenação “não ordenada”, vamos ser explícitos ao escolher os campos para ordenação. 


Por quê? Imagine que você use paginação “não ordenada”. 

  
  - Na prática, a ordem não é aleatória, mas previsível; 
  
  
  - ela nunca muda em requisições subsequentes. 
  
  
  - Suponha que você faça uma chamada e receba estes resultados “não ordenados”:


   - **Página 1:**

    $0.19

    $1.000,00

    $50,00

   - **Página 2:**

    $20,00

    $10,00


Apesar de parecer aleatório, toda vez que você fizer a mesma requisição os cartões virão nessa mesma ordem, garantindo que cada item apareça em exatamente uma página.

Agora a pergunta: se você criar um novo Cash Card com saldo de $42,00, em qual página ele cairá? Como você deve imaginar, só há um jeito de descobrir: refazer a requisição e ver onde o novo Cash Card apareceu.

Para tornar isso mais útil, vamos ordenar por um campo específico. Há boas razões para isso, como:


  >>>

  - **Reduzir a sobrecarga cognitiva:** outros desenvolvedores (e usuários) apreciarão uma ordenação definida.


  - **Evitar erros futuros:** e se uma nova versão do Spring, do Java ou do banco de dados alterar a “ordem aleatória” sem aviso?

  >>>


### API de Paginação do Spring Data
Felizmente, o Spring Data fornece as classes `PageRequest` e `Sort` para paginação. 

Veja um exemplo para obter a página 2 com tamanho 10, ordenando por amount em ordem decrescente (maiores primeiros):


```java
Page<CashCard> page2 = cashCardRepository.findAll(
    PageRequest.of(
        1,  // índice da segunda página (inicia em 0)
        10, // tamanho da página (a última pode ter menos itens)
        Sort.by(new Sort.Order(Sort.Direction.DESC, "amount"))
    )
);
```


### A Requisição e a Resposta

Agora, vamos usar o Spring Web para extrair os parâmetros e alimentar a paginação:


  - **Paginação:** o Spring pode interpretar os parâmetros page e size quando você recebe um objeto Pageable em um método de um `PagingAndSortingRepository`.


  - **Ordenação:** o Spring interpreta o parâmetro sort, que deve vir como campo, direção (sem espaços). Esses dados também fazem parte do `Pageable`.


### A URI

Veja como compor a URI do novo endpoint, passo a passo (omitimos o prefixo https://domínio nos exemplos):


 - **Segunda página:**


  ```bash
  /cashcards?page=1
  Com tamanho de página = 3:

  /cashcards?page=1&size=3
  Ordenado pelo saldo:

  /cashcards?page=1&size=3&sort=amount
  Ordem decrescente:

  /cashcards?page=1&size=3&sort=amount,desc
  ```


### O Código Java

Veja a implementação completa do método no Controller para o novo endpoint “buscar página de Cash Cards”:


  ```java
  @GetMapping
  private ResponseEntity<List<CashCard>> findAll(Pageable pageable) {
    Page<CashCard> page = cashCardRepository.findAll(
            PageRequest.of(
                    pageable.getPageNumber(),
                    pageable.getPageSize(),
                    pageable.getSortOr(Sort.by(Sort.Direction.DESC, "amount")))
    );
    return ResponseEntity.ok(page.getContent());
  }
  ```


### Detalhes:

- **Pageable:** permite ao Spring extrair page e size da query string.
Se não fornecidos, os defaults são `page=0` e `size=20`.


- **getSortOr():** garante uma ordenação padrão caso o cliente não passe o parâmetro sort. Diferente de page e size, escolher um campo de ordenação arbitrário não faz sentido.


- **page.getContent():** retorna apenas a lista de Cash Cards no corpo da resposta.


### O objeto Page

O Page contém, além dos Cash Cards, informações sobre a página no contexto da consulta. Em JSON ficaria assim:

  ```json
  {
    "content": [
      { "id": 1, "amount": 10.0 },
      { "id": 2, "amount": 0.19 }
    ],
    "pageable": {
      "sort": { "empty": false, "sorted": true, "unsorted": false },
      "offset": 3,
      "pageNumber": 1,
      "pageSize": 3,
      "paged": true,
      "unpaged": false
    },
    "last": true,
    "totalElements": 5,
    "totalPages": 2,
    "first": false,
    "size": 3,
    "number": 1,
    "sort": { "empty": false, "sorted": true, "unsorted": false },
    "numberOfElements": 2,
    "empty": false
  }
  ```


Embora pudéssemos retornar o objeto Page inteiro, não precisamos de toda essa informação. Nosso contrato de dados definirá que apenas os Cash Cards serão retornados.


## Resumo

Instrutores Com Spring Boot e Spring Data é tão fácil recuperar múltiplos itens quanto buscar um único. 

Além disso, os poderosos e fáceis recursos de paginação, ordenação e filtragem do Spring Data permitem que desenvolvedores incorporem rapidamente essas funcionalidades em uma aplicação.

Falando nisso — vamos implementar listas, paginação e ordenação na nossa própria API Family Cash Card!