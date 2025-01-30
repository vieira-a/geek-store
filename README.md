# Projeto GeekStore

## Documentação

### Requisitos

- [ERS 1 - Especificação de Requisitos de Software](https://github.com/vieira-a/geek-store/blob/main/docs/ers/1-especificacao-requisitos.md)

### C4 model

- [Contexto](https://github.com/vieira-a/geek-store/blob/main/docs/c4-model/context.md)
- [Container](https://github.com/vieira-a/geek-store/blob/main/docs/c4-model/container.md)
- [Componentes](https://github.com/vieira-a/geek-store/blob/main/docs/c4-model/component.md)

### Decisões técnicas

- [RFC 1 - Organização do Projeto](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc-1-organizacao-projeto.md)
- [RFC 2 - Segurança na API de Produtos](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc2-seguranca-api-produtos.md)
- [RFC 3 - Arquitetura simplificada do backend](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc3-arquitetura-simplificada-backend.md)
- [RFC 4 - Tratamento de erros no backend](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc4-tratamento-de-erros.md)
- [RFC 5 - Segurança na API de Carrinho de Compras](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc5-seguranca-api-carrinho-de-compra.md)
- [RFC 6 - Atualização do Carrinho de Compras](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc6-atualizacao-carrinho-compras.md)
- [RFC 7 - Mesclagem do Carrinho de Compras](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc7-mesclagem-carrinho-de-compra.md)
- [RFC 8 - Justificativa para não cumprimento de algumas etapas do desafio técnico](https://github.com/vieira-a/geek-store/blob/main/docs/rfc/rfc8-justificativa.md)

## Repositórios

- [Backend](https://github.com/vieira-a/geek-store-backend)
- [Frontend](https://github.com/vieira-a/geek-store-frontend)

## Como executar o projeto

### Configuração do ambiente

#### Backend (Nest.js)

##### Clone o repositório do backend:

`git clone https://github.com/vieira-a/geek-store-backend.git`

##### Navegue até o diretório geek-store-backend: 

`cd geek-store-backend`

##### Crie um arquivo .env na raiz do diretório geek-store-backend e configure as variáveis de ambiente necessárias. 

Exemplo de conteúdo do arquivo .env:

```
MONGODB_URI=mongodb+srv://<usuário>:<senha>@geekstore-db-0.hjyap.mongodb.net/?retryWrites=true&w=majority&appName=geekstore-db-0
MONGODB_DATABASE=geekstore-db-0
ENCRYPT_SALT=12
JWT_SECRET=geekstore
CORS_ORIGIN=http://localhost:3002
```

Endereço da aplicação: http://localhost:3001/api/v1
Documentação: http://localhost:3001/api/v1/docs

Se você optar por usar o MongoDB Atlas (recomendado para produção), substitua <usuário> e <senha> pelas suas credenciais do MongoDB Atlas. A string de conexão do Atlas está incluída na variável de ambiente MONGODB_URI.

#### **Frontend (Next.js)**

##### Clone o repositório do frontend:

`git clone https://github.com/vieira-a/geek-store-frontend.git`

##### Navegue até o diretório geek-store-frontend: 

`cd geek-store-frontend`

##### Crie um arquivo .env na raiz do diretório geek-store-frontend e configure as variáveis de ambiente necessárias. 

Exemplo de conteúdo do arquivo .env.local:
`NEXT_PUBLIC_GEEKSTORE_API_URL="http://localhost:3001/api/v1"`

Endereço da aplicação: http://localhost:3002

#### Configuração do MongoDB**

Se você optar por usar o MongoDB Atlas (recomendado), o processo será simples, pois você não precisa rodar o MongoDB localmente. Basta seguir as etapas para criar um cluster no MongoDB Atlas e gerar uma string de conexão.
Caso prefira usar o MongoDB local (não recomendado para produção), você pode configurar o MongoDB no arquivo docker-compose.yml (instruções abaixo).

## Como Rodar localmente com Docker

### Usando Docker Compose

#### Se você estiver utilizando o MongoDB local (não usando o Atlas), crie um arquivo .env na raiz deste projeto GeekStore (onde o docker-compose.yml está localizado) com as variáveis de ambiente:

Exemplo de conteúdo do .env:

```
MONGODB_URI=mongodb://mongodb:27017/geekstore-db-0
MONGODB_DATABASE=geekstore-db-0
ENCRYPT_SALT=12
JWT_SECRET=geekstore
CORS_ORIGIN=http://localhost:3002
NEXT_PUBLIC_GEEKSTORE_API_URL="http://localhost:3001/api/v1"
```

#### No diretório raiz, onde o arquivo docker-compose.yml está localizado, execute o comando para rodar os containers com Docker Compose:

` docker-compose up --build`

Isso irá iniciar os containers do frontend, backend e MongoDB local, permitindo que você acesse a aplicação na URL:

- Backend: http://localhost:3001
- Frontend: http://localhost:3002

### Sem Docker Compose (Usando Node.js diretamente)

Se você preferir rodar o projeto sem o Docker, basta seguir os passos abaixo:

#### Backend (Nest.js)

##### Navegue até o diretório geek-store-backend e instale as dependências:

```
cd geek-store-backend
npm install
```

##### Inicie o backend

`npm run start:dev`

#### Frontend (Next.js)

##### Navegue até o diretório geek-store-frontend e instale as dependências:

```
cd geek-store-frontend
npm install
```

##### Inicie o frontend:

`npm run dev`

O frontend estará disponível em http://localhost:3002 e o backend em http://localhost:3001.

