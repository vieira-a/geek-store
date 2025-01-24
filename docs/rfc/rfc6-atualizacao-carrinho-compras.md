# RFC: Atualização de Carrinho de Compras

- [Motivação](#motivação)
- [Contexto](#contexto)
- [Execução](#execução)
- [Conclusão](#conclusão)

## Motivação

Esta RFC propõe uma melhoria no comportamento de atualização de carrinho de compras para o e-commerce. Ela descreve como o backend deve gerenciar a atualização do carrinho, incluindo a adição de novos itens, a atualização de quantidades e a remoção de itens não enviados na atualização. Além disso, a RFC inclui o cálculo adequado dos totais do carrinho após cada operação.

## Contexto

O objetivo é garantir que a funcionalidade de atualização do carrinho seja robusta e reflita corretamente o comportamento esperado dos usuários, que podem:

- Atualizar quantidades de itens.
- Adicionar novos itens ao carrinho.
- Remover itens do carrinho ao deixá-los de fora na atualização.
- Além disso, os totais de totalItems e totalPrice devem ser recalculados adequadamente.

## Execução

### Detalhes da implementação

#### Regras de Atualização de Carrinho

- Adicionar Novo Item: Se um produto não existir no carrinho e for enviado na atualização com uma quantidade maior que 0, ele será adicionado ao carrinho.
- Atualizar Quantidade: Se o produto já existir no carrinho, sua quantidade será atualizada para o valor enviado na atualização. O subtotal será recalculado com base na nova quantidade.
- Remover Item: Se um item estiver presente no carrinho, mas não for enviado na atualização com quantidade maior que 0, ele será removido do carrinho.
- Validação de Estoque: A quantidade solicitada para cada item será validada contra o estoque disponível. Se o estoque for insuficiente, será gerado um erro.
- Recalcular Totais: Após cada operação de adição, atualização ou remoção, os totais de totalItems (quantidade total de itens) e totalPrice (valor total do carrinho) serão recalculados.

#### Fluxo de Atualização

**Recebimento da Atualização:**

- O backend recebe uma lista de itens atualizada a partir do frontend. Cada item contém:
  - gsic: código interno do produto.
  - quantity: nova quantidade solicitada para o produto.
  - subtotal: valor total daquele item, calculado a partir da quantidade e preço.

**- Validação de Produto:**

- O backend valida se cada item existe no banco de dados. Caso contrário, retorna um erro de "Produto não encontrado".
- O backend valida se a quantidade solicitada de cada item é maior que o estoque disponível. Caso contrário, retorna um erro de "Produto sem estoque suficiente".

**Adição ou Atualização de Itens:**

- Se o produto não está no carrinho, ele será adicionado.
- Se o produto já está no carrinho, sua quantidade será ajustada e o subtotal será recalculado.

**Remoção de Itens:**

- Itens que não foram incluídos na atualização serão removidos do carrinho.

**Recalcular Totais:**

- totalItems: Soma das quantidades de todos os itens no carrinho.
- totalPrice: Soma dos subtotais de todos os itens no carrinho.

**Persistência:** 

- O carrinho atualizado é salvo no banco de dados, refletindo as mudanças realizadas.

#### Exemplo de Fluxo

**Entrada:**

```
{
  "sessionId": "session123",
  "gsic": "cart123",
  "items": [
    { "gsic": "product123", "quantity": 2 },
    { "gsic": "product456", "quantity": 0 }
  ]
}
```
**Processamento:**

- Produto product123 já existe no carrinho e tem a quantidade atualizada para 2.
- Produto product456 não será removido, pois a quantidade foi passada como 0 (indicando remoção).
- Recalcular os totais do carrinho.

**Saída:**

```
{
  "sessionId": "session123",
  "gsic": "cart123",
  "totalItems": 2,
  "totalPrice": 200,
  "items": [
    { "gsic": "product123", "name": "Produto 123", "quantity": 2, "subtotal": 200 }
  ]
}
```

#### Validação de Estoque

- Se a quantidade solicitada de qualquer item for maior que o estoque disponível, o backend deve lançar um erro com status BAD_REQUEST e uma mensagem informando que o produto não possui estoque suficiente.

5.3. Exceções

- Produto não encontrado: Se um item não existir no banco de dados, a resposta será um erro NOT_FOUND.
- Produto sem estoque suficiente: Se o estoque do produto for inferior à quantidade solicitada, a resposta será um erro BAD_REQUEST.

## Impacto

- Melhoria no comportamento do carrinho: Usuários terão uma experiência mais fluida, com a possibilidade de adicionar, atualizar e remover itens de maneira consistente.
- Redução de erros: A validação de estoque e a remoção de itens não enviados garantem que o carrinho sempre reflita o estado atual desejado.
- Recalculo de totais: O total de itens e o preço do carrinho serão sempre precisos após a atualização.
  
## Conclusão
A implementação desta RFC vai garantir que o carrinho de compras se comporte de maneira confiável e precisa, permitindo aos usuários a gestão eficiente de seus itens, com adições, atualizações e remoções funcionando corretamente. A lógica de validação de estoque e remoção de itens não enviados proporciona uma experiência robusta e alinhada com as expectativas dos usuários.
