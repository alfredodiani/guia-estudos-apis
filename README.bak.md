1. Arquitetura REST e seus Fundamentos
Recursos e URIs: O estilo arquitetural REST (Representational State Transfer) baseia-se no conceito de "recursos" (que podem ser qualquer dado ou objeto de negócio), os quais são identificados através de endereços únicos chamados URIs (Uniform Resource Identifiers)
.
Comunicação Stateless (Sem Estado): Cada requisição do cliente para o servidor deve conter todas as informações necessárias para ser processada. O servidor não deve armazenar o contexto do cliente entre as requisições, o que facilita a escalabilidade
.
Separação Cliente-Servidor: O cliente (quem consome) e o servidor (quem provê os dados) possuem responsabilidades distintas. Isso permite que ambas as partes evoluam de forma independente
.
2. Design de URIs e Nomenclatura
Uso de Substantivos: As rotas da sua API devem ser nomeadas com substantivos que representam os recursos (ex: /produtos ou /usuarios) e nunca com verbos (evite /obterProdutos ou /deletarUsuario)
.
Pluralização e Hierarquia: O padrão da indústria é utilizar substantivos no plural para representar coleções de recursos. Além disso, as URIs devem demonstrar relacionamentos de forma hierárquica, como /clientes/{id}/pedidos
.
3. Métodos HTTP (Verbos) Os métodos do protocolo HTTP definem a intenção da operação sobre um recurso, equivalendo às operações CRUD (Create, Read, Update, Delete)
:
GET: Recupera dados de um recurso. É uma operação segura que não deve alterar o estado do servidor
.
POST: Cria um novo recurso em uma coleção
.
PUT: Atualiza ou substitui de forma completa um recurso existente
.
PATCH: Realiza uma atualização parcial, modificando apenas campos específicos de um recurso
.
DELETE: Remove um recurso
.
4. Códigos de Status HTTP A API deve comunicar claramente o resultado de uma operação usando os códigos de status padronizados do HTTP
:
2xx (Sucesso): Indica que a requisição funcionou. Ex: 200 OK (sucesso padrão) e 201 Created (recurso criado com sucesso)
.
4xx (Erro do Cliente): O cliente enviou uma requisição inválida. Ex: 400 Bad Request (erro de sintaxe ou validação), 401 Unauthorized (falta de autenticação), 403 Forbidden (sem permissão) e 404 Not Found (recurso não encontrado)
.
5xx (Erro do Servidor): O servidor falhou ao processar a requisição. Ex: 500 Internal Server Error
.
5. Formatos de Dados (JSON)
O JSON (JavaScript Object Notation) tornou-se o formato de troca de dados mais popular e padronizado em APIs modernas por ser leve, legível por humanos e facilmente interpretado por linguagens de programação
. Aprenda a estruturar requisições e respostas nesse formato.
6. Segurança de APIs Como as APIs expõem a lógica de negócios e dados sensíveis para a internet, segurança é um tópico crítico
:
Autenticação e Autorização: Estude o padrão OAuth 2.0 e o uso de JWT (JSON Web Tokens), que são amplamente utilizados para garantir que apenas usuários verificados e com as permissões corretas acessem os dados
.
Criptografia: Todo o tráfego de dados da API deve ocorrer sobre HTTPS (TLS) para evitar interceptação de dados
.
OWASP API Security Top 10: Familiarize-se com as principais vulnerabilidades de APIs listadas pela OWASP, como a Falha de Autorização em Nível de Objeto (BOLA), Exposição Excessiva de Dados e Injeção
.
7. Performance e Otimização
Paginação e Filtros: Para não sobrecarregar o servidor ou o cliente, as APIs não devem retornar todos os registros do banco de dados de uma vez. O uso de parâmetros de busca (query parameters), como limit e offset, é essencial para criar a paginação dos dados
.
Rate Limiting: Controlar a quantidade de requisições que um cliente pode fazer em um determinado período protege a API contra abusos e ataques de negação de serviço (DoS)
.
Cache: A implementação de estratégias de cache (no cliente ou no servidor) reduz a latência e a carga no sistema para dados que são solicitados frequentemente e mudam pouco
.
8. Documentação com OpenAPI
Uma API sem documentação é extremamente difícil de ser adotada. Estude a OpenAPI Specification (anteriormente Swagger), que é o padrão de mercado para documentar de forma clara quais rotas existem, quais parâmetros são exigidos e o que a API responde. Ferramentas baseadas em OpenAPI geram documentações interativas que permitem aos desenvolvedores testarem a API diretamente pelo navegador
.
9. Versionamento
Com o tempo, as regras de negócios mudam e a API precisa evoluir. Aprenda estratégias de versionamento (como inserir a versão na URI, ex: /v1/usuarios) para adicionar novas funcionalidades ou fazer mudanças sem quebrar as aplicações dos clientes que já consomem versões mais antigas da sua API
.
Focar nesses pilares te proporcionará uma base sólida, permitindo não apenas consumir APIs de terceiros corretamente, mas também projetar e construir suas próprias APIs de maneira profissional e escalável.
Quais tópicos um desenvolvedor mais experiênte deveria estudar para se aprofundar mais no design e implementação da APIs?

