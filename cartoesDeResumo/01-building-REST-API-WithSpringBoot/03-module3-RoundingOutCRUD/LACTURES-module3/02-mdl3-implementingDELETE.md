## Implementando DELETE  

Nesta lição, vamos implementar a última das quatro operações CRUD: Delete! Até agora, você já deve estar familiarizado com a primeira pergunta que devemos fazer: 

Qual é a especificação de dados da API para o endpoint Delete? Essa especificação inclui os detalhes da Requisição e da Resposta.

No risco de revelar o resultado antes da hora, esta é a especificação que definiremos:

  >>>

- **`Request:`** 
 

    - **Verbo:** DELETE  

    - **URI:** /cashcards/{id}  

    - **Body:** (vazio)  


  >>>


  >>>


  - **`Response:`**  


  - **Código de status:** 204 NO CONTENT  

  - **Body:** (vazio)  


Retornaremos o código `204 NO CONTENT` para um delete bem-sucedido, mas há casos adicionais:


  >>>
| Código de Resposta | Caso de Uso                                                                  |
|--------------------|------------------------------------------------------------------------------|
| 204 NO CONTENT     | O registro existe, e o Principal está autorizado, e o registro foi excluído. |
| 404 NOT FOUND      | O registro não existe (foi enviado um ID inexistente).                       |
| 404 NOT FOUND      | O registro existe, mas o Principal não é o proprietário.                     |
  >>>

Por que retornamos 404 tanto para “ID não existe” quanto para “não autorizado”? Para não vazar informação: se a API respondesse resultados diferentes, um usuário não autorizado poderia descobrir IDs que não tem permissão de acessar.


## Opções Adicionais 

Vamos explorar algumas opções que podemos considerar ao implementar a operação Delete.


### Hard Delete e Soft Delete


  - **Hard delete**: remover de vez o registro do banco de dados. Após a exclusão, o dado some permanentemente.  


  - **Soft delete**: marcar o registro como “deletado” em vez de removê-lo fisicamente. Por exemplo, adicionar uma coluna `IS_DELETED` (boolean) ou `DELETED_DATE` (timestamp). O repositório então deve filtrar registros com `IS_DELETED = true` nas operações de leitura.


### Audit Trail e Arquivamento  

Frequentemente é exigido manter um histórico de alterações nos dados. Exemplos: 


  >>>

  - Um atendente precisa saber quando um cliente excluiu seu Cash Card.  

  - Regulamentações de retenção de dados podem exigir que registros excluídos sejam mantidos por um período.  


  >>>


Para rastrear histórico podemos:  


  1. - **Arquivar** os dados excluídos em outra tabela ou banco.  


  2. - **Adicionar campos de auditoria** na própria tabela (ex.: `DELETED_DATE`, `DELETED_BY_USER`).  


  3. - **Manter um audit trail** separado com todas as operações de Create, Update e Delete.  


    APIs que adotam soft delete e campos de auditoria às vezes retornam `200 OK` e o estado do objeto no corpo da resposta. Mas escolhemos `204 NO CONTENT` para deixar claro que não há corpo.


É possível combinar estratégias:


  - Fazer soft delete e depois, periodicamente, hard delete ou arquivar registros marcados.  


  - Executar hard delete e ainda assim arquivar os registros em outro local.  


  - Em qualquer caso, manter um log de auditoria com data/hora de cada operação.  


  - **Observação:** a especificação simples que escolhemos (`204 NO CONTENT`) não determina se fazemos hard ou soft delete, tampouco se auditamos ou arquivamos. Porém, `204` implica ausência de corpo, sugerindo hard delete sem audit fields no payload.


## Neste artigo 

  - Opções adicionais  

  - Audit Trail e Arquivamento  

  - Resumo  


## Resumo  

Nesta lição, apresentamos o conceito de exclusão de um Cash Card e os códigos de resposta HTTP semânticos. 

  - Descrevemos em detalhes hard delete e soft delete, com vantagens e desvantagens de cada um. 
  
  
  - Além disso, abordamos conceitos de auditoria de dados para rastrear datas e horários de exclusão de entidades.

No laboratório, faremos o mais simples: implementaremos hard delete sem auditoria ou arquivamento.  
