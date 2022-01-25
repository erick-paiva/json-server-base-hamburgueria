<h1 align="center">
  <img alt="Burguer Kenzie" title="Burguer Kenzie" src="https://i.ibb.co/djbw6LV/x-burgue.png" width="100px" />
</h1>

<h1 align="center" color="#27AE60">
  Entrega Hamburgueria Kenzie
</h1>

## **Endpoints**
A api tem um total de 7 endPoints, onde o usuario pode comprar diversas guloseimas entre elas hamburguers e bebidas em geral.

O JSON para utilizar no Insomnia é este aqui -> 

<h2 align ='center'> Registrar um usuário </h2>

`POST /register -  FORMATO DA REQUISIÇÃO`

```json
{
	"email": "johndoe@email.com",
	"password":"123456",
	"name": "John Doe",
}

```
Caso dê tudo certo, a resposta será assim:

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6IjEyM3NrYXRlc2s4QG1haWwuY29tIiwiaWF0IjoxNjQyMDk0NjQ4LCJleHAiOjE2NDIwOTgyNDgsInN1YiI6IjIifQ.ruCsRqvBHQpxdOYBTA592GI99LykAKXX_895sZq9Boc",
  "user": {
    "email": "johndoe@email.com",
    "name": "John Doe",
    "id": 1
  }
}

```
<h2 align ='center'> Possíveis erros </h2>

Caso você esqueça de colocar o email ou senha a reposta do erro será assim:
No exemplo a requisição foi feita faltando o email.


`"Email and password are required"`

Email ja cadastrado:

`POST /register FORMATO DA RESPOSTA - STATUS 400`

```json
{
  "status": "error",
  "message": "Email already exists"
}
```

<h2 align ='center'> Fazer o Login </h2>


`POST /login - FORMATO DA REQUISIÇÃO`

```json
{
    "email": "johndoe@email.com",
    "password": "123456"
}
```

Caso dê tudo certo, a resposta será assim:

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6IjEyM3NrYXRlc2s4QG1haWwuY29tIiwiaWF0IjoxNjQyMDk3NTA5LCJleHAiOjE2NDIxMDExMDksInN1YiI6IjIifQ.O7t0sbgb8PvDjtDTrYIqHE-qE-jjvYsKsQ7gbcTNGwU",
  "user": {
    "email": "johndoe@email.com",
    "name": "John Doe",
    "id": 1
  }
}
```

Com essa resposta, vemos que temos duas informações, o user e o token respectivo, dessa forma você pode guardar o token e o usuário logado no localStorage para fazer a gestão do usuário no seu frontend.

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {token}

Após o usuário estar logado, ele deve conseguir adcionar produtos ao carrinho.

<h2 align ='center'> Adcionar um produto no carrinho </h2>

`POST /cart - FORMATO DA REQUISIÇÃO`
```json
[
 {
      "name": "Fanta",
      "category": "Bebidas",
      "price": 4.99,
      "img": "https://i.ibb.co/QNb3DJJ/milkshake-ovomaltine.png",
			"amount": 1,
			"userId": 2
}
]
```
Você devera informar os respectivos itens ao seu carrinho
    1. O nome do produto
    2. a categoria do produto
    3. o preço do produto
    4. a imagem
    5. a quantidade
    6. o id do usuario

Caso dê tudo certo, a resposta será assim:

```json
{
  "name": "Fanta",
  "category": "Bebidas",
  "price": 4.99,
  "img": "https://i.ibb.co/QNb3DJJ/milkshake-ovomaltine.png",
  "amount": 1,
  "userId": 2,
  "id": 4
}
```
<h2 align ='center'> Aumentar a quantidade de produtos </h2>

`PATCH /cart - FORMATO DA REQUISIÇÃO`
```json
{
	"amount": 3
}
```

Você devera informar os respectivos itens no seu produto
    1. a quantidade do respectivo produto

Caso dê tudo certo, a resposta será assim:

```json
{
  "amount": 3,
  "price": 14,
  "category": "Sanduíches",
  "img": "https://i.ibb.co/fpVHnZL/hamburguer.png",
  "name": "Hamburguer",
  "userId": 2,
  "id": 1
}
```

Obtendo o carrinho da api

`GET /cart/?userid="youUserId" - FORMATO DA REQUISIÇÃO`

Caso dê tudo certo, a resposta será assim:

```json[
{
    "amount": 3,
    "price": 14,
    "category": "Sanduíches",
    "img": "https://i.ibb.co/fpVHnZL/hamburguer.png",
    "name": "Hamburguer",
    "userId": 2,
    "id": 1
  },
  {
    "amount": 5,
    "price": 18,
    "category": "Sanduíches",
    "img": "https://i.ibb.co/FYBKCwn/big-kenzie.png",
    "name": "Big Kenzie",
    "userId": 2,
    "id": 2
  },
  {
    "amount": 4,
    "price": 16,
    "category": "Sanduíches",
    "img": "https://i.ibb.co/djbw6LV/x-burgue.png",
    "name": "X-Burguer",
    "userId": 2,
    "id": 3
  },
  {
    "name": "Fanta",
    "category": "Bebidas",
    "price": 4.99,
    "img": "https://i.ibb.co/QNb3DJJ/milkshake-ovomaltine.png",
    "amount": 1,
    "userId": 2,
    "id": 4
  }
]
```

Também é possível remover um produto do seu carrinho

`DELETE /cart/:product_id - FORMATO DA REQUISIÇÃO`

```
Não é necessário um corpo da requisição.
```

## Rotas que não necessitam de autorização

<h2 align ='center'> Obtendo todos os produtos da api </h2>

Nessa aplicação o usuário precisa esta apenas logado para ver os produtos já cadastrados na plataforma, na API podemos acessar os produtos dessa forma:

`GET /products -  FORMATO DA RESPOSTA - STATUS 200`

```json
[
  {
    "id": 1,
    "name": "Hamburguer",
    "category": "Sanduíches",
    "price": 14,
    "img": "https://i.ibb.co/fpVHnZL/hamburguer.png",
    "amount": 1
  },
  {
    "id": 2,
    "name": "X-Burguer",
    "category": "Sanduíches",
    "price": 16,
    "img": "https://i.ibb.co/djbw6LV/x-burgue.png",
    "amount": 1
  },
  {
    "id": 3,
    "name": "Big Kenzie",
    "category": "Sanduíches",
    "price": 18,
    "img": "https://i.ibb.co/FYBKCwn/big-kenzie.png",
    "amount": 1
  },
  {
    "id": 4,
    "name": "Fanta Guaraná",
    "category": "Bebidas",
    "price": 5,
    "img": "https://i.ibb.co/cCjqmPM/fanta-guarana.png",
    "amount": 1
  },
  {
    "id": 5,
    "name": "Coca",
    "category": "Bebidas",
    "price": 4.99,
    "img": "https://i.ibb.co/fxCGP7k/coca-cola.png",
    "amount": 1
  },
  {
    "id": 6,
    "name": "Fanta",
    "category": "Bebidas",
    "price": 4.99,
    "img": "https://i.ibb.co/QNb3DJJ/milkshake-ovomaltine.png",
    "amount": 1
  }
]
```


by Erick Paiva 👨‍💻
