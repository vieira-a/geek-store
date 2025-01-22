# RFC 1 - Segurança de Dados da API - Produtos

- [Motivação](#motivação)
- [Contexto](#contexto)
- [Alternativas Consideradas](#alternativas-consideradas)
- [Decisão](#decisão)
- [Impacto](#impacto)
- [Execução](#execução)
- [Conclusão](#conclusão)


## Motivação

Esta RFC descreve as práticas recomendadas para a geração e exposição de dados sensíveis de produtos na API e como garantir que informações como o ID do banco de dados não sejam expostas diretamente no frontend. Em vez disso, o sistema usará códigos internos e slugs, permitindo uma abordagem mais segura e amigável para exibir produtos no frontend.

## Contexto

Quando expomos dados de produtos diretamente, especialmente o identificador único (ID), podemos abrir brechas de segurança, como a possibilidade de os usuários tentarem acessar informações de produtos manipulando URLs. Além disso, o uso de `slugs` e códigos internos pode melhorar a experiência do usuário e facilitar a indexação e busca de produtos na plataforma.

## Alternativas Consideradas

1. Geração de Slug e Código Interno

A estratégia envolve criar e expor dados alternativos ao ID para o frontend. Esses dados devem ser gerados de forma segura e única, garantindo que não haja possibilidade de conflito ou manipulação fácil por parte do usuário.

**Slug**

O slug é uma string amigável para a URL, geralmente gerada a partir do nome do produto. Esse slug será exposto no frontend para representar o produto.

Formato do Slug: O slug deve ser gerado a partir do nome do produto, utilizando um algoritmo que converta espaços em hífens, remova caracteres especiais e converta tudo para minúsculas.

Exemplo: Smartphone Samsung Galaxy S21 → `smartphone-samsung-galaxy-s21`

Este slug será utilizado na URL para acessar a página do produto, por exemplo: `/product/smartphone-samsung-galaxy-s21`

2. Código Interno

Além do slug, o produto terá um código interno único, utilizado para identificar o produto de forma única e segura na base de dados. Esse código será utilizado em lugar do ID para fornecer uma camada adicional de ofuscação.

Formato do Código Interno: O código interno pode ser gerado automaticamente no momento da criação do produto. Ele pode ser composto por um prefixo (como PDT-) e um valor único gerado, por exemplo, por um UUID ou um contador único.

Exemplo: PDT1234567890

Esse código interno será passado para o frontend e usado para consultas e ações no sistema, ao invés do ID real.

**Prós**

- Segurança: A principal vantagem é a proteção contra ataques que tentam acessar produtos manipulando URLs. Sem expor o ID, a segurança do sistema é aumentada.
- SEO: O uso de slugs amigáveis ao SEO pode melhorar a indexação e a visibilidade do produto nos motores de busca.
- Facilidade de Manutenção: ao usar códigos internos, é possível garantir que a lógica interna de identificação de produtos seja desacoplada da representação pública do produto.

**Contras**

- Aumento da complexidade na implementação

## Decisão

- A API deve garantir que o frontend só receba o slug e o código interno ao fazer a consulta aos produtos. O ID real do banco de dados deve ser mantido no backend para evitar manipulação e exposição direta.

**Estrutura de Resposta da API**

- Quando um produto for retornado pela API, deve-se incluir o slug e o código interno como parte da resposta, mas não o ID. 

Exemplo:

```
{
  "slug": "smartphone-samsung-galaxy-s21",
  "gsic": "PDT1234567890",
  "name": "Smartphone Samsung Galaxy S21",
  "price": 3999.99,
  "stock": 150,
  "category": "Smartphones",
  "imageUrl": "https://example.com/images/s21.jpg"
}
```

- Quando o frontend faz uma consulta para detalhes de um produto, ele utiliza o slug ou codigo interno, não o ID.

**Exemplo de endpoint de consulta**

- `GET /products/{slug}/{gsic}`: Retorna os detalhes de um produto com base no slug e código interno.

## Impacto

A criação de um código interno único para cada produto garante que, mesmo que o usuário tenha acesso ao backend ou ao banco de dados, ele não possa facilmente adivinhar ou manipular o ID real do produto.

## Execução

- Incluir os campos `slug` e `gsic` na modelagem de dados dos produtos
- Criar lógica para geração do `slug` com base no nome descritivo do produto
- Criar lógica para geração do `gsic` (com um prefixo `PT` no caso de produto, seguido de mais 10 caracteres alfanuméricos em maiúsculo)
- A aplicação não deve permitir duplicidade do código interno

## Conclusão

Este RFC propõe uma maneira eficiente e segura de manipular dados na API, utilizando o slug e o código interno em vez do ID para representar produtos. Isso não só melhora a segurança ao ocultar identificadores internos, mas também oferece benefícios como uma melhor experiência de usuário e uma estrutura mais robusta para a API.