Para um desenvolvedor mais experiente, o foco deve mudar da implementação básica para tópicos relacionados à arquitetura escalável, resiliência, segurança profunda e padrões avançados de comunicação. Com base nas fontes fornecidas, aqui estão os tópicos avançados que um desenvolvedor experiente deve dominar:
1. Hipermídia e Nível Máximo de Maturidade REST
HATEOAS (Hypertext as the Engine of Application State): Estudar como alcançar o "Nível 3" do Modelo de Maturidade de Richardson
. O HATEOAS permite que a API seja autodescritiva, retornando links que instruem os clientes sobre quais ações de navegação ou transição de estado estão disponíveis no momento para um determinado recurso
. Isso reduz significativamente o acoplamento entre o cliente e a API, permitindo que as regras de negócio e as rotas evoluam no servidor sem quebrar o front-end
.
2. Operações Complexas e Assíncronas
Operações de Longa Duração (Long-Running Operations - LROs): Como lidar com solicitações cujo processamento excede o tempo de uma resposta HTTP normal. O padrão correto envolve retornar um status 202 Accepted e um cabeçalho (como Operation-Location) contendo a URI de um "monitor de status" onde o cliente pode fazer polling para checar o progresso
.
Webhooks e Callbacks: Para interações iniciadas pelo provedor da API (comunicação reversa ou orientada a eventos), estude como modelar Webhooks e Callbacks na especificação OpenAPI, permitindo que a API notifique o cliente assim que um evento ocorrer
.
3. Resiliência, Concorrência e Tolerância a Falhas
Controle de Concorrência Otimista (Optimistic Concurrency): O uso avançado de Entity Tags (ETag) e cabeçalhos condicionais (If-Match, If-None-Match, If-Modified-Since)
. Isso previne problemas como atualizações perdidas (lost updates) quando múltiplos clientes tentam modificar o mesmo recurso concorrentemente
.
Idempotência e Repetibilidade: Em sistemas em nuvem distribuídos, os clientes devem poder repetir requisições (retry) no caso de falhas de rede sem causar efeitos colaterais não intencionais
. O estudo e implementação de cabeçalhos de rastreio de repetibilidade (ex: Repeatability-Request-ID) em métodos POST tornam transações críticas à prova de falhas
.
4. Design Profundo de Dados e Estruturas
Polimorfismo: Como modelar e documentar corretamente recursos que podem ter múltiplos tipos estruturais (usando herança ou propriedades como allOf, oneOf, e anyOf no OpenAPI), exigindo o uso de campos discriminadores (como kind ou type)
.
Otimização e APIs "Chatty" (Tagarelas): Técnicas para evitar que clientes façam dezenas de chamadas HTTP para montar uma única tela
. Isso abrange o desenvolvimento de respostas parciais (o cliente especifica quais campos quer receber usando ?fields=id,nome) e operações em lote (Batch)
.
Grandes Payloads e Streaming: Como lidar com arquivos muito grandes usando a codificação Chunked transfer (transferência em pedaços) ou permitindo Range requests (retorno de conteúdo parcial com o status 206 Partial Content)
.
Padrão API Façade: Criação de uma camada de mediação (API Gateway/Façade) que serve para ocultar a complexidade de sistemas de back-end legados, orquestrar múltiplos microsserviços e aplicar políticas globais (segurança, cache, versionamento)
.
5. Segurança Profunda e DevSecOps
OWASP API Security Top 10: Superar os conhecimentos básicos de segurança (como apenas adicionar HTTPS) e entender vulnerabilidades modernas como:
Broken Object Level Authorization (BOLA): Quando clientes forçam a manipulação do ID de objetos pertencentes a outros usuários
.
Mass Assignment: Quando hackers enviam propriedades não previstas no JSON (ex: "credit_balance": 99999) e o framework backend as injeta indevidamente nos modelos internos
.
Broken Function Level Authorization: Executar funções administrativas apenas alterando o método HTTP ou adivinhando URLs (ex: trocando de GET para DELETE ou /users por /admins)
.
Integração DevSecOps: Automação de testes de segurança de APIs em pipelines CI/CD com foco nas especificidades da API sem comprometer a velocidade da entrega
.
Autenticação Avançada e Zero Trust: Empregar Arquitetura de Confiança Zero e segurança orientada à inteligência artificial para detecção de ameaças em tempo real
.
6. Monitoramento, Tracing e Performance em Microsserviços
Rastreamento Distribuído (Distributed Tracing): Quando uma API chama outras cinco no backend, monitorar de onde vêm os gargalos ou falhas é essencial. Estude o uso de ferramentas como OpenTelemetry e a propagação de contexto de rastreio através de cabeçalhos como Correlation-ID, X-Request-ID ou X-Trace-ID
.
Estratégias de Rate Limiting (Limitação de Taxa): Ir além do limite global e criar limites multidimensionais (ex: baseados no custo computacional de um endpoint ou tipo de assinatura/nível de cliente)
.
7. Alternativas Modernas ao REST
GraphQL e gRPC: Avaliar as limitações do REST para certos casos de uso, sabendo arquitetar sistemas usando o GraphQL (quando clientes precisam consultar dinamicamente grafos de dados sem over-fetching) ou gRPC (para comunicação ultra-rápida, compacta e serializada entre microsserviços)
.
Qual uma maneira simples e efetiva de retornar erros nas APIs? Cite os 10 casos mais comuns de erros e como fazer o retorno.

