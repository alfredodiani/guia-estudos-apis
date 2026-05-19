Um resumo estruturado sobre APIs REST exige o entendimento de conceitos arquiteturais, boas práticas de design de endpoints, comunicação via protocolo HTTP, segurança e documentação. Abaixo estão os conhecimentos fundamentais necessários para dominar o assunto:

## 1. O que é uma API REST?

- Uma API (Interface de Programação de Aplicações) atua como uma ponte que permite a comunicação e a troca de dados entre sistemas diferentes. REST (Representational State Transfer) é um estilo arquitetural para sistemas distribuídos (como a Web), desenhado para oferecer escalabilidade, flexibilidade e independência de plataforma.

## 2. Princípios Fundamentais do REST

- Para que uma API seja considerada verdadeiramente RESTful, ela deve seguir restrições arquiteturais específicas:

+- **Cliente-Servidor (Client-Server):** O cliente (quem consome) e o servidor (quem provê os dados) possuem responsabilidades separadas. Isso permite que ambas as partes evoluam de forma independente.

+- **Sem Estado (Statelessness):** Nenhuma informação de contexto (sessão) do cliente é armazenada no servidor entre as requisições. Cada requisição deve conter, por si só, todas as informações necessárias para ser compreendida e processada.

+- **Interface Uniforme:** O uso padronizado de métodos e convenções HTTP garante que a interface seja genérica e previsível.

+- **Sistema em Camadas (Layered System):** A arquitetura permite o uso de intermediários (como proxies, gateways e balanceadores de carga) de forma transparente para o cliente.

+- **Capacidade de Cache (Cacheable):** As respostas devem informar implicitamente ou explicitamente se podem ser armazenadas em cache, melhorando a performance.

## 3. Modelagem de Recursos e Design de URIs

- No REST, tudo gira em torno de recursos (entidades de negocio como clientes, pedidos, produtos).

- **Use Substantivos, Nunca Verbos:** As URIs representam os recursos (ex: /clientes) e as ações são definidas pelos métodos HTTP. Evite rotas como /obterClientes ou /criarPedido.

- **Nomenclatura no Plural:** Utilize substantivos no plural para representar coleções (ex: /usuarios, /produtos).

- **Estrutura Hierárquica:** Mostre os relacionamentos de forma clara nas URIs (ex: /clientes/123/pedidos para acessar os pedidos do cliente 123).

- **Evite Extensões de Arquivo:** Não inclua extensões (como .json ou .xml) na URI para definir o formato, use a negociação de conteúdo via cabeçalhos HTTP.

## 4. Métodos HTTP (Operações CRUD)

- Os verbos HTTP definem a intenção da operação sobre o recurso:

 - **GET (Leitura):** Recupera a representação de um recurso. É uma operação segura (não altera o estado) e idempotente (fazer a requisição uma ou mil vezes gera o mesmo resultado).

 - **POST (Criação):** Cria um novo recurso subordinado a uma coleção (ex: um novo pedido em /pedidos). Geralmente não é idempotente.

 - **PUT (Substituição):** Atualiza ou substitui completamente um recurso existente. Deve ser uma operação idempotente.

 - **PATCH (Atualização Parcial):** Aplica modificações parciais a um recurso, sem precisar enviar todo o objeto novamente.

 - **DELETE (Remoção):** Remove o recurso especificado. Também é uma operação idempotente.

## 5. Códigos de Status HTTP

- A API deve comunicar claramente o resultado através de códigos de status HTTP apropriados:

 - **2xx (Sucesso):** 200 OK (sucesso padrão), 201 Created (recurso criado com sucesso, retornando o link no cabeçalho Location), 204 No Content (sucesso sem corpo na resposta, comum em exclusões).

 - **4xx (Erro do Cliente):** 400 Bad Request (requisição malformada ou inválida), 401 Unauthorized (falha na autenticação), 403 Forbidden (sem permissão de acesso), 404 Not Found (recurso não existe) e 405 Method Not Allowed (verbo usado na rota incorreta).

 - **5xx (Erro do Servidor):** 500 Internal Server Error (falha inesperada na API).

## 6. Formatos, Cabeçalhos e HATEOAS

- **JSON:** É o formato padrão e preferido na troca de mensagens em APIs REST modernas.

- **Negociação de Conteúdo:** Use o cabeçalho Content-Type para indicar o formato do corpo da requisição ou resposta (ex: application/json), e o Accept para o cliente dizer qual formato ele deseja receber.

**Exemplo de uso de cabeçalhos:**

```http
POST /v1/produtos HTTP/1.1
Host: api.exemplo.com
Content-Type: application/json
Accept: application/json
```

- **HATEOAS (Hypermedia):** Uma API REST madura (Nível 3 de Richardson) deve incluir links no corpo da resposta para orientar o cliente sobre os próximos passos e ações possíveis de forma dinâmica (ex: links para navegação de "próximo" e "anterior" em listagens).

## 7. Operações de Coleção (Paginação e Filtros)

- Para evitar o tráfego excessivo de dados e sobrecarga do servidor, rotas de listagem não devem retornar todos os registros de uma vez.

- Implemente paginação (usando parâmetros como limit e offset, ou page e size) e permita a aplicação de filtros e ordenação diretamente pela URL da requisição (ex: /produtos?categoria=eletronicos&limit=10&offset=0).

## 8. Versionamento

- **Versionamento:** À medida que as regras de negocio mudam, a API evolui. Implemente estratégias de versionamento (inserindo a versão principal na URI, ex: /v1/usuarios, ou no cabeçalho) para que as modificações estruturais não quebrem as integrações dos clientes existentes.

## 9. Segurança e Documentação

- **Segurança (DevSecOps):** Todo tráfego deve ser feito via HTTPS. O padrão da indústria para autenticação e autorização em APIs é o OAuth 2.0 e tokens como JWT. Também é essencial mitigar os riscos reportados no manual de segurança OWASP API Security Top 10, como quebra de autorização a nível de objeto, atribuição em massa (Mass Assignment), proteção contra injeções (SQL, NoSQL) e limite de taxa (Rate Limiting).

- **Documentação (OpenAPI):** Documente suas APIs utilizando o padrão OpenAPI Specification (antigo Swagger). Ele permite gerar documentação legível e testável (interativa), melhorando muito a experiência do desenvolvedor (DX) ao consumir seus serviços.
