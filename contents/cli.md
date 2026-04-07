# 🛠️ NestJS CLI (Command Line Interface)

O **NestJS CLI** é uma ferramenta de linha de comando poderosa que ajuda a inicializar, desenvolver e manter aplicações Nest de forma rápida e eficiente. Ele automatiza tarefas repetitivas, como a criação de arquivos boilerplate, e garante que o seu código siga as convenções e a estrutura recomendadas pelo framework.

> ⚠️ **Nota Importante:** O uso do NestJS CLI **NÃO é obrigatório**.
> Você pode perfeitamente criar todos os arquivos (módulos, controladores, serviços, etc.) manualmente do zero. No entanto, usar a CLI é altamente recomendado, pois ela economiza muito tempo, evita erros de digitação e garante que a arquitetura do projeto permaneça consistente e padronizada.

---

## 📥 Instalação

Para utilizar o CLI globalmente na sua máquina, instale-o via npm (ou yarn/pnpm):

```bash
npm i -g @nestjs/cli
```

Após a instalação, você pode verificar se tudo ocorreu bem digitando:

```bash
nest --version
```

---

## 🚀 Comandos Básicos

Aqui estão os comandos mais comuns para o dia a dia de desenvolvimento:

### Criar um novo projeto
Gera a estrutura inicial de um projeto NestJS, incluindo configurações de TypeScript, linting e testes.
```bash
nest new nome-do-projeto
```

### Iniciar a aplicação (Modo Desenvolvimento)
Inicia o servidor em modo de observação (watch mode). Qualquer alteração no código reiniciará o servidor automaticamente.
```bash
nest start --watch
```

### 🆘 Obtendo Ajuda (Help)
O CLI possui um sistema de ajuda integrado muito útil. Se você esquecer algum comando ou quiser ver todas as opções disponíveis para um comando específico, use:
```bash
# Lista todos os comandos e opções gerais disponíveis
nest --help

# Mostra ajuda para um comando específico (ex: generate)
nest generate --help
```

---

## 🏗️ Generators (Geradores de Código)

A verdadeira mágica do CLI está nos geradores. Eles permitem criar diferentes blocos de construção da sua aplicação com um único comando. A sintaxe base é `nest generate <tipo> <nome>` (ou simplesmente `nest g <tipo> <nome>`).

| Comando Completo | Atalho | O que faz? |
| :--- | :--- | :--- |
| `nest generate module users` | `nest g mo users` | Cria um novo Módulo (`users.module.ts`). |
| `nest generate controller users` | `nest g co users` | Cria um Controlador e atualiza o Módulo mais próximo. |
| `nest generate service users` | `nest g s users` | Cria um Serviço e o injeta no Módulo correspondente. |
| `nest generate class users` | `nest g cl users` | Cria uma classe simples. |
| `nest generate interface users` | `nest g itf users` | Cria uma interface TypeScript. |
| `nest generate pipe custom` | `nest g pi custom` | Cria um Pipe personalizado. |
| `nest generate guard auth` | `nest g gu auth` | Cria um Guard de autenticação. |

### 🌟 O Comando "Mágico": Resource

Se você precisar criar um CRUD completo (Módulo, Controlador, Serviço, Entidades e DTOs) de uma só vez, o CLI tem um atalho perfeito:

```bash
nest g resource users
```
*Ele perguntará se você está usando REST API, GraphQL, Microservices, etc., e gerará todo o boilerplate para você.*

---

## 🛠️ Opções Úteis ao Gerar Arquivos

- `--no-spec`: Impede a criação do arquivo de testes unitários (`.spec.ts`).
  *Ex: `nest g s users --no-spec`*
- `--dry-run`: Simula a criação dos arquivos sem de fato criá-los (ótimo para testar se os arquivos serão gerados na pasta correta).
  *Ex: `nest g co users --dry-run`*

---

⬅️ [Voltar ao Início](../README.md)