Uma maneira simples e efetiva de retornar erros em APIs é combinar o uso de códigos de status HTTP padronizados para sinalizar o resultado da operação com um formato estruturado e consistente na resposta (geralmente um payload JSON)
. O corpo da resposta de erro deve sempre apresentar, no mínimo, um código identificador do erro (id ou code) e uma mensagem clara e descritiva do problema
,
. É crucial que os erros sejam consistentes em toda a API e que não exponham detalhes internos sensíveis de implementação ou stack traces ao cliente
,
.
Abaixo estão os 10 casos mais comuns de erros em APIs e como você deve fazer o retorno para o cliente:
1. 400 Bad Request (Requisição Inválida)
Quando ocorre: O cliente enviou dados inválidos, ocorreu um erro de sintaxe na requisição, ou a requisição falhou nas regras de validação
,
.
Como retornar: Responda com o status 400 acompanhado de um corpo (payload) que detalhe claramente quais dados ou propriedades do JSON estavam incorretos ou faltantes
,
.
2. 401 Unauthorized (Não Autorizado)
Quando ocorre: O cliente tentou acessar um recurso protegido sem fornecer as credenciais corretas ou omitiu a autenticação
,
.
Como retornar: Retorne o status 401 e inclua o cabeçalho WWW-Authenticate listando pelo menos um desafio de autenticação aplicável (como indicar que um token OAuth2/Bearer é necessário)
,
.
3. 403 Forbidden (Proibido)
Quando ocorre: O cliente está devidamente autenticado e a requisição é válida, mas ele não possui permissão no nível de aplicação (falha de autorização) para realizar aquela ação
,
.
Como retornar: Retorne o status 403. Diferente do erro 401, o cliente não deve ser instruído a repetir a requisição com as mesmas credenciais
. Uma alternativa aceita em alguns cenários de segurança restrita é retornar 404 Not Found para sequer confirmar a existência do recurso bloqueado
,
.
4. 404 Not Found (Não Encontrado)
Quando ocorre: A URI solicitada pelo cliente não pôde ser mapeada para nenhum recurso existente
,
.
Como retornar: Retorne o status 404 simples indicando a falha de localização
.
5. 405 Method Not Allowed (Método Não Permitido)
Quando ocorre: A URI de destino existe, mas o verbo HTTP utilizado pelo cliente (por exemplo, tentar um POST num endpoint somente-leitura) não é suportado pela rota
,
.
Como retornar: Retorne o status 405 e obrigatoriamente inclua o cabeçalho Allow na resposta listando quais métodos são aceitos por aquele recurso (ex: Allow: GET, PUT, DELETE)
,
.
6. 406 Not Acceptable (Não Aceitável)
Quando ocorre: A API não consegue formatar os dados no tipo de mídia exigido pelo cliente no cabeçalho de requisição Accept (por exemplo, o cliente exige application/xml e a API só opera com application/json)
,
.
Como retornar: Retorne o status 406 informando que o formato solicitado não pode ser servido
.
7. 409 Conflict (Conflito)
Quando ocorre: A requisição não pode ser concluída pois causa um conflito com o estado atual do recurso no servidor
,
. Exemplos incluem violações de concorrência ou tentar deletar um recurso raiz que ainda contém recursos filhos ou pedidos pendentes
,
.
Como retornar: Retorne o status 409 acompanhado de um corpo que contenha informações suficientes para o cliente entender a origem do conflito e ter a chance de tentar resolvê-lo
.
8. 412 Precondition Failed (Falha de Pré-condição)
Quando ocorre: Fundamental em operações de atualização concorrentes. O cliente faz uma requisição condicional fornecendo cabeçalhos como If-Match (com uma Entity Tag ou ETag), mas a versão enviada não corresponde à versão atual dos dados no servidor
,
.
Como retornar: Retorne o status 412. Isso barra a operação de salvar para que o cliente não sobrescreva dados que outro usuário atualizou em paralelo
.
9. 415 Unsupported Media Type (Tipo de Mídia Não Suportado)
Quando ocorre: O formato do payload inserido pelo cliente no corpo da requisição não é compreendido pela API. Acontece quando o cabeçalho Content-Type não condiz com as expectativas do serviço (como enviar XML para uma rota exclusiva de JSON)
,
.
Como retornar: Retorne o status 415. Adicionalmente, pode-se usar o cabeçalho de resposta Accept ou documentar no corpo da mensagem quais são as mídias suportadas por aquela rota
.
10. 500 Internal Server Error (Erro Interno do Servidor)
Quando ocorre: Ocorreu um mau funcionamento, exceção não tratada na aplicação ou falha na infraestrutura/banco de dados
,
. É um erro que pertence exclusivamente à responsabilidade do servidor.
Como retornar: Retorne o status 500 com uma mensagem padronizada no corpo JSON. Capture as exceções completas e detalhadas (stack traces e componentes técnicos) e grave-as apenas em logs internos, relacionando-as usando um cabeçalho identificador como x-ms-request-id
,
.
Me explique sobre os itens de cabeçalho frequentemente utilizados em uma API Rest. Complemente com uma explicação de se "os cabeçalhos utilizados no padrão Rest são diferente de outros padrões de api".

Em uma API REST, os cabeçalhos (headers) HTTP são componentes fundamentais que transmitem metadados sobre a mensagem, o recurso solicitado e o cliente, sem fazer parte do conteúdo principal (corpo da mensagem)
.
Abaixo estão os cabeçalhos mais frequentemente utilizados em APIs REST e seus propósitos:
1. Negociação de Conteúdo (Content Negotiation)
Content-Type: Especifica qual é o formato (tipo de mídia ou MIME type) da representação do recurso que está sendo enviada no corpo da mensagem. Em APIs modernas, o valor mais comum é application/json ou application/xml
.
Accept: Utilizado na requisição pelo cliente para informar ao servidor quais tipos de mídia ele é capaz de processar na resposta (ex: Accept: application/json). Isso permite que a API suporte múltiplos formatos para a mesma informação
.
2. Segurança e Autenticação
Authorization: Carrega as credenciais do cliente para que ele se autentique junto ao servidor (como tokens Bearer/JWT ou chaves de API), permitindo o acesso a recursos protegidos
.
3. Criação e Navegação de Recursos
Location: Frequentemente utilizado pelo servidor em respostas com o status 201 Created (após um POST bem-sucedido). Ele contém a URI (endereço exato) do novo recurso que acabou de ser criado, permitindo que o cliente saiba onde encontrá-lo
.
Allow: Enviado pelo servidor geralmente em respostas a requisições OPTIONS ou quando ocorre um erro 405 Method Not Allowed. Ele lista os métodos HTTP (GET, POST, PUT, etc.) que são suportados por aquele recurso específico
.
4. Cache e Concorrência Otimista
Cache-Control, Expires e Date: Controlam o comportamento de armazenamento em cache das respostas por parte dos clientes e intermediários. O Cache-Control, por exemplo, pode definir o tempo máximo (em segundos) que os dados são considerados recentes (max-age)
.
ETag: Uma string opaca gerada pelo servidor que atua como um "identificador de versão" daquele estado específico do recurso. Se o recurso muda, o ETag muda
.
If-Match e If-None-Match: Usados pelo cliente em requisições condicionais. O If-None-Match usa o ETag guardado no cache do cliente para perguntar ao servidor se o recurso mudou; se não mudou, economiza banda retornando o status 304 Not Modified
. O If-Match é crucial em requisições PUT/PATCH para controle de concorrência, garantindo que o cliente não sobrescreva dados se outro usuário os tiver modificado no servidor enquanto isso
.
5. Metadados de Rastreabilidade
x-ms-request-id ou Correlation-ID / Trace-ID: Cabeçalhos muito comuns na prática de sistemas distribuídos para identificar unicamente uma transação de ponta a ponta, permitindo a correlação de logs e o diagnóstico de falhas
.
Os cabeçalhos no padrão REST são diferentes de outros padrões de API?
Sim, existe uma diferença filosófica e prática vital na forma como o REST utiliza os cabeçalhos em comparação a outros padrões (como SOAP ou RPC).
A principal diferença é que o REST abraça a arquitetura e os padrões nativos do protocolo HTTP de forma semântica
. Como parte da restrição de "Interface Uniforme" do REST, as operações e o estado devem ser comunicados utilizando os mecanismos padrão do HTTP
. Dessa forma, um cliente e um servidor concordam com o formato dos dados conversando pelos cabeçalhos Accept e Content-Type, gerenciam cache através do Cache-Control, e usam os métodos corretos para cada intenção. As boas práticas de design REST ditam inclusive que não se deve utilizar cabeçalhos customizados para alterar o comportamento fundamental de um método HTTP; a intenção principal sempre reside na semântica padrão do protocolo
.
Em contraste, padrões como SOAP ou arquiteturas puramente orientadas a RPC frequentemente tratam o HTTP apenas como um "túnel de transporte"
. Nesses outros padrões, os metadados de requisição — como a intenção da ação, roteamento, autenticação, detalhes do tipo de dado e controle de sessão — tendem a ser ignorados nos cabeçalhos HTTP e são empacotados ("envelopados") diretamente dentro da carga de dados principal (o payload, como um Envelope XML do SOAP)
. Consequentemente, outras arquiteturas acabam precisando reinventar mecanismos de cache, paginação e segurança que no REST já são resolvidos nativamente pelos cabeçalhos padronizados da Web
.
Hoje • 00:24
mostre um exemplo de api rest bem estruturado e explique seus detalhes

