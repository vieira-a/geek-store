# Organização do Projeto Geek Store

- [Motivação](#motivação)
- [Contexto](#contexto)
- [Alternativas Consideradas](#alternativas-consideradas)
- [Decisão](#decisão)
- [Impacto](#impacto)
- [Execução](#execução)
- [Conclusão](#conclusão)


## Motivação

Garantir a clareza e modularidade do projeto Geek Store, facilitando o desenvolvimento, revisão e manutenção do código, bem como a avaliação durante o desafio técnico.

## Contexto

O projeto Geek Store consiste em três principais componentes:

- Backend: API desenvolvida com Nest.js para gerenciar lógica de negócio e persistência.
- Frontend: Interface de usuário criada com Next.js e TailwindCSS.
- Documentação: Contendo RFCs, diagramas e explicações do sistema.

Por conta do prazo limitado e do objetivo de demonstrar boas práticas, foi necessário decidir sobre a estrutura dos repositórios.

## Alternativas Consideradas

1. Repositório Monolítico
Todos os componentes do projeto ficariam em um único repositório, com subpastas separando backend, frontend e documentação:

```
/docs
/backend
/frontend
```

**Prós:**

- Simplicidade no gerenciamento do Git.
- Fácil sincronização entre backend e frontend.

**Contras:**

- Pode dificultar a avaliação isolada de componentes (ex.: analisar apenas o backend).
- Mistura de responsabilidades no repositório.
- CI/CD menos modular.

2. Repositórios Separados
Cada componente teria seu próprio repositório:

```
geek-store
geek-store-backend
geek-store-frontend
```

**Prós:**

- Alta modularidade, permitindo independência no desenvolvimento e avaliação.
- CI/CD configurado individualmente para cada componente.
- Reflete práticas reais em sistemas de grande escala.

**Contras:**

- Aumenta a complexidade de gerenciamento.
- Requer mais atenção para sincronização entre backend e frontend.

## Decisão

Optar por repositórios separados devido às seguintes razões:

- Facilidade de avaliação independente (backend, frontend, documentação).
- Melhor organização e modularidade.
- Demonstrar práticas comuns em projetos reais.

## Impacto

**Organização do Git:**

Três repositórios independentes no GitHub:

- geek-store
- geek-store-backend
- geek-store-frontend

**Processo de Desenvolvimento:**

Backend e frontend serão tratados como projetos independentes.
A documentação ficará centralizada no repositório de geek-store.

**Facilidade para os avaliadores:**

- Avaliadores podem focar em um componente específico sem navegar por um repositório monolítico.

## Execução
Criar os repositórios no GitHub:

- geek-store
- geek-store-backend
- geek-store-frontend

**Estruturar cada repositório:**

- Adicionar .gitignore e arquivos de configuração necessários.
- Criar um README.md claro para cada repositório.
- Definir scripts para iniciar os componentes (ex.: npm start, npm run dev).

**Adicionar instruções de integração:**

- Como conectar o backend e o frontend.
- Detalhes sobre a documentação e decisões técnicas.

## Conclusão

A separação dos repositórios reflete boas práticas, promovendo clareza e modularidade. Apesar do aumento na complexidade de gerenciamento, os benefícios superam os desafios no contexto deste projeto.
