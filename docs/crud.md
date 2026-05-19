Um CRUD (Create, Read, Update, Delete) em APIs REST baseia-se em aplicar os verbos nativos e padronizados do protocolo HTTP sobre URIs que representam os recursos do sistema. O design não deve espelhar a estrutura interna do banco de dados, mas sim focar na intenção das operações sobre as entidades de negocio.

Abaixo, apresento um exemplo estruturado de um CRUD para um recurso de produtos (products), explicando o motivo de cada escolha arquitetural.

## 1. CREATE (Criar)

- Requisição do Cliente:

POST /v1/produtos HTTP/1.1
Host: api.exemplo.com
Content-Type: application/json

{
  "nome": "Smartphone",
  "preco": 2500.00,
  "categoria": "eletronicos"
}

- Resposta do Servidor:

HTTP/1.1 201 Created
Location: https://api.exemplo.com/v1/produtos/884
Content-Type: application/json

{
  "id": "884",
  "nome": "Smartphone",
  "preco": 2500.00,
  "categoria": "eletronicos",
  "links": {
    "self": { "href": "https://api.exemplo.com/v1/produtos/884" }
  }
}

- Por que foi feito assim?

- Método POST: O verbo POST é o padrão para instruir o servidor a criar um novo recurso dentro de uma coleção.

- URI com Substantivo no Plural: A rota é /produtos e não /criarProduto. A arquitetura REST exige o uso de substantivos no plural para coleções e o verbo HTTP (POST) já denota a ação.

- Delegação de ID: O cliente não envia o ID. É responsabilidade do servidor aceitar os dados, criar o recurso e então gerar seu identificador único.

- Status 201 Created e Header Location: Quando a criação tem sucesso, o servidor não deve retornar apenas 200 OK. O padrão é usar o status 201 Created acompanhado do cabeçalho Location, contendo a URI exata de onde o recurso recém-criado pode ser acessado no futuro.

## 2. READ (Ler / Listar e Detalhar)

- Requisição para Listar (Coleção):

GET /v1/produtos?categoria=eletronicos&limit=10&offset=0 HTTP/1.1
Host: api.exemplo.com
Accept: application/json

- Requisição para Detalhar (Item Específico):

GET /v1/produtos/884 HTTP/1.1
Host: api.exemplo.com
Accept: application/json

- Resposta do Servidor (Detalhar):

HTTP/1.1 200 OK
Content-Type: application/json
ETag: "v1-abc1234"

{
  "id": "884",
  "nome": "Smartphone",
  "preco": 2500.00,
  "categoria": "eletronicos"
}

- Por que foi feito assim?

- Método GET: O verbo GET deve ser usado unicamente para recuperar dados e nunca deve alterar o estado do servidor (operação segura).

- Paginação e Filtros via Query: Na listagem de coleções, utiliza-se a query string (ex: ?limit=10&offset=0) para filtrar e paginar. Isso previne o envio de payloads gigantescos, reduzindo latencia, banda e sobrecarga de processamento no banco de dados.

- Status 200 OK: Confirma que a recuperação foi bem-sucedida e que os dados estão no corpo da mensagem.

- ETag (Entity Tag): A resposta detalhada incluiu uma ETag. Trata-se de um identificador da versão atual do recurso, fundamental para implementar estratégias de cache e atualizações seguras no futuro.

## 3. UPDATE (Atualizar)

- Para atualizações, o REST dita uma diferença clara entre substituir tudo (PUT) ou alterar parcialmente (PATCH).

- Requisição (Atualização Completa):

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

- Resposta do Servidor:

HTTP/1.1 204 No Content
ETag: "v2-def5678"

- Por que foi feito assim?

- Método PUT vs PATCH: O PUT sinaliza a substituição completa daquele recurso pelo objeto enviado no corpo da requisição.

- Idempotência: A especificação exige que o PUT seja idempotente. Ou seja, enviar a mesma requisição para atualizar o produto para 2300.00 dez vezes resultará no mesmo estado exato no servidor.

- Cabeçalho Condicional (If-Match): Para garantir a concorrência otimista e evitar que um cliente sobreescreva a atualização de outro inadvertidamente ("lost update"), envia-se a ETag capturada anteriormente no cabeçalho If-Match. Se o dado foi alterado nesse meio tempo, o servidor bloqueia a operação retornando 412 Precondition Failed.

- Status 204 No Content: Retornou 204 para avisar ao cliente que a alteração ocorreu com sucesso, mas que, para economizar banda, o servidor intencionalmente não repetiu os dados da representação no corpo da resposta.

## 4. DELETE (Deletar)

- Requisição:

DELETE /v1/produtos/884 HTTP/1.1
Host: api.exemplo.com

- Resposta do Servidor:

HTTP/1.1 204 No Content

- Por que foi feito assim?

- Método DELETE: Designado nativamente no HTTP para solicitar a remoção da associação entre a URI e o recurso no servidor.

- URI Específica: O comando deve ser direcionado para o ID específico (hierarquia de item), e não para a coleção inteira (/produtos), a menos que a intenção seja perigosamente apagar todos os produtos.

- Ausência de Corpo: Uma requisição de DELETE ou a sua resposta normalmente não precisam carregar mensagens no corpo (payload). O status 204 No Content já informa explicitamente ao cliente que o recurso foi destruído (ou marcado como deletado via "soft-delete") e a ação foi completada.