Para ilustrar a aplicação prática das melhores arquiteturas e padrões REST, usaremos o cenário de uma API de e-commerce na qual um cliente (aplicação) deseja buscar a lista de pedidos de um usuário específico.
Aqui está um exemplo de uma requisição e resposta de uma API REST bem estruturada:
1. A Requisição (Request)
```http
GET /v1/clientes/1234/pedidos?limit=10&offset=0 HTTP/1.1
Host: api.exemplo.com
Accept: application/json
```
Detalhes da Requisição:
Método HTTP (GET): Utilizado corretamente para recuperar dados sem causar efeitos colaterais (operações seguras e idempotentes)
.
Versionamento (/v1/): A inclusão da versão principal (major version) diretamente na URI é uma prática recomendada para garantir que futuras atualizações com quebras de contrato (breaking changes) não afetem os clientes atuais
.
Nomenclatura com Substantivos no Plural (/clientes, /pedidos): A URI utiliza substantivos (nunca verbos) no plural para identificar coleções de recursos. Isso mantém a API intuitiva e focada nos recursos
.
Hierarquia de Recursos (/clientes/1234/pedidos): Demonstra claramente o relacionamento pai-filho. O cliente está buscando a coleção de pedidos que pertence unicamente ao cliente de ID 1234
.
Paginação (?limit=10&offset=0): Como um cliente pode ter milhares de pedidos, o uso de parâmetros de query previne a sobrecarga do servidor e do banco de dados. O limit restringe a quantidade de itens a 10, e o offset indica de onde a contagem deve começar (neste caso, do início)
.
Negociação de Conteúdo (Accept: application/json): O cabeçalho Accept especifica que o cliente espera que a resposta seja formatada em JSON
.

--------------------------------------------------------------------------------
2. A Resposta (Response)
```http
HTTP/1.1 200 OK
Content-Type: application/json
API-Version: 1.0.2
Cache-Control: max-age=60, must-revalidate
ETag: "83921abc"
```

```json
{
  "dados": [
    {
      "id": "987",
      "total": 150.50,
      "status": "enviado",
      "data_criacao": "2026-05-10T14:30:00.000Z"
    },
    {
      "id": "988",
      "total": 20.00,
      "status": "pendente",
      "data_criacao": "2026-05-18T09:15:00.000Z"
    }
  ],
  "metadados": {
    "total": 45,
    "limit": 10,
    "offset": 0
  },
  "links": {
    "self": {
      "href": "https://api.exemplo.com/v1/clientes/1234/pedidos?limit=10&offset=0",
      "rel": "self"
    },
    "next": {
      "href": "https://api.exemplo.com/v1/clientes/1234/pedidos?limit=10&offset=10",
      "rel": "next"
    },
    "cliente": {
      "href": "https://api.exemplo.com/v1/clientes/1234",
      "rel": "parent"
    }
  }
}
```
Detalhes da Resposta:
Código de Status (200 OK): Comunica claramente que a operação foi bem-sucedida e que o conteúdo solicitado está no corpo da resposta
.
Cabeçalhos de Cache (Cache-Control e ETag): O Cache-Control instrui o cliente de que ele pode guardar esta resposta em cache por 60 segundos. O ETag ("Entity Tag") é um identificador opaco do estado atual desta representação, permitindo requisições condicionais futuras mais eficientes
.
Separação de Dados e Metadados: O payload JSON separa claramente a carga principal (dados) dos dados de apoio, como a estrutura de paginação (metadados). Os metadados ajudam o cliente a saber que existem 45 pedidos no total, permitindo desenhar corretamente a interface do usuário (ex: botões de "próxima página")
.
HATEOAS / Links (Hipermídia): Este é o grande diferencial de uma API REST de alto nível (Nível 3 de Maturidade). O bloco links permite a descoberta dinâmica da API, entregando ao cliente as URIs exatas para navegar no estado da aplicação
:
self: Aponta para a própria URI que gerou essa resposta, garantindo contexto e rastreabilidade
.
next: Como há paginação (retornou apenas 2 de 45 registros), o servidor já entrega a URI pronta para buscar a próxima página (offset=10), liberando o cliente de ter que calcular ou montar URLs no front-end
.
cliente / parent: Fornece a rota exata para obter os detalhes do dono daqueles pedidos, facilitando a navegação na hierarquia de informações
.
mostre um exemplo bem estruturado de um CRUD feito numa API Rest e explique o porque de cada elemento.

Um CRUD (Create, Read, Update, Delete) em APIs REST baseia-se em aplicar os verbos nativos e padronizados do protocolo HTTP sobre URIs que representam os recursos do sistema
. O design não deve espelhar a estrutura interna do banco de dados, mas sim focar na intenção das operações sobre as entidades de negócio
.
Abaixo, apresento um exemplo estruturado de um CRUD para um recurso de produtos (products), explicando o motivo de cada escolha arquitetural.

--------------------------------------------------------------------------------
1. CREATE (Criar)
Requisição do Cliente:

```http
POST /v1/produtos HTTP/1.1
Host: api.exemplo.com
Content-Type: application/json
```

```json
{
  "nome": "Smartphone",
  "preco": 2500.00,
  "categoria": "eletronicos"
}
```
Resposta do Servidor:

```http
HTTP/1.1 201 Created
Location: https://api.exemplo.com/v1/produtos/884
Content-Type: application/json
```

```json
{
  "id": "884",
  "nome": "Smartphone",
  "preco": 2500.00,
  "categoria": "eletronicos",
  "links": {
    "self": { "href": "https://api.exemplo.com/v1/produtos/884" }
  }
}
```
Por que foi feito assim?
Método POST: O verbo POST é o padrão para instruir o servidor a criar um novo recurso dentro de uma coleção
.
URI com Substantivo no Plural: A rota é /produtos e não /criarProduto. A arquitetura REST exige o uso de substantivos no plural para coleções e o verbo HTTP (POST) já denota a ação
.
Delegação de ID: O cliente não envia o ID. É responsabilidade do servidor aceitar os dados, criar o recurso e então gerar seu identificador único
.
Status 201 Created e Header Location: Quando a criação tem sucesso, o servidor não deve retornar apenas 200 OK. O padrão é usar o status 201 Created acompanhado do cabeçalho Location, contendo a URI exata de onde o recurso recém-criado pode ser acessado no futuro
.

