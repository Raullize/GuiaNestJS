# 🏗️ Arquitetura e Conceitos Fundamentais

A arquitetura do NestJS é um dos seus maiores diferenciais. Fortemente inspirada no Angular, ela foi projetada para resolver o "problema da arquitetura" no ecossistema Node.js, onde a falta de padrões frequentemente resulta em códigos difíceis de manter e escalar.

O NestJS propõe uma estrutura opinativa (opinionated), baseada em **Injeção de Dependências** e **Modularidade**, separando claramente as responsabilidades de cada parte da aplicação.

---

## 🧩 Os Três Pilares do NestJS

Para entender o fluxo de uma aplicação Nest, você precisa conhecer seus três componentes principais. Eles formam a base de qualquer recurso que você criar:

### 1. Controllers (Controladores)
A camada responsável por **receber as requisições HTTP** do cliente (Frontend, App Mobile, etc.) e **retornar as respostas**.
- **O que eles fazem:** Lidam com rotas (ex: `/users`), leem parâmetros da URL (`@Param()`), extraem dados do corpo da requisição (`@Body()`) e definem o status HTTP de retorno.
- **O que NÃO fazem:** Não devem conter regras de negócio complexas ou acessar o banco de dados diretamente. Eles delegam esse trabalho para os Services.

[Saiba mais sobre Controllers →](controllers.md)

### 2. Providers / Services (Provedores / Serviços)
É aqui que vive a **Regra de Negócio** (Business Logic) da sua aplicação. 
- **O que eles fazem:** Processam dados, comunicam-se com bancos de dados, chamam APIs externas, etc. 
- **A Mágica da Injeção de Dependência (DI):** O NestJS possui um contêiner de Inversão de Controle (IoC) integrado. Quando você decora uma classe com `@Injectable()`, o NestJS entende que essa classe pode ser injetada em outras.
  - **Exemplo prático:** Em vez de você fazer `const service = new UsersService()` dentro do seu Controller (o que criaria um alto acoplamento), você simplesmente declara o `UsersService` no construtor do Controller. O NestJS, por debaixo dos panos, cria a instância do Service e a entrega (injeta) pronta para uso.
  - **Vantagens:** Isso facilita imensamente a criação de **Testes Unitários** (pois você pode injetar "mocks" no lugar dos serviços reais) e permite reaproveitar a mesma instância de um serviço em vários lugares da aplicação (padrão Singleton).

[Saiba mais sobre Providers →](providers.md)

### 3. Modules (Módulos)
Os módulos são a forma do NestJS **organizar e agrupar** o código por domínio ou funcionalidade. 
- **O que eles fazem:** Eles "empacotam" Controllers e Providers relacionados (ex: um `UsersModule` agrupará `UsersController` e `UsersService`). Toda aplicação Nest tem pelo menos um módulo raiz (geralmente chamado `AppModule`).
- **Encapsulamento:** Por padrão, os Providers dentro de um módulo são encapsulados (privados). Para usá-los em outro lugar, o módulo precisa exportá-los explicitamente.

[Saiba mais sobre Modules →](modules.md)

---

## 🔄 O Ciclo de Vida da Requisição (Request Lifecycle)

No Express puro, o fluxo de uma requisição geralmente passa por middlewares até chegar ao manipulador da rota. O NestJS expande isso, introduzindo várias camadas de processamento (inspiradas em padrões de design) para manter o código limpo e organizado.

Quando uma requisição chega, ela passa por essas etapas (nesta ordem):

1. **Middlewares:** Funções executadas antes de qualquer outra coisa. Ótimas para logging, parsing de headers ou validações globais simples (semelhante ao Express).
2. **Guards (Guardas):** Onde a **Autenticação** e **Autorização** devem ocorrer. Eles decidem se a requisição tem permissão para continuar (ex: "O usuário tem token JWT? Tem a role de 'admin'?").
3. **Interceptors (Interceptadores) - Pré-Controller:** Podem manipular a requisição antes de chegar ao Controller (ex: iniciar um timer para medir a performance).
4. **Pipes:** Fazem a **Transformação** (ex: converter uma string '1' para o número 1) e a **Validação** dos dados de entrada (ex: garantir que o e-mail passado no corpo da requisição é válido).
5. **Controller:** Recebe os dados validados, chama o Service adequado e recebe o resultado.
6. **Service:** Executa a regra de negócio (acessa banco, etc.) e devolve o dado ao Controller.
7. **Interceptors (Interceptadores) - Pós-Controller:** Podem manipular a resposta do Controller antes de ser enviada ao cliente (ex: mascarar senhas, mapear formatos, calcular o tempo final de execução).
8. **Exception Filters (Filtros de Exceção):** Se algum erro ocorrer em qualquer uma das etapas acima (ex: `HttpException`), esse filtro captura o erro e formata uma resposta amigável e padronizada para o cliente.

---

## 🎯 Por que tanta separação?

Pode parecer muita coisa no começo, mas essa separação tem um propósito claro (o princípio de **Single Responsibility** - Responsabilidade Única):

- Se o problema é permissão, você olha nos **Guards**.
- Se os dados estão chegando com o formato errado, você olha nos **Pipes**.
- Se precisa alterar como os dados são retornados (ex: envelopar tudo em `{ data: ... }`), você usa um **Interceptor**.
- Se a regra de calcular um desconto mudar, você altera o **Service**.

Isso torna a aplicação **altamente testável**, fácil de dar manutenção em equipe e escalável a longo prazo!

---

⬅️ [Voltar ao Início](../README.md)