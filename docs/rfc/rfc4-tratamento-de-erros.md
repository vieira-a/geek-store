# RFC 4 - Tratamento de erros no Backend

- [Motivação](#motivação)
- [Contexto](#contexto)
- [Alternativas Consideradas](#alternativas-consideradas)
- [Decisão](#decisão)
- [Impacto](#impacto)
- [Execução](#execução)
- [Conclusão](#conclusão)
  
## Motivação
Esta RFC descreve as práticas recomendadas para uma abordagem consistente e robusta para o tratamento de erros garantindo que as respostas sejam uniformes e informativas para todos os casos de erro.

## Contexto
Quando uma aplicação não manipula os erros corretamente, além de correr o risco de parar de funcionar, dificulta a identificação de problemas.

## Alternativas Consideradas

1. Implementar um sistema de tratamento de erros global no backend que:

- Garanta uniformidade no formato das respostas de erro.
- Suporte a utilização de exceções personalizadas com um campo `name` para identificar o tipo de erro.
- Forneça informações adicionais, como timestamp e URL requisitada.

**Prós**

- **Uniformidade**: Todas as respostas de erro seguem o mesmo padrão.
- **Extensibilidade**: Suporte a exceções personalizadas com nomes identificáveis.
- **Consistência**: Informações claras e completas para facilitar o debug.

**Contras**

- Aumenta a complexidade

2. Utilizar erros padrão do Nest.js sem filtro global de exceções

**Prós**

- Desenvolvimento mais rápido e simplificado

**Contras**

- Não trata uma parte crítica de qualquer aplicação.
- Não ter um filtro global não garante que erros desconhecidos serão manipulados de forma correta, podendo acarretar em quebra da aplicação

## Decisão

1. **Criação de uma classe de exceções personalizadas** que herda de `HttpException` e permite a definição de um campo `name`.
2. **Implementação de um filtro global de exceções** utilizando o mecanismo do NestJS para capturar todas as exceções e formatar as respostas de erro.
3. **Definição de um formato padrão** para todas as respostas de erro, contendo:
   - `statusCode`: código HTTP da resposta.
   - `message`: mensagem descritiva do erro.
   - `error`: tipo do erro.
   - `name`: identificador do tipo de recurso (quando aplicável).
   - `timestamp`: momento em que o erro ocorreu.
   - `path`: URL requisitada.

### Exemplo de Resposta
Se uma exceção `ProductException` for lançada, a resposta seguirá o seguinte formato:

```json
{
  "statusCode": 404,
  "message": "Product not found",
  "error": "Not Found",
  "name": "ProductException",
  "timestamp": "2025-01-22T19:15:02.123Z",
  "path": "/products/nonexistent-slug/12345"
}
```

## Impacto

Manipulação correta dos erros, mesmo os desconhecidos, de modo a facilitar o debug, bem como assegurar o bom funcionamento de toda a aplicação

## Conclusão
  
Essa implementação garante um tratamento de erros robusto e escalável, atendendo às necessidades do projeto e melhorando a experiência de desenvolvimento e integração com o frontend.