--------------------------------------------------------------------------------
2. READ (Ler / Listar e Detalhar)
Requisição para Listar (Coleção):

```http
GET /v1/produtos?categoria=eletronicos&limit=10&offset=0 HTTP/1.1
Host: api.exemplo.com
Accept: application/json
```
Requisição para Detalhar (Item Específico):

```http
GET /v1/produtos/884 HTTP/1.1
Host: api.exemplo.com
Accept: application/json
```
Resposta do Servidor (Detalhar):

```http
HTTP/1.1 200 OK
Content-Type: application/json
ETag: "v1-abc1234"
```

```json
{
  "id": "884",
  "nome": "Smartphone",
  "preco": 2500.00,
  "categoria": "eletronicos"
}
```
Por que foi feito assim?
Método GET: O verbo GET deve ser usado unicamente para recuperar dados e nunca deve alterar o estado do servidor (operação segura)
.
Paginação e Filtros via Query: Na listagem de coleções, utiliza-se a query string (ex: ?limit=10&offset=0) para filtrar e paginar. Isso previne o envio de payloads gigantescos, reduzindo latência, banda e sobrecarga de processamento no banco de dados
.
Status 200 OK: Confirma que a recuperação foi bem-sucedida e que os dados estão no corpo da mensagem
.
ETag (Entity Tag): A resposta detalhada incluiu uma ETag. Trata-se de um identificador da versão atual do recurso, fundamental para implementar estratégias de cache e atualizações seguras no futuro
.

--------------------------------------------------------------------------------
3. UPDATE (Atualizar)
Para atualizações, o REST dita uma diferença clara entre substituir tudo (PUT) ou alterar parcialmente (PATCH)
.
Requisição (Atualização Completa):
PUT /v1/produtos/884 HTTP/1.1
Host: api.exemplo.com
Content-Type: application/json
If-Match: "v1-abc1234"

{
  "id": "884",
  "nome": "Smartphone",
  "preco": 2300.00,
  "categoria": "eletronicos"
}
Resposta do Servidor:
HTTP/1.1 204 No Content
ETag: "v2-def5678"
Por que foi feito assim?
Método PUT vs PATCH: O PUT sinaliza a substituição completa daquele recurso pelo objeto enviado no corpo da requisição
. Se a intenção fosse alterar apenas o preço, o correto seria usar o verbo PATCH com um formato documentado (como JSON Merge Patch)
.
Idempotência: A especificação exige que o PUT seja idempotente. Ou seja, enviar a mesma requisição para atualizar o produto para 2300.00 dez vezes resultará no mesmo estado exato no servidor
.
Cabeçalho Condicional (If-Match): Para garantir a concorrência otimista e evitar que um cliente sobreescreva a atualização de outro inadvertidamente ("lost update"), envia-se a ETag capturada anteriormente no cabeçalho If-Match. Se o dado foi alterado nesse meio tempo, o servidor bloqueia a operação retornando 412 Precondition Failed
.
Status 204 No Content: Retornou 204 para avisar ao cliente que a alteração ocorreu com sucesso, mas que, para economizar banda, o servidor intencionalmente não repetiu os dados da representação no corpo da resposta
.

--------------------------------------------------------------------------------
4. DELETE (Deletar)
Requisição:
DELETE /v1/produtos/884 HTTP/1.1
Host: api.exemplo.com
Resposta do Servidor:
HTTP/1.1 204 No Content
Por que foi feito assim?
Método DELETE: Designado nativamente no HTTP para solicitar a remoção da associação entre a URI e o recurso no servidor
.
URI Específica: O comando deve ser direcionado para o ID específico (hierarquia de item), e não para a coleção inteira (/produtos), a menos que a intenção seja perigosamente apagar todos os produtos
.
Ausência de Corpo: Uma requisição de DELETE ou a sua resposta normalmente não precisam carregar mensagens no corpo (payload)
. O status 204 No Content já informa explicitamente ao cliente que o recurso foi destruído (ou marcado como deletado via "soft-delete") e a ação foi completada
. Caso tente se deletar o recurso novamente, o servidor pode responder com 404 Not Found
.
Faça um resumo estruturado sobre APIs Rest com o conhecimento necessário para entendimento do assunto.

