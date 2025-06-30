# 🚀 Implementando o Endpoint GET – Visão Estruturada

Vamos organizar em etapas conceituais, cada uma com uma explicação aprofundada, um exemplo lúdico (para crianças) e um exemplo prático com boas práticas.

---

## 🌐 1. REST, CRUD e HTTP – O Trinômio Fundamenta­l  
**Conceito:**  
- REST (Representational State Transfer) define como expor “recursos” (objetos do domínio) por meio de URIs e verbos HTTP.  
- CRUD (Create, Read, Update, Delete) são as quatro operações essenciais aplicadas a esses recursos.  
- HTTP é o protocolo subjacente que transporta Requests e Responses entre cliente e servidor.  

**Exemplo lúdico:**  
“Imagine um zoológico onde cada animal (recurso) tem sua própria casinha (URI). Você usa bilhetes coloridos (GET, POST, PUT, DELETE) para falar com o tratador e pedir para ver, alimentar, mudar ou levar embora o animal.”  

**Exemplo prático & boas práticas:**  
- **URI semântica:** `/cashcards/{id}`, nunca `/getCashCard?id=…`  
- **Verbos corretos:** GET para leitura, POST para criação, PUT/PATCH para atualização e DELETE para remoção.  
- **Documentação:** mantenha um spec OpenAPI que reflita exatamente esses endpoints.

---

## 📊 2. Convenções RESTful e Códigos HTTP – O Guia de Tráfego  
**Conceito:**  
CRUD mapeado em HTTP estabelece respostas padronizadas:
| Operação | URI                    | Método | Status Padrão      |
|----------|------------------------|--------|--------------------|
| Create   | `/cashcards`           | POST   | 201 CREATED        |
| Read     | `/cashcards/{id}`      | GET    | 200 OK             |
| Update   | `/cashcards/{id}`      | PUT    | 204 NO CONTENT     |
| Delete   | `/cashcards/{id}`      | DELETE | 204 NO CONTENT     |

**Exemplo lúdico:**  
“É como semáforos: verde (200 OK) indica ‘siga em frente’, amarelo (202 ACCEPTED) ‘prepare-se’ e vermelho (404 NOT FOUND) ‘pare, não existe esse caminho’.”  

**Exemplo prático & boas práticas:**  
- **Idempotência:** GET, PUT e DELETE devem ser idempotentes; múltiplas chamadas não mudam o estado além da primeira.  
- **Localização no Create:** após POST, retorne `Location: /cashcards/{newId}` no header.  
- **Erros claros:** use 400 Bad Request para payload inválido, 404 Not Found para recurso ausente e 500 para erros inesperados.

---

## 📦 3. Estrutura de Request e Response – Os Ingredientes da Receita  
**Conceito:**  
- **Request:** verbo, URI, headers (`Content-Type`, `Authorization`), e body (JSON em POST/PUT).  
- **Response:** status code, headers (`Content-Type`, caching) e body (JSON com recurso ou erro).  

**Exemplo lúdico:**  
“Fazer pizza por telefone: diga o sabor (URI), tamanho (headers) e cobertura extra (body). O pizzaiolo responde com tempo de entrega (status) e confirma o pedido (body).”  

**Exemplo prático & boas práticas:**  
```http
GET /cashcards/123 HTTP/1.1
Host: api.meusite.com
Accept: application/json
Authorization: Bearer <token>

HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: max-age=60

{
  "id": 123,
  "amount": 25.00
}
```

Boas práticas:

Proteja com HTTPS.

Use cabeçalhos de cache para recursos estáticos.

Padronize formato de data (ISO-8601).

💳 4. Exemplo Prático de GET /cashcards/{id} – Mãos na Massa
Request Simples:

bash
curl -i \
  -H "Authorization: Bearer <token>" \
  http://localhost:8080/cashcards/123
Resposta Esperada:

http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": 123,
  "amount": 25.00
}
Exemplo lúdico: “É como colocar seu cartão de sócio numa catraca e ouvir ‘bip’ de sucesso, mostrando quantas fichas você ainda tem.”

Exemplo prático & boas práticas:

Teste manual inicial: use curl ou Postman antes de escrever código.

Automação: crie testes com MockMvc para validar status e payload.

🏗️ 5. Spring Boot: Component Scan e REST Controller
🔍 5.1. IoC Container e Component Scan
O Spring varre pacotes anotados e registra Beans no IoC container.

Permite injeção automática de dependências (@Autowired ou via construtor).

Exemplo lúdico: “Imagine um mordomo que, ao chegar na mansão, conhece todos os funcionários (Beans) e sabe exatamente onde deixá-los no salão de festas.”

Exemplo prático & boas práticas:

Organize pacotes por camada: controller, service, repository.

Prefira injeção por construtor para facilitar testes e imutabilidade.

📡 5.2. Criando o Controller REST
java
@RestController
@RequestMapping("/cashcards")
public class CashCardController {
  private final CashCardService service;
  public CashCardController(CashCardService service) {
    this.service = service;
  }
}
@RestController = @Controller + @ResponseBody (retorna JSON).

@RequestMapping agrupa URIs comuns.

Exemplo lúdico: “O Controller é o recepcionista do hotel, recebendo pedidos e chamando o serviço certo (Service).”

Exemplo prático & boas práticas:

Separe responsabilidades: o Controller só orquestra, a lógica vai no Service.

Trate exceções globais com @ControllerAdvice.

🚦 5.3. Mapeando GET com @GetMapping e @PathVariable
java
@GetMapping("/{id}")
public ResponseEntity<CashCard> getById(@PathVariable("id") Long id) {
  return service.findById(id)
      .map(ResponseEntity::ok)
      .orElse(ResponseEntity.notFound().build());
}
@GetMapping("/{id}") rege GET /cashcards/{id}.

@PathVariable liga o placeholder {id} ao parâmetro do método.

Exemplo lúdico: “É como apertar o botão com o número do elevador e o elevador (método) desce até você.”

Exemplo prático & boas práticas:

Valide entrada com @Min(1) e retorne 400 para IDs inválidos.

Utilize Optional no Service para tratar ausência de forma elegante.

📨 5.4. Construindo a Resposta com ResponseEntity
java
return ResponseEntity
    .ok()                           // 200 OK
    .header("X-Cache-Status","HIT")// cabeçalho customizado
    .body(cashCard);
Permite configurar status, headers e body.

Exemplo lúdico: “Empacotar um presente com etiqueta ‘frágil’ e bilhete de agradecimento.”

Exemplo prático & boas práticas:

Adicione ETag/Last-Modified para otimizar cache.

Retorne Location nos POST e Allow nos 405 Method Not Allowed.

🎉 Resumo Final

REST & CRUD: entenda recursos, verbos e status.

Request/Response: método, URI, headers, body.

Spring Boot: IoC, @RestController, @GetMapping, ResponseEntity.

Boas práticas: semântica de URIs, segurança HTTPS, testes automatizados, documentação OpenAPI.

Agora você tem a base para criar endpoints GET robustos e bem estruturados em Spring Boot. Próximos passos: POST, PUT e DELETE seguindo o mesmo padrão!