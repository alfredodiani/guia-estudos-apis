Para ilustrar a aplicação prática das melhores arquiteturas e padrões REST, usaremos o cenário de uma API de e-commerce na qual um cliente (aplicação) deseja buscar a lista de pedidos de um usuário específico. Aqui está um exemplo de uma requisição e resposta de uma API REST bem estruturada:

## 1. A Requisição (Request)

```http
GET /v1/clientes/1234/pedidos?limit=10&offset=0 HTTP/1.1
Host: api.exemplo.com
Accept: application/json
```

Detalhes da Requisição:

- **Método HTTP (GET):** Utilizado corretamente para recuperar dados sem causar efeitos colaterais (operações seguras e idempotentes).

- Versionamento (/v1/): A inclusão da versão principal (major version) diretamente na URI é uma prática recomendada para garantir que futuras atualizações com quebras de contrato (breaking changes) não afetem os clientes atuais.

- Nomenclatura com Substantivos no Plural (/clientes, /pedidos): A URI utiliza substantivos (nunca verbos) no plural para identificar coleções de recursos. Isso mantém a API intuitiva e focada nos recursos.

- Hierarquia de Recursos (/clientes/1234/pedidos): Demonstra claramente o relacionamento pai-filho. O cliente está buscando a coleção de pedidos que pertence unicamente ao cliente de ID 1234.

- Paginação (?limit=10&offset=0): Como um cliente pode ter milhares de pedidos, o uso de parâmetros de query previne a sobrecarga do servidor e do banco de dados. O limit restringe a quantidade de itens a 10, e o offset indica de onde a contagem deve começar (neste caso, do início).

- **Negociação de Conteúdo (Accept: application/json):** O cabeçalho **Accept** especifica que o cliente espera que a resposta seja formatada em JSON.

## 2. A Resposta (Response)

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

- **Código de Status (200 OK):** Comunica claramente que a operação foi bem-sucedida e que o conteúdo solicitado está no corpo da resposta.

- **Cabeçalhos de Cache (Cache-Control e ETag):** O **Cache-Control** instrui o cliente de que ele pode guardar esta resposta em cache por 60 segundos. O **ETag** ("Entity Tag") é um identificador opaco do estado atual desta representação, permitindo requisições condicionais futuras mais eficientes.

- Separação de Dados e Metadados: O payload JSON separa claramente a carga principal (dados) dos dados de apoio, como a estrutura de paginação (metadados). Os metadados ajudam o cliente a saber que existem 45 pedidos no total, permitindo desenhar corretamente a interface do usuário (ex: botões de "próxima página").

- **HATEOAS / Links (Hipermídia):** Este é o grande diferencial de uma API REST de alto nível (Nível 3 de Maturidade). O bloco links permite a descoberta dinâmica da API, entregando ao cliente as URIs exatas para navegar no estado da aplicação.

- **self:** Aponta para a própria URI que gerou essa resposta, garantindo contexto e rastreabilidade.

- **next:** Como há paginação (retornou apenas 2 de 45 registros), o servidor já entrega a URI pronta para buscar a próxima página (offset=10), o que economiza esforços no cliente.

- **cliente / parent:** Fornece a rota exata para obter os detalhes do dono daqueles pedidos, facilitando a navegação na hierarquia de informações.