Um resumo estruturado sobre APIs REST exige o entendimento de conceitos arquiteturais, boas práticas de design de endpoints, comunicação via protocolo HTTP, segurança e documentação. Abaixo estão os conhecimentos fundamentais necessários para dominar o assunto:
1. O que é uma API REST? Uma API (Interface de Programação de Aplicações) atua como uma ponte que permite a comunicação e a troca de dados entre sistemas diferentes
. REST (Representational State Transfer) é um estilo arquitetural para sistemas distribuídos (como a Web), desenhado para oferecer escalabilidade, flexibilidade e independência de plataforma
.
2. Princípios Fundamentais do REST Para que uma API seja considerada verdadeiramente RESTful, ela deve seguir restrições arquiteturais específicas
:
Cliente-Servidor (Client-Server): O cliente (quem consome) e o servidor (quem provê os dados) possuem responsabilidades separadas. Isso permite que ambas as partes evoluam de forma independente
.
Sem Estado (Statelessness): Nenhuma informação de contexto (sessão) do cliente é armazenada no servidor entre as requisições. Cada requisição deve conter, por si só, todas as informações necessárias para ser compreendida e processada
.
Interface Uniforme: O uso padronizado de métodos e convenções HTTP garante que a interface seja genérica e previsível
.
Sistema em Camadas (Layered System): A arquitetura permite o uso de intermediários (como proxies, gateways e balanceadores de carga) de forma transparente para o cliente
.
Capacidade de Cache (Cacheable): As respostas devem informar implicitamente ou explicitamente se podem ser armazenadas em cache, melhorando a performance
.
3. Modelagem de Recursos e Design de URIs No REST, tudo gira em torno de recursos (entidades de negócio como clientes, pedidos, produtos).
Use Substantivos, Nunca Verbos: As URIs representam os recursos (ex: /clientes) e as ações são definidas pelos métodos HTTP. Evite rotas como /obterClientes ou /criarPedido
.
Nomenclatura no Plural: Utilize substantivos no plural para representar coleções (ex: /usuarios, /produtos)
.
Estrutura Hierárquica: Mostre os relacionamentos de forma clara nas URIs (ex: /clientes/123/pedidos para acessar os pedidos do cliente 123)
.
Evite Extensões de Arquivo: Não inclua extensões (como .json ou .xml) na URI para definir o formato, use a negociação de conteúdo via cabeçalhos HTTP
.
4. Métodos HTTP (Operações CRUD) Os verbos HTTP definem a intenção da operação sobre o recurso
:
GET (Leitura): Recupera a representação de um recurso. É uma operação segura (não altera o estado) e idempotente (fazer a requisição uma ou mil vezes gera o mesmo resultado)
.
POST (Criação): Cria um novo recurso subordinado a uma coleção (ex: um novo pedido em /pedidos). Geralmente não é idempotente
.
PUT (Substituição): Atualiza ou substitui completamente um recurso existente. Deve ser uma operação idempotente
.
PATCH (Atualização Parcial): Aplica modificações parciais a um recurso, sem precisar enviar todo o objeto novamente
.
DELETE (Remoção): Remove o recurso especificado. Também é uma operação idempotente
.
5. Códigos de Status HTTP A API deve comunicar claramente o resultado através de códigos de status HTTP apropriados
:
2xx (Sucesso): 200 OK (sucesso padrão), 201 Created (recurso criado com sucesso, retornando o link no cabeçalho Location), 204 No Content (sucesso sem corpo na resposta, comum em exclusões)
.
4xx (Erro do Cliente): 400 Bad Request (requisição malformada ou inválida), 401 Unauthorized (falha na autenticação), 403 Forbidden (sem permissão de acesso), 404 Not Found (recurso não existe) e 405 Method Not Allowed (verbo usado na rota incorreta)
.
5xx (Erro do Servidor): 500 Internal Server Error (falha inesperada na API)
.
6. Formatos, Cabeçalhos e HATEOAS
JSON: É o formato padrão e preferido na troca de mensagens em APIs REST modernas
.
Negociação de Conteúdo: Use o cabeçalho Content-Type para indicar o formato do corpo da requisição ou resposta (ex: application/json), e o Accept para o cliente dizer qual formato ele deseja receber
.
HATEOAS (Hypermedia): Uma API REST madura (Nível 3 de Richardson) deve incluir links no corpo da resposta para orientar o cliente sobre os próximos passos e ações possíveis de forma dinâmica (ex: links para navegação de "próximo" e "anterior" em listagens)
.
7. Operações de Coleção (Paginação e Filtros) Para evitar o tráfego excessivo de dados e sobrecarga do servidor, rotas de listagem não devem retornar todos os registros de uma vez.
Implemente paginação (usando parâmetros como limit e offset, ou page e size) e permita a aplicação de filtros e ordenação diretamente pela URL da requisição (ex: /produtos?categoria=eletronicos&limit=10&offset=0)
.
8. Versionamento À medida que as regras de negócio mudam, a API evolui. Implemente estratégias de versionamento (inserindo a versão principal na URI, ex: /v1/usuarios, ou no cabeçalho) para que as modificações estruturais não quebrem as integrações dos clientes existentes
.
9. Segurança e Documentação
Segurança (DevSecOps): Todo tráfego deve ser feito via HTTPS
. O padrão da indústria para autenticação e autorização em APIs é o OAuth 2.0 e tokens como JWT
. Também é essencial mitigar os riscos reportados no manual de segurança OWASP API Security Top 10, como quebra de autorização a nível de objeto, atribuição em massa (Mass Assignment), proteção contra injeções (SQL, NoSQL) e limite de taxa (Rate Limiting)
.
Documentação (OpenAPI): Documente suas APIs utilizando o padrão OpenAPI Specification (antigo Swagger). Ele permite a geração de documentação legível e testável (interativa), melhorando muito a experiência do desenvolvedor (DX) ao consumir seus serviços
.
Crie um glossário dos principais conceitos sobre API Rest

