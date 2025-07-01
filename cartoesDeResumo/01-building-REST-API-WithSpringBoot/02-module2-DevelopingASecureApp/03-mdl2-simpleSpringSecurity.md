# Segurança Simples com Spring Security

Vamos voltar nossa atenção para o nosso Steel Thread, concentrando-nos em outro componente da arquitetura: Segurança.

## 1. O que é Segurança?  

Segurança de software pode significar muitas coisas. O campo é extenso e merece um curso próprio. Nesta lição, falaremos sobre Segurança Web. 


Mais especificamente, abordaremos como funcionam Autenticação e Autorização HTTP, 

  >>>

  - As formas comuns de ataque no ecossistema web e como podemos usar o Spring Security para impedir acessos não autorizados ao nosso serviço Family Cash Card.

  >>>


## 2. Autenticação  
Um usuário de uma API pode ser uma pessoa ou outro programa, 


  - Então frequentemente usamos o termo **Principal** como sinônimo de “usuário”. 


**`Autenticação`**:


  >>>

  - É o ato de um Principal **provar sua identidade ao sistema**. 
  
  
  - Uma forma de fazer isso é `fornecer credenciais` (por exemplo, usuário e senha via Basic Authentication).


  - Dizemos que, uma vez apresentadas as credenciais corretas, o Principal está autenticado — ou seja, o usuário fez login com sucesso.

  >>>


**`HTTP`**


  >>>


  - É um protocolo sem estado, então cada requisição deve conter dados que provem que vem de um Principal autenticado.


  - Embora seja possível enviar credenciais a cada requisição, isso é ineficiente, pois exige mais processamento no servidor. 
  
  
  - Em vez disso, cria-se uma **Sessão de Autenticação** quando o usuário se autentica. 
  
  
  - Usaremos um mecanismo comum: um **Token de Sessão** (uma string aleatória) gerado e armazenado em um **Cookie**. 
    
    
    - Um Cookie é um conjunto de dados guardado no cliente web (por exemplo, navegador) e associado a um URI específico.


  >>>


  - **Duas vantagens dos Cookies:**


    - Eles são enviados automaticamente em toda requisição (sem código adicional). Basta o servidor verificar se o Token no Cookie é válido para rejeitar requisições não autenticadas.  


    - Eles podem persistir mesmo após fechar a página, melhorando a experiência do usuário.


## 3. Spring Security e Autenticação  

O Spring Security implementa a autenticação na **Filter Chain**. Esse componente da arquitetura web Java define uma sequência de filtros executados antes do Controller. 

  >>>

  - Cada filtro decide permitir ou não a continuação do processamento. 


  - O Spring Security injeta um filtro que verifica a autenticação do usuário, retornando `401 UNAUTHORIZED` caso a requisição não esteja autenticada.


  >>>


## 4. Autorização  
Até agora falamos de autenticação, mas este é só o primeiro passo. A **Autorização** acontece após a autenticação, permitindo que usuários diferentes do mesmo sistema tenham permissões distintas.

O Spring Security fornece autorização via **Controle de Acesso Baseado em Papéis** (RBAC). 


  >>>

  - Cada Principal possui um ou mais **Papéis** (Roles). 
  
  
  - Cada recurso ou operação especifica quais Roles são necessárias para executar ações autorizadas. Por exemplo, um usuário com Role de Administrador pode realizar mais ações que outro com Role de Proprietário de Cartão. 
  
  
  - Você pode configurar autorização por papéis globalmente ou por método.

  >>>


## Política de Mesma Origem  

A web é um lugar perigoso, onde agentes maliciosos aproveitam vulnerabilidades. O mecanismo básico de proteção é a **Same Origin Policy (SOP)**, 


  >>>


  - Que determina que apenas scripts contidos na própria página podem enviar requisições ao mesmo domínio (origem) daquela página.


  - A `SOP` é crítica para a segurança, pois sem ela qualquer página poderia conter um script que envia requisições a outros sites. 
  

    - **Por exemplo:** Se um usuário estiver logado no seu banco e visitar uma página maliciosa em outra aba, esse site poderia enviar requisições (com Cookies de autenticação) ao site do banco, resultando em saques indesejados!


  >>>


## Compartilhamento de Recursos entre Origens (CORS)  

Em arquiteturas distribuídas (microserviços), serviços podem rodar em domínios diferentes. O **Cross-Origin Resource Sharing (CORS)** 
  
  
  - É um mecanismo que permite a navegadores e servidores relaxar a SOP. Um servidor pode listar origens permitidas. 
  
  
  - O Spring Security disponibiliza a anotação `@CrossOrigin` para especificar as origens autorizadas. Cuidado: sem argumentos, ela libera todas as origens!


## Explorações Comuns na Web  

Além de explorar vulnerabilidades conhecidas, agentes maliciosos descobrem constantemente novas falhas. Felizmente, o Spring Security oferece ferramentas para mitigar ataques comuns. Vamos ver dois deles:


### Falsificação de Solicitação Entre Sites (CSRF)  

Conhecido como “Sea-Surf” ou Session Riding, o CSRF aproveita Cookies de sessão: 


  >>>

  - Um código malicioso envia requisições a um servidor onde o usuário já está autenticado. 


  - O servidor, ao receber o Cookie de autenticação, não consegue saber se o usuário autorizou o pedido.

  >>>


Para proteger contra CSRF, usa-se um **Token CSRF**, diferente do Token de sessão, pois é gerado a cada requisição. 


  - Isso dificulta que atacantes se insiram na comunicação. O Spring Security já suporta CSRF tokens por padrão.


### Cross-Site Scripting (XSS)  

Ainda mais perigoso é o **Cross-Site Scripting**, que ocorre quando um invasor faz a aplicação executar código arbitrário. 


  - Por exemplo, salvar no banco uma string com `<script>` e depois renderizá-la na página, executando o script.


XSS é mais crítico que CSRF, pois permite execução de código arbitrário no cliente ou no servidor, sem depender de autenticação. A principal defesa é **sanitizar** todos os dados externos (como entradas de formulários ou parâmetros de URI), escapando caracteres especiais HTML antes da renderização.

Ufa! Essa foi nossa rápida introdução à Segurança Web. O tema é vasto, mas agora você tem uma visão geral de como o Spring Security pode proteger suas aplicações e usuários!

---

## Neste artigo  

  - O que é Segurança?  
  - Autenticação  
  - Spring Security e Autenticação  
  - Autorização  
  - Política de Mesma Origem  
  - Compartilhamento de Recursos entre Origens (CORS)  
  - Explorações Comuns na Web  


## Resumo  
**Instrutores**  


  - Autenticação e autorização são os dois pilares mais importantes da segurança de software. Em aplicações web modernas, esses aspectos estão sob constante ataque via CSRF, XSS e falhas em CORS.


  - Felizmente, o Spring Security é a ferramenta mais poderosa da comunidade Spring para gerenciar a segurança de aplicações Spring Boot. E é muito fácil de incorporar e configurar.


Agora, vamos proteger nossa crescente API Family Cash Card usando o Spring Security.  
