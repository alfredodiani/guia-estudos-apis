## Glossário

- API (Application Programming Interface): O tecido conectivo no ecossistema digital que permite a troca de dados e a interoperabilidade funcional entre diferentes sistemas de software, estendendo a funcionalidade das aplicações.

- REST (Representational State Transfer): Um estilo arquitetural para sistemas distribuídos (como a web), derivado por Roy Fielding, que orienta o design de aplicações escaláveis, fracamente acopladas e eficientes através de restrições fundamentais, como interface uniforme, arquitetura cliente-servidor e comunicação stateless. Uma API baseada nesses princípios é chamada de API RESTful.

- Recurso (Resource): O conceito fundamental e principal bloco de construção de uma API REST, representando qualquer objeto, entidade de negocio ou dado que possa ser acessado e manipulado pelo cliente. Os recursos devem, preferencialmente, ser mapeados em quatro arquétipos: Document (recurso singular), Collection (diretório gerenciado pelo servidor), Store (repositório gerenciado pelo cliente) e Controller (conceito procedimental, semelhante a uma função).

- URI (Uniform Resource Identifier): O identificador único global utilizado para endereçar especificamente um recurso na web. Em boas práticas de design, as URIs de coleções devem usar substantivos no plural, indicar uma hierarquia clara de relacionamentos e evitar ativamente o uso de verbos.

- Representação (Representation): A forma codificada e estruturada — comumente em JSON ou XML — que transporta o estado passado, atual ou desejado de um recurso entre o cliente e o servidor via protocolo HTTP. No REST, manipula-se a representação do recurso e não o recurso diretamente, permitindo que a mesma informação seja oferecida em diferentes formatos sem alterar o seu identificador (URI).

- Statelessness (Comunicação Sem Estado): Princípio crucial da arquitetura REST onde o servidor não retém informações de contexto ou sessão do cliente entre as requisições. Cada requisição HTTP enviada à API deve conter, em si mesma, todos os dados e o contexto necessários para ser processada, o que viabiliza a alta escalabilidade dos sistemas.

- Métodos HTTP (HTTP Verbs): Comandos padronizados do protocolo HTTP utilizados na interface uniforme para sinalizar qual ação o servidor deve realizar sobre um recurso.

	- GET: Recupera a representação do estado de um recurso sem causar efeitos colaterais (operação segura).

	- POST: Cria novos recursos subordinados a uma coleção ou aciona operações não padronizadas (arquétipo Controller).

	- PUT: Atualiza ou substitui um recurso por completo; pode inserir um recurso caso a URI seja definida pelo cliente (em um Store).

	- PATCH: Aplica atualizações e modificações parciais a um recurso existente.

	- DELETE: Remove permanentemente a associação de um recurso da sua coleção principal.

- Idempotência (Idempotency): Uma propriedade de determinados métodos HTTP (como PUT, DELETE e os métodos de leitura GET e HEAD) que garante que a execução repetida de uma mesma requisição resulta no mesmo estado final no servidor que uma única execução. Isso é vital para criar clientes tolerantes a falhas, que possam repetir requisições em caso de problemas de rede sem causar efeitos colaterais acidentais.

- Códigos de Status HTTP (HTTP Status Codes): Códigos numéricos de três dígitos retornados no cabeçalho das respostas da API para indicar o resultado final da requisição do cliente.

	- 2xx (Sucesso): Informa que a operação foi bem-sucedida (ex: 200 OK, 201 Created, 204 No Content).

	- 3xx (Redirecionamento): O cliente deve tomar uma ação adicional ou buscar a informação em outra URI.

	- 4xx (Erro do Cliente): A requisição enviada estava malformada, carecia de autenticação/permissões ou pedia um recurso inexistente (ex: 400 Bad Request, 401 Unauthorized, 404 Not Found).

	- 5xx (Erro do Servidor): O servidor falhou ao processar uma requisição aparentemente válida (ex: 500 Internal Server Error).

- HATEOAS (Hypermedia As The Engine Of Application State): Uma das restrições fundamentais do REST que determina que a representação de um recurso deve incluir links de hipermídia apontando para recursos relacionados e ações de transição de estado disponíveis. Através desses links dinâmicos, o cliente pode navegar autonomamente pela API sem precisar codificar (hardcode) o formato ou a estrutura exata das URIs previamente.

- Negociação de Conteúdo (Content Negotiation): Mecanismo através do qual o cliente e o servidor concordam sobre a formatação (tipo de mídia) da resposta que será enviada. O cliente indica suas preferências usando o cabeçalho HTTP Accept (ex: application/json ou application/xml), e a API responde incluindo o cabeçalho Content-Type com o formato final adotado.

- Paginação e Filtros (Pagination and Filtering): Estratégias para lidar com coleções grandes de recursos e não sobrecarregar os sistemas e a rede. A paginação restringe o número de elementos retornados por requisição (utilizando parâmetros de busca na URI como limit, offset ou page), enquanto os filtros restringem quais registros devem vir baseados em parâmetros específicos (ex: ?categoria=eletronicos).

- Operações de Longa Duração (LRO - Long-Running Operations): Um padrão aplicado quando o processamento de uma requisição exige um tempo considerável (geralmente mais de 1 segundo) de modo a evitar bloqueios do cliente e problemas de tolerância de carga. Nesses casos, o servidor não retorna o resultado imediatamente, mas sim um status 202 Accepted e um "monitor de status" para que o cliente realize verificações periódicas (polling) a fim de descobrir quando a tarefa for concluída.

- OAuth 2.0 e JWT (JSON Web Tokens): Padrões cruciais para a segurança das APIs, que asseguram o controle de acesso e autenticação. O OAuth 2.0 é o framework de delegação de autorização padrão da indústria que permite compartilhar acesso seguro entre serviços sem expor as senhas. O JWT é amplamente utilizado como o formato do token repassado durante essa comunicação para realizar a validação dos usuários nas chamadas à API de modo stateless.

- OWASP API Security Top 10: A lista internacional mantida pelo Projeto Aberto de Segurança em Aplicações Web (OWASP) com as 10 ameaças e riscos de vulnerabilidades mais críticos direcionados a APIs. Ela abrange fragilidades de segurança como falhas de autorização de nível de objeto (BOLA), mass assignment, exposição excessiva de dados e falta de limite de taxa (rate limiting).

- OpenAPI Specification (OAS): Originalmente chamada de Swagger, é o formato e a especificação independente de linguagem que define e descreve formalmente o contrato de uma API REST. Permite documentar quais rotas existem, métodos permitidos e schemas de resposta de maneira que tanto humanos quanto máquinas as compreendam, o que possibilita gerar documentos interativos e SDKs automáticos.

- CORS (Cross-Origin Resource Sharing): Um mecanismo definido pela W3C (World Wide Web Consortium) que permite que recursos restritos de uma API sejam acessados por aplicações em navegadores da web que estejam em domínios (origens) diferentes. O CORS exige do servidor a emissão de cabeçalhos de resposta específicos autorizando explicitamente requisições cruzadas advindas do front-end.