API (Application Programming Interface) O tecido conectivo no ecossistema digital que permite a troca de dados e a interoperabilidade funcional entre diferentes sistemas de software, estendendo a funcionalidade das aplicações
.
REST (Representational State Transfer) Um estilo arquitetural para sistemas distribuídos (como a web), derivado por Roy Fielding, que orienta o design de aplicações escaláveis, fracamente acopladas e eficientes através de restrições fundamentais, como interface uniforme, arquitetura cliente-servidor e comunicação stateless
. Uma API baseada nesses princípios é chamada de API RESTful
.
Recurso (Resource) O conceito fundamental e principal bloco de construção de uma API REST, representando qualquer objeto, entidade de negócio ou dado que possa ser acessado e manipulado pelo cliente
. Os recursos devem, preferencialmente, ser mapeados em quatro arquétipos: Document (recurso singular), Collection (diretório gerenciado pelo servidor), Store (repositório gerenciado pelo cliente) e Controller (conceito procedimental, semelhante a uma função)
.
URI (Uniform Resource Identifier) O identificador único global utilizado para endereçar especificamente um recurso na web
. Em boas práticas de design, as URIs de coleções devem usar substantivos no plural, indicar uma hierarquia clara de relacionamentos e evitar ativamente o uso de verbos
.
Representação (Representation) A forma codificada e estruturada — comumente em JSON ou XML — que transporta o estado passado, atual ou desejado de um recurso entre o cliente e o servidor via protocolo HTTP
. No REST, manipula-se a representação do recurso e não o recurso diretamente, permitindo que a mesma informação seja oferecida em diferentes formatos sem alterar o seu identificador (URI)
.
Statelessness (Comunicação Sem Estado) Princípio crucial da arquitetura REST onde o servidor não retém informações de contexto ou sessão do cliente entre as requisições
. Cada requisição HTTP enviada à API deve conter, em si mesma, todos os dados e o contexto necessários para ser processada, o que viabiliza a alta escalabilidade dos sistemas
.
Métodos HTTP (HTTP Verbs) Comandos padronizados do protocolo HTTP utilizados na interface uniforme para sinalizar qual ação o servidor deve realizar sobre um recurso
.
GET: Recupera a representação do estado de um recurso sem causar efeitos colaterais (operação segura)
.
POST: Cria novos recursos subordinados a uma coleção ou aciona operações não padronizadas (arquétipo Controller)
.
PUT: Atualiza ou substitui um recurso por completo; pode inserir um recurso caso a URI seja definida pelo cliente (em um Store)
.
PATCH: Aplica atualizações e modificações parciais a um recurso existente
.
DELETE: Remove permanentemente a associação de um recurso da sua coleção principal
.
Idempotência (Idempotency) Uma propriedade de determinados métodos HTTP (como PUT, DELETE e os métodos de leitura GET e HEAD) que garante que a execução repetida de uma mesma requisição resulta no mesmo estado intencional no servidor que uma única execução
. Isso é vital para criar clientes tolerantes a falhas, que possam repetir requisições em caso de problemas de rede sem causar efeitos colaterais acidentais
.
Códigos de Status HTTP (HTTP Status Codes) Códigos numéricos de três dígitos retornados no cabeçalho das respostas da API para indicar o resultado final da requisição do cliente
.
2xx (Sucesso): Informa que a operação foi bem-sucedida (ex: 200 OK, 201 Created, 204 No Content)
.
3xx (Redirecionamento): O cliente deve tomar uma ação adicional ou buscar a informação em outra URI
.
4xx (Erro do Cliente): A requisição enviada estava malformada, carecia de autenticação/permissões ou pedia um recurso inexistente (ex: 400 Bad Request, 401 Unauthorized, 404 Not Found)
.
5xx (Erro do Servidor): O servidor falhou ao processar uma requisição aparentemente válida (ex: 500 Internal Server Error)
.
HATEOAS (Hypermedia As The Engine Of Application State) Uma das restrições fundamentais do REST que determina que a representação de um recurso deve incluir links de hipermídia apontando para recursos relacionados e ações de transição de estado disponíveis
. Através desses links dinâmicos, o cliente pode navegar autonomamente pela API sem precisar codificar (hardcode) o formato ou a estrutura exata das URIs previamente
.
Negociação de Conteúdo (Content Negotiation) Mecanismo através do qual o cliente e o servidor concordam sobre a formatação (tipo de mídia) da resposta que será enviada
. O cliente indica suas preferências usando o cabeçalho HTTP Accept (ex: application/json ou application/xml), e a API responde incluindo o cabeçalho Content-Type com o formato final adotado
.
Paginação e Filtros (Pagination and Filtering) Estratégias para lidar com coleções grandes de recursos e não sobrecarregar os sistemas e a rede
. A paginação restringe o número de elementos retornados por requisição (utilizando parâmetros de busca na URI como limit, offset ou page), enquanto os filtros restringem quais registros devem vir baseados em parâmetros específicos (ex: ?categoria=eletronicos)
.
Operações de Longa Duração (LRO - Long-Running Operations) Um padrão aplicado quando o processamento de uma requisição exige um tempo considerável (geralmente mais de 1 segundo) de modo a evitar bloqueios do cliente e problemas de tolerância de carga
. Nesses casos, o servidor não retorna o resultado imediatamente, mas sim um status 202 Accepted e um "monitor de status" para que o cliente realize verificações periódicas (polling) a fim de descobrir quando a tarefa for concluída
.
OAuth 2.0 e JWT (JSON Web Tokens) Padrões cruciais para a segurança das APIs, que asseguram o controle de acesso e autenticação
. O OAuth 2.0 é o framework de delegação de autorização padrão da indústria que permite compartilhar acesso seguro entre serviços sem expor as senhas
. O JWT é amplamente utilizado como o formato do token repassado durante essa comunicação para realizar a validação dos usuários nas chamadas à API de modo stateless
.
OWASP API Security Top 10 A lista internacional mantida pelo Projeto Aberto de Segurança em Aplicações Web (OWASP) com as 10 ameaças e riscos de vulnerabilidades mais críticos direcionados a APIs
. Ela abrange fragilidades de segurança como falhas de autorização de nível de objeto (BOLA), mass assignment, exposição excessiva de dados e falta de limite de taxa (rate limiting)
.
OpenAPI Specification (OAS) Originalmente chamada de Swagger, é o formato e a especificação independente de linguagem que define e descreve formalmente o contrato de uma API REST
. Permite documentar quais rotas existem, métodos permitidos e schemas de resposta de maneira que tanto humanos quanto máquinas as compreendam, o que possibilita gerar documentos interativos e SDKs automáticos
.
CORS (Cross-Origin Resource Sharing) Um mecanismo definido pela W3C (World Wide Web Consortium) que permite que recursos restritos de uma API sejam acessados por aplicações em navegadores da web que estejam em domínios (origens) diferentes
. O CORS exige do servidor a emissão de cabeçalhos de resposta específicos autorizando explicitamente requisições cruzadas advindas do front-end
.
Transforme todo o conteúdo desse chat em um arquivo md

Aqui está todo o conteúdo e conhecimento abordado em nossa conversa estruturado em um formato Markdown (.md). Você pode copiar o bloco de código abaixo e salvá-lo como um arquivo .md (por exemplo, guia_apis_rest.md) em seu computador.
# Guia Completo de Design e Boas Práticas para APIs REST

Este documento consolida os principais tópicos abordados sobre o design, implementação e melhores práticas de APIs REST [1].

## 1. Tópicos Essenciais para Iniciantes
* **Arquitetura REST e Fundamentos:** A arquitetura baseia-se em recursos identificados por URIs, exige comunicação *stateless* (sem retenção de estado da sessão no servidor) e estabelece uma separação clara de responsabilidades entre cliente e servidor [1].
* **Design de URIs:** Deve-se utilizar substantivos no plural (ex: `/produtos` em vez de `/produto`) e organizar as URIs para demonstrar hierarquia de forma clara (ex: `/clientes/{id}/pedidos`) [2]. Evite o uso de verbos nas rotas [2].
* **Verbos HTTP:** As operações devem utilizar os métodos padronizados: `GET` (ler/recuperar), `POST` (criar), `PUT` (substituir de forma completa), `PATCH` (atualizar de forma parcial) e `DELETE` (remover) [3-7].
* **Códigos de Status HTTP:** A comunicação de resultados usa os códigos padrão: 2xx para sucesso (ex: `200 OK`, `201 Created`), 4xx para erros do cliente (ex: `400 Bad Request`, `404 Not Found`) e 5xx para erros do servidor [3, 7, 8].
* **Formatos de Dados:** O JSON é o padrão de mercado para troca de dados por ser leve e facilmente interpretado pelas linguagens de programação [9].
* **Segurança:** A proteção envolve a comunicação obrigatória via HTTPS, métodos de autenticação como OAuth 2.0 e JWT, e conhecimento das principais vulnerabilidades listadas pela OWASP [10].
* **Performance:** É vital implementar paginação e filtros via parâmetros de busca (`query parameters`) em listagens de coleções para otimizar transferência de dados [11].
* **Documentação e Versionamento:** A documentação via OpenAPI Specification é a principal referência do mercado [12]. Além disso, deve-se incluir a versão na URI (ex: `/v2/`) para permitir que a API evolua sem quebrar integrações existentes [13].

