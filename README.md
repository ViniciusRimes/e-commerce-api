# Sumário

1. [Descrição](#descrição)
2. [Funcionalidades](#funcionalidades)
3. [Endpoints](#endpoints)
    - [Empresas](#empresas)
    - [Usuários](#usuários)
    - [Produtos](#produtos)
    - [Endereços](#endereços)
    - [Avaliações](#avaliações)
    - [Perguntas](#perguntas)
    - [Respostas](#respostas)
    - [Carrinho de Produtos](#carrinho-de-produtos)
4. [Autenticação](#autenticação)
    - [Obtendo um Token JWT](#obtendo-um-token-jwt)
    - [Usando o Token JWT](#usando-o-token-jwt)
    - [Expiração do Token](#expiração-do-token)
    - [Exemplo de Solicitação Autenticada](#exemplo-de-solicitação-autenticada)
    - [Possíveis Erros de Autenticação](#possíveis-erros-de-autenticação)
5. [Pré-requisitos](#pré-requisitos)
6. [Instalação](#instalação)
7. [Contato](#contato)

# E-commerce API

## Descrição
A E-commerce API é um sistema desenvolvido para lojas virtuais, inspirado em plataformas como Amazon, Americanas, KABUM, entre outras. A API oferece diversas funcionalidades relacionadas a produtos e empresas.

## Funcionalidades
### Cadastro e Login de Empresas e Usuários
- Autenticação segura utilizando a biblioteca JSON Web Token (JWT).

### Empresas
## Endpoint `POST /enterprise/register`

### Descrição
Este endpoint é responsável por cadastrar uma nova empresa no sistema.

### Parâmetros necessários

1. **name** (*string*): Nome da empresa.
2. **email** (*string*): Endereço de e-mail da empresa.
3. **password** (*string*): Senha para a conta da empresa.
4. **confirmPassword** (*string*): Confirmação da senha, deve coincidir com o campo de senha.
5. **phone** (*string*): Número de telefone da empresa.

### Exemplo de solicitação (JSON)

```json
{
  "name": "Nome da Empresa",
  "email": "empresa@email.com",
  "password": "senha123",
  "confirmPassword": "senha123",
  "phone": "123456789"
}
```
### Respostas
200 OK: O cadastro foi realizado com sucesso. Retorna informações adicionais, como o ID da empresa.
{
  "message": "Empresa cadastrada!"
}
400 Bad Request: Erro nos parâmetros fornecidos. Pode incluir uma mensagem de erro para indicar o problema específico. Exemplo:
{
  "message": "Já existe um usuário cadastrado com este email, tente novamente."
}
500 Internal Server Error: Erro interno no servidor. Retorna uma mensagem de erro genérica.
{
    "message": "Erro em processar a sua solicitação",
    "error": "Detalhes do erro"
}
## Endpoint `POST /enterprise/login`

### Descrição
Este endpoint realiza o login de uma empresa existente no sistema.

### Parâmetros necessários

1. **email** (*string*): Endereço de e-mail da empresa.
2. **password** (*string*): Senha associada à conta da empresa.

### Exemplo de solicitação (JSON)

```json
{
  "email": "empresa@email.com",
  "password": "senha123"
}
```
### Respostas
200 OK: O login foi realizado com sucesso. Retorna informações adicionais, como o token da empresa.
{
  "message": "Empresa logada!",
  "token": "Token gerado"
}
400 Bad Request: Erro nos parâmetros fornecidos. Pode incluir uma mensagem de erro para indicar o problema específico. Exemplo:
{
  "message": "Nenhum usuário está cadastrado com este email, tente novamente."
}
500 Internal Server Error: Erro interno no servidor. Retorna uma mensagem de erro genérica.
{
    "message": "Erro em processar a sua solicitação",
    "error": "Detalhes do erro"
}
## Endpoint `DELETE /enterprise/delete`

### Descrição
Este endpoint deleta a empresa cadastrada com base no token de autenticação fornecido. É necessário também fornecer a senha da empresa para confirmação.

### Parâmetros necessários

1. **Token** (*string*): Token de autenticação da empresa.
2. **Senha da Empresa** (*string*): Senha atual da empresa para confirmar a exclusão da conta.

### Exemplo de solicitação

```http
DELETE /enterprise/delete
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
Content-Type: application/json
```
```json
{
  "password": "senha123"
}
```
### Respostas
200 OK: A empresa foi deletada com sucesso.  
{
  "message": "Empresa excluída!"
}

400 Bad Request: Erro nos parâmetros fornecidos. Pode incluir uma mensagem de erro para indicar o problema específico.  
{
  "message": "Senha incorreta."
}

401 Unauthorized: Token inválido ou ausente, ou senha incorreta. Indica que o token fornecido não é válido, não foi enviado ou a senha fornecida não confere.  
{
  "message": "Token não encontrado ou inválido!"
}

500 Internal Server Error: Erro interno no servidor. Retorna uma mensagem de erro genérica.  
{
  "message": "Erro em processar a sua solicitação",
  "error": "Detalhes do erro"
}


### Usuários
- `POST /user/register`: Cadastra um novo usuário.
- `POST /user/login`: Realiza o login de um usuário existente.
- `GET /user/getuser`: Retorna informações do usuário com base no token.
- `PATCH /user/edituser`: Atualiza informações do usuário usando o token.
- `DELETE /enterprise/delete`: Deleta o usuário cadastrado.

### Produtos
- `POST /product/create`: Cadastra um novo produto.
- `DELETE /product/delete/:productId`: Deleta um produto com base no ID.
- `PATCH /product/edit/:productId`: Edita um produto com base no ID.
- `GET /product/all`: Retorna todos os produtos cadastrados.
- `GET /product/get/:productId`: Retorna um produto com base no ID.
- `POST /product/addcart/:productId`: Adiciona um produto ao carrinho com base no ID.
- `GET /product/products?offer`: Retorna produtos com desconto.
- `GET /product/availables`: Retorna produtos disponíveis em estoque.
- `GET /product?q`: Retorna todos os produtos com base em uma busca.

### Endereços
- `POST /address/addUserAddress`: Cadastra um novo endereço para empresa ou usuário.
- `DELETE /address/delete/:addressId`: Deleta um endereço cadastrado com base no ID.
- `GET /address/allAddress`: Retorna todos os endereços cadastrados.
- `PATCH /address/edit/:addressId`: Edita um endereço com base no ID.
- `GET /address/getaddress/:addressId`: Retorna informações de um endereço.
- `PATCH /address/select/mainAddress/:addressId`: Seleciona o endereço principal.

### Avaliações
- `POST /avaliation/create/:productId`: Permite que o usuário crie uma avaliação para um produto, incluindo texto e número.
- `GET /avaliation/:productId`: Retorna todas as avaliações de um produto com todas as informações.
- `GET /avaliation/myavaliations`: Retorna todas as avaliações do usuário.
- `DELETE /avaliation/delete/:avaliationID`: Deleta uma avaliação.

### Perguntas
- `POST /questions/create/:productId`: Permite que o usuário faça uma pergunta sobre um produto.
- `GET /questions/:productId`: Retorna todas as perguntas de um produto.
- `GET /questions/myquestions`: Retorna todas as perguntas do usuário.
- `DELETE /questions/delete/:questionId`: Deleta uma pergunta.

### Respostas
- `POST /answers/create/:questionId`: Permite que o usuário responda a uma pergunta sobre um produto.
- `GET /answers/:questionId?product=`: Retorna todas as respostas para uma pergunta de um produto.
- `GET /answers/myanswers`: Retorna todas as respostas do usuário.
- `DELETE /answers/delete/:answersId?product=`: Deleta uma resposta.

### Carrinho de Produtos
- `POST /cart/addcart/:productID`: Adiciona um produto ao carrinho do usuário.
- `GET /cart/getcart`: Retorna informações do carrinho do usuário.
- `DELETE /cart/removeproduct/:productId`: Remove um produto do carrinho.
- `POST /cart/addqty/:productId`: Aumenta a quantidade do produto em 1 unidade.
- `DELETE /cart/decreasyqty/:productId`: Diminui a quantidade do produto em 1 unidade.
- `POST /cart/checkout`: Finaliza a compra.
- `PATCH /cart/select/:productId`: Seleciona os produtos do carrinho a serem comprados.
- `PATCH /cart/editqty/:productId`: Edita a quantidade desejada do produto.
- `POST /cart/updatestock`: Atualiza o estoque após a finalização da compra.

## Autenticação
A E-commerce API utiliza autenticação baseada em tokens JWT (Json Web Token) para proteger os endpoints que requerem autorização. A seguir, explicamos como a autenticação funciona e como os usuários podem obter um token de autenticação.

### Obtendo um Token JWT
Para obter um token JWT e acessar os recursos protegidos, siga estas etapas:
1. Realize o cadastro como estabelecimento ou usuário da empresa.
2. Faça o login com suas credenciais utilizando o endpoint:
   `POST /enterprise/login`

Isso fornecerá um token JWT que deve ser incluído no cabeçalho de todas as solicitações autenticadas.

### Usando o Token JWT
Uma vez que você tenha obtido um token JWT, inclua-o no cabeçalho de suas solicitações para acessar os endpoints protegidos. O cabeçalho deve ser definido da seguinte forma:
```makefile
Authorization: Bearer SEU_TOKEN_JWT_AQUI
```
## Expiração do Token
Os tokens JWT geralmente têm um tempo de vida limitado. Certifique-se de que seu token esteja atualizado para evitar a expiração. Caso contrário, você precisará obter um novo token fazendo login novamente.

## Exemplo de Solicitação Autenticada
Aqui está um exemplo de como fazer uma solicitação autenticada utilizando o token JWT no cabeçalho:

GET /cart/addCart/:productId 
Authorization: Bearer SEU_TOKEN_JWT_AQUI

## Possíveis Erros de Autenticação
Caso ocorram erros de autenticação, a API retornará as mensagens de erro apropriadas para ajudar a solucionar problemas de autenticação.

Certifique-se de proteger seu token JWT e nunca compartilhá-lo publicamente. Mantenha suas credenciais seguras para garantir a segurança da sua conta.

Lembre-se de que esta é uma visão geral da autenticação na E-commerce API. Consulte a documentação completa para obter detalhes adicionais e exemplos.

## Pré-requisitos
Antes de começar a usar a E-commerce API, verifique se você atende aos seguintes pré-requisitos:
- Node.js e npm (gerenciador de pacotes Node.js) instalados em seu sistema. Você pode baixá-los em [nodejs.org](https://nodejs.org/).
- Um banco de dados MySQL configurado e acessível.
- Um ambiente de desenvolvimento ou servidor onde você possa implantar a API.

## Instalação
Siga estas etapas para instalar e configurar a E-commerce API:

1. Clone o repositório a partir do GitHub:

    ```bash
    git clone https://github.com/ViniciusRimes/Api-E-commerce.git
    ```
## Contato
- 📧 E-mail: viniciusrimess@gmail.com
- 💼 LinkedIn: [ViníciusRimes](https://www.linkedin.com/in/vinicius-rimes-de-oliveira/)