# 🛠️ Implementando PUT – Tradução Completa

## Introdução

Até agora, conseguimos criar e recuperar cartões de mesada (Cash Cards). O próximo passo lógico é permitir que o saldo de um cartão seja ajustado! Implementar a operação de atualização (Update) em uma API RESTful é um tema bastante debatido — e é isso que vamos abordar nesta lição.

Quando falamos em “ajustar o saldo” de um Cash Card, estamos nos referindo a atualizar o valor (`amount`) de um registro existente no banco de dados. **Isso envolve:**


  >>>

  - Criar um novo endpoint para receber uma requisição HTTP com verbo, URI e corpo.
  

  - Retornar uma resposta apropriada para os casos de sucesso e erro.

  >>>


Já conhecemos o verbo HTTP `POST`, que usamos para o endpoint de criação. Agora vamos falar sobre PUT e PATCH, e como esses três verbos se relacionam.


## PUT e PATCH 

Ambos `PUT` e `PATCH` podem ser usados para atualizar, mas funcionam de maneiras diferentes:

  >>>

  - **PUT** significa “criar ou substituir o registro completo”.
  

  - **PATCH** significa “atualizar apenas alguns campos do registro existente” — ou seja, uma atualização parcial.

  >>>


  - **Por que usar uma atualização parcial?**  


    Atualizações parciais liberam o cliente da necessidade de carregar o registro inteiro e reenviá-lo ao servidor. 


    Se o registro for grande, isso pode impactar significativamente a performance.


Para nossa aplicação, decidimos **não implementar atualização parcial**.


## PUT e POST

Há algo que não mencionamos quando criamos o endpoint de criação: 


  >>>

  - O padrão HTTP **não especifica** se o verbo `POST` ou `PUT` é preferido para criar um recurso! 


  - Isso é relevante porque usaremos `PUT` para o endpoint de atualização, e precisamos decidir se nossa API permitirá que `PUT` seja usado também para criar recursos.

  >>>


Existem diferentes formas de enxergar a relação entre as operações de criação e atualização e como elas são implementadas em REST usando verbos HTTP. A seguir, discutiremos três pontos de vista diferentes e, depois, resumiremos tudo em uma tabela.

O mais importante aqui não é memorizar todos os detalhes, mas entender que há várias escolhas possíveis — e que nós, como autores da API Cash Card, estamos tomando decisões conscientes sobre como implementar REST.


## Chaves Naturais e Substitutas 

Por que usar `PUT` para criar um recurso? Isso está relacionado à definição dos verbos HTTP. A diferença é sutil. Vamos explicar comparando dois sistemas:

  - Nossa API Cash Card
  - Uma API fictícia chamada Invoice API

A Invoice API aceita o número da fatura como identificador único. Isso é um exemplo de uso de **chave natural** (fornecida pelo cliente) em vez de **chave substituta** (gerada pelo servidor, como fazemos na Cash Card API).

A diferença importante é se a URI (que inclui o ID do recurso) precisa ser gerada pelo servidor ou não.


  - Se você precisa que o servidor retorne a URI do recurso criado, use **POST**. 


    **Exemplo:** `POST /cashcards` → servidor gera `/cashcards/101`.


  - Se a URI do recurso já é conhecida no momento da criação, use **PUT**.  


    **Exemplo:** `PUT /invoice/1234-567` → o mesmo URI será usado para leitura: `GET /invoice/1234-567`.


## Recursos e Sub-Recursos  

Outra forma de entender a diferença é em termos de URIs e coleções de sub-recursos, como descrito na documentação HTTP:


  - **POST** cria um sub-recurso dentro da URI da requisição.  
  Exemplo: `POST /cashcards` → cria `/cashcards/101`.


  - **PUT** cria ou substitui um recurso na URI exata da requisição.  
  Exemplo: `PUT /invoice/1234-567` → cria ou substitui esse recurso.


## Corpo da Resposta e Código de Status  

Ao decidir se `PUT` pode criar objetos, também é necessário decidir qual será o código de status e o corpo da resposta.
Existem duas opções:


  1. Retornar **201 CREATED** (se criou o objeto) ou **200 OK** (se substituiu um existente).  
    Neste caso, recomenda-se retornar o objeto no corpo da resposta — útil se o servidor adicionou dados (como data de criação).

  2. Retornar **204 NO CONTENT** com corpo vazio.  
    A justificativa aqui é que, como PUT apenas coloca um objeto na URI, o cliente já sabe o que foi salvo — não precisa de resposta.

---

## POST, PUT, PATCH e Operações CRUD – Resumo  
A tabela abaixo resume os comportamentos discutidos:



| Método HTTP | Operação | Definição da URI do Recurso | O que faz?                 | Código de Resposta | Corpo da Resposta |
|-------------|----------|-----------------------------|----------------------------|--------------------|-------------------|
| POST        | Create   | Servidor gera e retorna URI | Cria sub-recurso dentro da URI | 201 CREATED | Recurso criado |
| PUT         | Create   | Cliente fornece a URI       | Cria recurso na URI fornecida   | 201 CREATED | Recurso criado |
| **PUT**     | **Update** | Cliente fornece a URI     | Substitui o recurso por completo | **204 NO CONTENT** | (vazio) |
| PATCH       | Update   | Cliente fornece a URI       | Atualização parcial             | 200 OK       | Recurso atualizado |



Na API Cash Card, não precisamos permitir que PUT crie recursos. Também não precisamos adicionar dados no servidor durante a atualização, nem permitir atualizações parciais. Portanto, nosso endpoint PUT se limita à terceira linha da tabela acima.

As linhas em negrito são as implementadas pela API Cash Card. As demais não são.


## Segurança

**Mais uma decisão:** como aplicar segurança à operação de atualização?

Na lição de segurança simples, decidimos retornar **404 NOT FOUND** para requisições GET em dois casos:


  - IDs inexistentes

  - Acesso não autorizado a cartões


Usaremos a mesma estratégia para o endpoint de atualização, pelos mesmos motivos (que serão detalhados no laboratório).


## Decisões da Nossa API

Ufa! Muitas decisões. Vamos resumir:


  >>>

  - PUT **não** será usado para criar um Cash Card.

  - O novo endpoint de atualização:
  
    - Usará o verbo **PUT**
    - Aceitará um objeto Cash Card e substituirá o existente
    - Retornará **204 NO CONTENT** em caso de sucesso
    - Retornará **404 NOT FOUND** para atualizações não autorizadas ou IDs inexistentes

  >>>


## Neste Artigo

  - PUT e PATCH  
  - PUT e POST  
  - Resumo: POST, PUT, PATCH e CRUD  
  - Segurança  
  - Decisões da API


## Resumo

Esta lição abordou as operações de **Update** e **Create** no contexto REST, e como elas podem ser implementadas com os verbos HTTP apropriados.

Agora que tomamos decisões importantes sobre como implementar o Update na API Cash Card, vamos escrever código. Hora de ir para o laboratório!
