# RFC 5 - Segurança de dados da API - Carrinhos

- [Motivação](#motivação)
- [Contexto](#contexto)
- [Alternativas Consideradas](#alternativas-consideradas)
- [Decisão](#decisão)
- [Impacto](#impacto)
- [Execução](#execução)
- [Conclusão](#conclusão)

## Motivção

Este documento tem como objetivo estabelecer diretrizes de segurança para a API de carrinhos de compras, alinhando as melhores práticas para proteger os dados dos usuários e evitar vulnerabilidades.

A API deve garantir:
- Proteção contra exposição de dados sensíveis (como ID de banco de dados).
- Segurança no armazenamento e gerenciamento dos carrinhos de compras.
- Suporte para fluxos de usuários logados e anônimos sem comprometer a segurança.

## Contexto

Durante o desenvolvimento da API de carrinhos, identifiquei que a exposição direta de IDs internos (como o identificador do produto ou do carrinho no banco de dados) poderia comprometer a segurança e facilitar ataques como manipulação de dados ou exploração indevida de recursos.


## Alternativas Consideradas

Considerei o cenário em que usuários anônimos criam carrinhos temporários, que podem ser vinculados a um usuário ao realizar login. O fluxo precisa ser seguro e evitar inconsistências no sistema.

1. Ao fazer login, o usuario pode ser vinculado diretamente ao carrinho de compras, inserindo o ID do usuario no carrinho

**Prós**
- Facilidade e agilidade na implemtação

**Contras**
- Expõe dados sensíveis
- Complexidade para gerenciar abandono do carrinho futuramente

2. Ao fazer login, o sistema vincula internamente o carrinho a um registro de carrinho do usuario (UserCartSchema); dessa forma, posso manter o histórico do carrinho e poder implementar recursos para gerenciar abandonos de carrinho futuramente.

**Prós**
- O `cartCode` é gerado de forma segura e é único.
- Não expor IDs internos no frontend.
- Utilizar autenticação e autorização adequadas para endpoints relacionados ao `UserCart`.
- 
**Contras**
- Aumento da complexidade na implementação

## Decisão

A solução adotada envolve:

1. **Uso de Códigos Internos Seguros:**
   - Cada carrinho de compras terá um código interno único gerado pelo backend (**gsic**).
   - O gsic é armazenado no localStorage do frontend para identificar o carrinho nas interações.
   - O ID do documento do carrinho no banco de dados é mantido interno ao sistema e nunca exposto.

2. **Desassociação de dados do usuário do carrinho:**
   - Para usuários anônimos, o carrinho será identificado apenas pelo gsic.
   - Quando o usuário fizer login, o carrinho anônimo será vinculado a ele através de uma entrada em um modelo separado (**UserCart**).

3. **Status de carrinho:**
   - Cada carrinho terá um status que reflete seu estado atual:
     - **`active`**: Carrinho ativo e em uso.
     - **`abandoned`**: Carrinho não acessado por um período definido.
     - **`completed`**: Carrinho associado a um pedido concluído.

4. **Fluxo seguro para usuários logados:**
   - Ao logar, o backend verifica a existência de um carrinho ativo no localStorage e:
     - Associa o carrinho ao usuário criando uma entrada em **UserCart**.
     - Caso não exista um carrinho ativo, cria um novo e atualiza o localStorage.

5. **Persistência de histórico:**
   - Mesmo que o carrinho seja abandonado ou concluído, os dados serão mantidos para análise e tratamento de carrinhos abandonados.

## Impacto

- **Segurança:** Reduz o risco de exploração de IDs internos e manipulações indevidas.
- **Flexibilidade:** Permite gerenciar carrinhos de usuários logados e anônimos sem alterar a lógica de base.
- **Escalabilidade:** Suporta funcionalidades futuras como histórico e recuperação de carrinhos abandonados.

## Execução

### Modelagem de Dados

#### Carrinho de Compras (Cart)
```typescript
const CartSchema = new Schema({
  gsic: { type: String, unique: true, required: true },
  items: [
    {
      gsic: { type: String, required: true },
      quantity: { type: Number, required: true },
      price: { type: Number, required: true },
    },
  ],
  status: { type: String, enum: ['active', 'abandoned', 'completed'], default: 'active' },
  createdAt: { type: Date, default: Date.now },
  updatedAt: { type: Date, default: Date.now },
});
```

#### Fluxo de Requisições da API

1. **Criação de Carrinho:**
   - Gera um novo **gsic** e retorna ao frontend para persistência no localStorage.

2. **Adicionação de Produtos:**
   - Recebe o **gsic** e valida sua existência.
   - Atualiza os itens e verifica estoque.

3. **Login do Usuário:**
   - Associa o **gsic** existente ao userId no UserCart.

4. **Finalização da Compra:**
   - Atualiza o status do carrinho para `completed`.

## Conclusão

Esta RFC detalha uma abordagem segura e eficiente para a API de carrinhos de compras, garantindo proteção dos dados e flexibilidade para extensões futuras.

