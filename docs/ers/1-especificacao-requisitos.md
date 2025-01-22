# Especificação de Requisitos de Software

- [Objetivo](#objetivo)
- [Requisitos Funcionais](#requisitos-funcionais)
- [Requisitos Não Funcionais](#requisitos-nao-funcionais)

## Objetivo

Aplicação que simula as funcionalidades de um e-commerce, permitindo a exibição de uma lista de produtos, interação com o carrinho de compras e proporcionando uma experiência responsiva para o usuário. 
A aplicação será estruturada para possibilitar a adição de funcionalidades no futuro, como o envio de e-mails para usuários que abandonaram o carrinho.

## Requisitos Funcionais

**Listagem de Produtos:**

- Exibição de uma lista de produtos com seus respectivos detalhes (nome, preço, descrição e imagem).
- Capacidade de adicionar produtos ao carrinho de compras.

**Carrinho de Compras:**

- Exibição de um carrinho de compras com itens selecionados.
- Possibilidade de ajustar a quantidade de cada item no carrinho.
- Exibição do preço total do carrinho, considerando os itens e suas quantidades.
- Capacidade de remover itens do carrinho.

**Interação Responsiva:**

- Interface responsiva, funcionando bem em dispositivos móveis, tablets e desktops.

**Funcionalidade futura**

- A estrutura da aplicação deve possibilitar a implementação futura de envio de e-mails para usuários que abandonaram o carrinho.

## Requisitos Não Funcionais

**Desempenho e escalabilidade:**

- A aplicação deve ser escalável, sendo capaz de lidar com um aumento no número de usuários e produtos.

**Segurança:**

- Implementar boas práticas de segurança no backend, incluindo criptografia de dados sensíveis e proteção contra ataques comuns.

**Performance**

- Utilizar sistema de cache para otimizar a performance em consultas frequentes ao banco de dados.

**Tecnologias a serem usadas:**

**Backend**

- Nest.js com Node.js e Express. 

**Banco de Dados**

- MongoDB
- Mongoose

**Sistema de Cache**

- Redis.

**Frontend**

- React com Next.js.

**UI/UX**

- Shadcn UI 
- TailwindCSS.

**Documentação da API**

- Swagger

**Testes**

- Testes unitários, de integração ou end-to-end serão implementados.
