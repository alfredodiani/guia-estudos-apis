Uma maneira simples e efetiva de retornar erros em APIs é combinar o uso de códigos de status HTTP padronizados para sinalizar o resultado da operação com um formato estruturado e consistente na resposta (geralmente um payload JSON). O corpo da resposta de erro deve sempre apresentar, no mínimo, um código identificador do erro (id ou code) e uma mensagem clara e descritiva do problema.

É crucial que os erros sejam consistentes em toda a API e que não exponham detalhes internos sensíveis de implementação ou stack traces ao cliente.

## Casos comuns de erros e como retornar

## 1. 400 Bad Request (Requisição Inválida)

- Quando ocorre: O cliente enviou dados inválidos, ocorreu um erro de sintaxe na requisição, ou a requisição falhou nas regras de validação.

- Como retornar: Responda com o status 400 acompanhado de um corpo (payload) que detalhe claramente quais dados ou propriedades do JSON estavam incorretos ou faltantes.

## 2. 401 Unauthorized (Não Autorizado)

- Quando ocorre: O cliente tentou acessar um recurso protegido sem fornecer as credenciais corretas ou omitiu a autenticação.

- Como retornar: Retorne o status 401 e inclua o cabeçalho WWW-Authenticate listando pelo menos um desafio de autenticação aplicável (como indicar que um token OAuth2/Bearer é necessário).

## 3. 403 Forbidden (Proibido)

- Quando ocorre: O cliente está devidamente autenticado e a requisição é válida, mas ele não possui permissão no nível de aplicação (falha de autorização) para realizar aquela ação.

- Como retornar: Retorne o status 403. Diferente do erro 401, o cliente não deve ser instruído a repetir a requisição com as mesmas credenciais. Uma alternativa aceita em alguns cenários de segurança restrita é retornar 404 Not Found para sequer confirmar a existência do recurso bloqueado.

## 4. 404 Not Found (Não Encontrado)

- Quando ocorre: A URI solicitada pelo cliente não pôde ser mapeada para nenhum recurso existente.

- Como retornar: Retorne o status 404 simples indicando a falha de localização.

## 5. 405 Method Not Allowed (Método Não Permitido)

- Quando ocorre: A URI de destino existe, mas o verbo HTTP utilizado pelo cliente (por exemplo, tentar um POST num endpoint somente-leitura) não é suportado pela rota.

- Como retornar: Retorne o status 405 e obrigatoriamente inclua o cabeçalho Allow na resposta listando quais métodos são aceitos por aquele recurso (ex: Allow: GET, PUT, DELETE).

## 6. 406 Not Acceptable (Não Aceitável)

- Quando ocorre: A API não consegue formatar os dados no tipo de mídia exigido pelo cliente no cabeçalho de requisição Accept (por exemplo, o cliente exige application/xml e a API só opera com application/json).

- Como retornar: Retorne o status 406 informando que o formato solicitado não pode ser servido.

## 7. 409 Conflict (Conflito)

- Quando ocorre: A requisição não pode ser concluída pois causa um conflito com o estado atual do recurso no servidor. Exemplos incluem violações de concorrência ou tentar deletar um recurso raiz que ainda contém recursos filhos ou pedidos pendentes.

- Como retornar: Retorne o status 409 acompanhado de um corpo que contenha informações suficientes para o cliente entender a origem do conflito e ter a chance de tentar resolvê-lo.

## 8. 412 Precondition Failed (Falha de Pré-condição)

- Quando ocorre: Fundamental em operações de atualização concorrentes. O cliente faz uma requisição condicional fornecendo cabeçalhos como If-Match (com uma Entity Tag ou ETag), mas a versão enviada não corresponde à versão atual dos dados no servidor.

- Como retornar: Retorne o status 412. Isso barra a operação de salvar para que o cliente não sobrescreva dados que outro usuário os tiver modificado em paralelo.

## 9. 415 Unsupported Media Type (Tipo de Mídia Não Suportado)

- Quando ocorre: O formato do payload inserido pelo cliente no corpo da requisição não é compreendido pela API. Acontece quando o cabeçalho Content-Type não condiz com as expectativas do serviço (como enviar XML para uma rota exclusiva de JSON).

- Como retornar: Retorne o status 415. Adicionalmente, pode-se usar o cabeçalho de resposta Accept ou documentar no corpo da mensagem quais são as mídias suportadas por aquela rota.

## 10. 500 Internal Server Error (Erro Interno do Servidor)

- Quando ocorre: Ocorreu um mau funcionamento, exceção não tratada na aplicação ou falha na infraestrutura/banco de dados. É um erro que pertence exclusivamente à responsabilidade do servidor.

- Como retornar: Retorne o status 500 com uma mensagem padronizada no corpo JSON. Capture as exceções completas e detalhadas (stack traces e componentes técnicos) e grave-as apenas em logs internos, relacionando-as usando um cabeçalho identificador como x-ms-request-id.