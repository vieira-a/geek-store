# RFC 3 - Arquitetura Simplificada do Backend

- [Motivação](#motivação)
- [Contexto](#contexto)
- [Alternativas Consideradas](#alternativas-consideradas)
- [Decisão](#decisão)
- [Impacto](#impacto)
- [Execução](#execução)
- [Conclusão](#conclusão)

## Motivação

Esta RFC propõe uma simplificação na arquitetura do backend do projeto, com o objetivo de acelerar o desenvolvimento, reduzir a complexidade desnecessária e aderir às práticas recomendadas pela documentação oficial do Nest.js. 

A decisão de simplificar busca atender às restrições de tempo do desafio técnico e manter o código limpo e fácil de entender, sem comprometer a qualidade ou a escalabilidade do projeto.

## Contexto

Originalmente, o backend foi planejado com uma abordagem baseada em camadas complexas, incluindo o uso extensivo de repositórios, mapeadores e abstrações para cada funcionalidade. Embora essas práticas sejam adequadas para sistemas mais robustos e de longo prazo, elas podem ser excessivas para um projeto inicial ou MVP (Minimum Viable Product).

Em vez disso, o backend será estruturado utilizando os recursos padrão do Nest.js, com foco na simplicidade, enquanto mantém uma separação de responsabilidades clara por meio de módulos organizados. 

Cada funcionalidade (produtos, usuários, carrinho, etc.) será implementada como um módulo separado, seguindo a estrutura recomendada pelo Nest.js.

## Alternativas Consideradas

### Arquitetura Complexa com Camadas Abstratas

Nesta abordagem, cada módulo teria as seguintes camadas:

- **Controllers**: Responsáveis por receber as requisições HTTP.
- **Services**: Contendo toda a lógica de negócios.
- **Repositories**: Lidando com a comunicação direta com o banco de dados.
- **Mappers**: Convertendo entidades para DTOs e vice-versa.

**Prós:**
- Altamente modular e escalável.
- Facilita testes unitários ao isolar dependências.

**Contras:**
- Excesso de abstração pode dificultar a manutenção em um MVP.
- Tempo de desenvolvimento maior.

### Arquitetura Simplificada

- Utilizar apenas **Controllers** e **Services** para lidar com a lógica de negócios e comunicação com o banco de dados.
- O módulo Mongoose será utilizado diretamente para interagir com o banco, eliminando a necessidade de criar repositórios customizados.
- Validações serão centralizadas nos DTOs.

**Prós:**
- Redução significativa da complexidade.
- Agilidade no desenvolvimento.
- Facilmente escalável para arquiteturas mais complexas futuramente.

**Contras:**
- Menor abstração pode dificultar mudanças drásticas no futuro, embora isso seja mitigado pela separação por módulos.

## Decisão

Adotar a **Arquitetura Simplificada**, utilizando apenas as estruturas essenciais oferecidas pelo Nest.js. 

- Cada funcionalidade será implementada como um módulo, contendo controllers, services e schemas.
- DTOs serão usados para validação e transporte de dados.
- O Mongoose será utilizado diretamente nos serviços para operações no banco de dados.

**Exemplo de Estrutura do Módulo de Produtos:**

- **products**
  - controllers/
    - products.controller.ts
  - services/
    - products.service.ts
  - schemas/
    - product.schema.ts
  - dtos/
    - create-product.dto.ts
    - update-product.dto.ts
  - products.module.ts

## Impacto

- Redução do tempo de desenvolvimento, permitindo atender ao prazo do desafio técnico.
- Código mais direto e fácil de compreender.
- Menor barreira de entrada para novos desenvolvedores que precisem trabalhar no projeto.

## Execução

1. Estruturar o projeto conforme a arquitetura simplificada.
2. Criar os módulos necessários para as funcionalidades principais (produtos, clientes, carrinho, etc.).
3. Implementar os controllers, services, schemas e DTOs necessários para cada módulo.

## Conclusão

A simplificação da arquitetura permite equilibrar agilidade de desenvolvimento e qualidade do código, cumprindo os requisitos do desafio técnico sem introduzir complexidade desnecessária. Essa abordagem segue as práticas recomendadas pelo Nest.js e pode ser facilmente expandida no futuro para atender demandas mais complexas.

