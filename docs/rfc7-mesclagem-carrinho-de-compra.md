# RFC 7 - Mesclagem de Carrinhos de Compras

## Motivação

No contexto de e-commerce, os usuários podem adicionar itens ao carrinho sem estarem logados, criando um carrinho de sessão temporário. 
Quando esses usuários se autenticam, é necessário mesclar o carrinho de sessão com o carrinho persistente do cliente. 
Isso garantirá que o usuário não perca os itens ao fazer login em diferentes dispositivos, proporcionando uma experiência de compra contínua e segura.

## Contexto

- **Carrinho de Sessão**: Criado automaticamente quando o usuário adiciona itens ao carrinho sem estar autenticado. É identificado por um `sessionId` e é persistido temporariamente no banco de dados até que o usuário se autentique ou o carrinho seja completado.
- **Carrinho de Cliente**: Criado quando o usuário se autentica. É persistente e associado ao `gsic` do cliente.

Quando o usuário faz login, o sistema precisa verificar se existe um carrinho de sessão e mesclar os itens no carrinho de cliente, se este existir, ou associar o carrinho de sessão ao cliente, caso contrário.

Após a mesclagem, o carrinho de sessão precisa ser removido para evitar dados duplicados e garantir que o processo de compra seja claro e sem interferências.

## Alternativas Consideradas

1. **Não Mesclar Carrinhos**:

- **Prós**: Manter os carrinhos separados: o carrinho de sessão seria ignorado ou removido ao realizar login.
- **Contras**: Isso causaria a perda dos itens do carrinho de sessão, prejudicando a experiência do usuário, que perderia os itens adicionados sem estar logado.

2. **Mesclar Carrinhos Automaticamente**:

Mesclar os itens do carrinho de sessão no carrinho do cliente ao realizar o login, somando as quantidades e ajustando os subtotais.

- **Prós**: Proporciona uma experiência de usuário contínua, onde os itens não são perdidos e o processo de compra é fluido.
- **Contras**: Requer lógica extra para garantir que a mesclagem seja feita corretamente, o que pode aumentar a complexidade.

## Decisão

A decisão foi optar pela **mesclagem automática de carrinhos**. Esta solução mantém a consistência de dados e proporciona uma experiência de usuário muito mais amigável, especialmente em um cenário onde os clientes navegam em vários dispositivos.

Além disso, o carrinho de sessão será **removido** após a mesclagem para evitar a persistência desnecessária e garantir que o sistema não mantenha dados duplicados.

## Impacto

### Positivos:

- **Melhoria na experiência do usuário**: Garantia de que os itens do carrinho não se percam ao mudar de dispositivo.
- **Aumento na conversão**: Menor chance de abandono de carrinho, pois os itens ficam disponíveis ao usuário em qualquer dispositivo.
- **Redução de fricções no processo de login e checkout**: Usuários podem continuar de onde pararam, independentemente de como interagiram com a plataforma.

### Negativos:

- **Complexidade adicional**: A implementação de mesclagem de carrinhos aumenta a complexidade do sistema, exigindo verificação de carrinhos existentes e ajuste de totais.
- **Custo de desempenho**: Dependendo do volume de itens e do tráfego, a mesclagem pode gerar sobrecarga no banco de dados.

## Execução

1. **Verificação de Carrinho de Sessão**:

- Ao realizar login, o sistema verifica se há um carrinho de sessão ativo. Esse carrinho está identificado pelo `sessionId` e é persistido temporariamente.

2. **Mesclagem de Itens**:

- Se um carrinho de cliente já existir, o sistema mescla os itens do carrinho de sessão no carrinho do cliente, somando as quantidades e recalculando os subtotais.

3. **Deleção do Carrinho de Sessão**:

- Após a mesclagem, o carrinho de sessão é removido da base de dados, garantindo que não permaneça mais persistido e que não cause conflitos no futuro.

4. **Atualização de Totais**:

- Após a mesclagem, o total de itens e o total do preço do carrinho de cliente são recalculados com base nos itens combinados.

## Conclusão

A implementação da funcionalidade de mesclagem de carrinhos de sessão e de cliente proporciona uma experiência de compra contínua e segura para os usuários. 
A solução escolhida garante que os itens adicionados ao carrinho antes do login sejam preservados e adequadamente combinados com o carrinho do cliente, aumentando a satisfação do usuário e reduzindo o risco de abandono de carrinho.
