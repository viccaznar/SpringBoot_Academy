# 🤝 Contratos de API & JSON

Uma API contract é o acordo formal que define como provedores e consumidores trocam dados — endpoints, verbos HTTP, códigos de resposta e formados de payload. Vamos destrinchar esse conceito em tópicos explicativos, com exemplos lúdicos para crianças e exemplos práticos com boas práticas.



## 1️⃣ O que são Contratos de API? 🔏  
- **Definição:**  

- Um contrato de API documenta de forma precisa **como** e **quando** clientes e servidores interagem.  
- Serve como “manual de instruções” compartilhado, evitando surpresas e ambiguidades.


  - **Exemplo lúdico (criança):**  
  - “Dois amigos combinam as regras do jogo de tabuleiro antes de começar: quem tira 6 ganha, quem não tem peças passa a vez.”  
  

  - **Exemplo prático & boas práticas:**  
  - Mantenha um arquivo OpenAPI (`api.yaml`) versionado em Git.  
  - Inclua descrições claras de cada operação e exemplos de request/response.  
    ```yaml
    paths:
      /cashcards/{id}:
        get:
          summary: “Consulta cartão de mesada pelo ID”
          parameters:
            - in: path
              name: id
              schema:
                type: integer
              required: true
              description: “Identificador único do CashCard”
          responses:
            '200':
              description: “Cartão retornado com sucesso”
            '401':
              description: “Não autorizado”
            '404':
              description: “Não encontrado”
  ```



## 2️⃣ Componentes Essenciais do Contrato 📐


- **Endpoint URI –** endereço onde o recurso vive.

- **HTTP Verb –** ação a ser executada (GET, POST, etc.).

- **Status Codes –** indicam sucesso ou falha (200, 201, 400, 404, 500).

- **Payload –** formato (JSON) e estrutura de dados.


  - **Exemplo lúdico (criança):**
  - “Na receita de bolo, você precisa de ingredientes (payload), modo de fazer (verbo), forno ligado a 180°C (status code), e um nome para o prato (URI).”


  >>> - **Exemplo prático & boas práticas:**

    Utilize snippets de request/response em Postman Collections para validar automaticamente.

    Defina schemas JSON via components/schemas no OpenAPI e gere DTOs no backend.

  >>>



## 3️⃣ Por que usar Contratos? 🛡️
- **Alinhamento:** Todos sabem exatamente o que esperar da API.

- **Teste Automatizado:** Suítes de contract tests validam que implementações não quebrem o contrato.

- **Evolução Segura:** Permitem versionamento cuidadoso, evitando breaking changes.


  - **Exemplo lúdico (criança):**
  “Uma banda ensaia cada nota antes do festival, garantindo que o maestro e os instrumentistas estejam em sincronia.”


  - **Exemplo prático & boas práticas:**
  Implemente Pact ou Spring Cloud Contract para rodar tests de consumidor e provedor antes do merge.
  Configure o pipeline CI para falhar se qualquer contract test quebrar.



## 4️⃣ JSON: Formato de Intercâmbio de Dados 🌐
JSON (JavaScript Object Notation) é leve, legível e quase universal.

  - Representa objetos como pares chave–valor, suportado por todas as linguagens modernas.

  - **Exemplo lúdico (criança):**
  “Cada figurinha de álbum traz o nome e a foto do jogador — é só colar no lugar certo!”


  >>> - **Exemplo prático & boas práticas:**

    - Padronize propriedades em camelCase (firstName, lastName).

    - Valide JSON com JSON Schema e use anotations como @JsonProperty no Java.
  
  >>> 


  ```json
    {
      "id": 99,
      "amount": 123.45,
      "ownerName": "Joãozinho"
    }
  ```



## 5️⃣ Integrando Testes e Automação 🤖
- Escreva contract tests que confirmem request/response contra o contrato.

- Gere documentação dinâmica (Swagger UI, ReDoc) a partir do spec.

- Mantenha testes unitários, integração e end-to-end alinhados ao contrato.


  - **Exemplo lúdico (criança):**
  “Construa um robô que só toca música se todas as notas da partitura estiverem corretas, senão ele apita com erro!”


  >>> - **Exemplo prático & boas práticas:**
    
    - Use Spring Rest Docs para gerar snippets de API e incluí-los no manual técnico.

    - Configure Pact Broker para publicar e versionar contratos entre times.

  >>>


## 6️⃣ Boas Práticas & Próximos Passos 🚀
- **Versionamento Semântico:** altere major quando fizer breaking change.
- **Deprecação Gradual:** anuncie endpoints obsoletos antes de removê-los.

  - Monitore erros 4xx/5xx para evoluir contratos com base em uso real.


  - **Exemplo lúdico (criança):**
  “No jogo, você ganha conquistas ao completar tarefas e desbloqueia fases extras com recompensas.”


  >>> - **Exemplo prático & boas práticas:**
  
    - Publique o spec no GitHub Pages ou SwaggerHub para fácil acesso.

    - Automatize alertas (Slack, e-mail) quando novos consumidores registrarem incompatibilidades de contrato.
  
  >>>



## 🎉 Resumo 
Contratos de API + JSON formam a base de uma comunicação confiável e escalável entre sistemas. Com documentação clara, testes automatizados e boas práticas de versionamento, você garante que provedores e consumidores caminhem juntos sem surpresas.