## 2. Tópicos Avançados para Desenvolvedores Experientes
* **Nível Máximo de Maturidade REST (HATEOAS):** Representa o estado da arte do REST, onde a API retorna links de hipermídia nas respostas, instruindo o cliente dinamicamente sobre quais ações e navegações são possíveis no atual estado do recurso [14, 15].
* **Operações de Longa Duração (LROs):** Para processos demorados, a API deve retornar `202 Accepted` acompanhado do cabeçalho `Operation-Location`, oferecendo um monitor de status para o cliente acompanhar o progresso via *polling* [16].
* **Resiliência e Concorrência Otimista:** Uso do cabeçalho `ETag` combinado com `If-Match` garante a concorrência otimista, impedindo que requisições concorrentes sobrescrevam dados acidentalmente ("lost update") [17, 18].
* **Performance Profunda:** Otimização via retorno de respostas parciais (ex: usar `?fields=nome,status` para reduzir o payload de retorno) e operações em lote [19].
* **Segurança Profunda e Monitoramento:** Mitigação de falhas complexas como o BOLA (Broken Object Level Authorization) e o *Mass Assignment* (quando clientes enviam campos não previstos no JSON e o sistema os injeta indevidamente) [20, 21]. O uso de cabeçalhos de rastreamento distribuído também é essencial em ecossistemas de microsserviços.

## 3. Retorno e Tratamento de Erros
A melhor prática é utilizar os códigos HTTP padronizados em conjunto com um corpo estruturado de erro que identifique o problema sem expor detalhes sensíveis ou falhas de infraestrutura.
* **400 Bad Request:** Dados inválidos ou erro de validação/sintaxe [4].
* **401 Unauthorized:** Falta de autenticação ou credenciais inválidas.
* **403 Forbidden:** O cliente está autenticado, mas não possui autorização (permissão) para aquela ação.
* **404 Not Found:** A URI ou o recurso não foi encontrado [3].
* **405 Method Not Allowed:** O verbo utilizado não é suportado pelo endpoint.
* **406 Not Acceptable:** O formato exigido pelo cliente no cabeçalho `Accept` não é suportado pela API [22].
* **409 Conflict:** O estado atual do recurso entra em conflito com a requisição [8].
* **412 Precondition Failed:** Falha de pré-condição, utilizado ao barrar concorrência via ETags desatualizadas [23].
* **415 Unsupported Media Type:** O payload de entrada possui um tipo de mídia não aceito [7].
* **500 Internal Server Error:** Falha ou exceção não tratada originada no servidor.

## 4. Cabeçalhos (Headers) Mais Utilizados
Em APIs REST, a intenção, os formatos e metadados viajam nos cabeçalhos nativos do HTTP em vez de serem "envelopados" no corpo da mensagem, se diferenciando do padrão SOAP [24].
* **Negociação de Conteúdo:** `Content-Type` (o que a API ou cliente está enviando) e `Accept` (o formato esperado de retorno) [22, 24].
* **Segurança e Identidade:** `Authorization` para credenciais e tokens.
* **Criação:** `Location`, que retorna a URI exata do novo recurso recém-criado em operações POST [4].
* **Cache:** `Cache-Control` (tempo de expiração), `ETag` (identificador da versão do estado atual), `If-Match` e `If-None-Match` (cabeçalhos condicionais) [25, 26].

## 5. Exemplo de API Bem Estruturada e HATEOAS
**Requisição (Busca paginada):**
```http
GET /v1/clientes/1234/pedidos?limit=10&offset=0 HTTP/1.1
Host: api.exemplo.com
Accept: application/json
Resposta:
HTTP/1.1 200 OK
Content-Type: application/json
Cache-Control: max-age=60, must-revalidate
ETag: "83921abc"

{
  "dados": [
    { "id": "987", "total": 150.50, "status": "enviado" },
    { "id": "988", "total": 20.00, "status": "pendente" }
  ],
  "metadados": {
    "total": 45,
    "limit": 10,
    "offset": 0
  },
  "links": {
    "self": { "href": "https://api.exemplo.com/v1/clientes/1234/pedidos?limit=10&offset=0" },
    "next": { "href": "https://api.exemplo.com/v1/clientes/1234/pedidos?limit=10&offset=10" }
  }
}
6. Estrutura Padrão de um CRUD (Operações Essenciais)
CREATE (Criar): POST /v1/produtos -> A API recebe o payload, gera o ID internamente, e deve responder com 201 Created e o cabeçalho Location apontando para a URI gerada
.
READ (Ler): GET /v1/produtos (busca paginada da coleção inteira) ou GET /v1/produtos/{id} (detalhamento do item). Responde com 200 OK
.
UPDATE (Atualizar): PUT /v1/produtos/{id} (substituição integral) ou PATCH /v1/produtos/{id} (atualização parcial, economizando banda de rede). A resposta costuma ser 204 No Content ou 200 OK
.
DELETE (Deletar): DELETE /v1/produtos/{id} -> Remove o recurso. A resposta deve ser 204 No Content indicando sucesso sem conteúdo a ser retornado
.
7. Glossário de Conceitos REST
API: O tecido conectivo do ecossistema digital que permite que diferentes sistemas conversem de forma interoperável.
REST (Representational State Transfer): Estilo arquitetural para sistemas em rede distribuídos com foco na flexibilidade, simplicidade e escalabilidade
.
Recurso: A abstração fundamental das informações numa API REST. Pode ser uma coleção ou um documento singular.
URI: Identificador Uniforme de Recurso; é o endereço global e único onde o recurso se encontra
.
Statelessness: Princípio onde cada requisição deve carregar todas as informações contextuais necessárias, sem depender da retenção de estado de sessões no servidor
.
Idempotência: Característica de métodos (como PUT, DELETE e GET) em que aplicar a mesma requisição repetidas vezes resultará no mesmo estado final
.
HATEOAS: Restrição de arquitetura onde a representação dos recursos inclui hiperlinks navegáveis para guiar a transição de estado da aplicação do cliente
.
OpenAPI Specification (OAS): Linguagem padronizada e legível por humanos e máquinas que descreve detalhadamente contratos, rotas e propriedades estruturais de uma API