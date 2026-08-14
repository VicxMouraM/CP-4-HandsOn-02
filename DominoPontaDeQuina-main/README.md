# Domino Ponta de Quina

Projeto desenvolvido em C# com .NET 8 e Entity Framework Core para implementar a persistência dos dados de um jogo de dominó.

## Objetivo

Evoluir o modelo de dados do jogo de dominó utilizando o Entity Framework Core, configurando as entidades, o contexto do banco de dados e as migrations.

Nesta etapa foram trabalhados:

* Configuração de `Jogador` utilizando Data Annotations.
* Configuração de `Usuario` utilizando Fluent API.
* Utilização das convenções do EF Core para a entidade `Jogo`.
* Implementação do `DominoDbContext`.
* Configuração do banco de dados SQLite.
* Criação e aplicação da migration inicial.

## Estrutura do Projeto

O projeto está dividido em diferentes camadas:

* `DominoPontaDeQuina.Core`: contém as regras e o fluxo do jogo.
* `DominoPontaDeQuina.Domain`: contém as entidades e enums persistentes.
* `DominoPontaDeQuina.Repository`: contém o `DominoDbContext`, os mapeamentos Fluent API e os repositórios do Entity Framework Core.
* `DominoPontaDeQuina.Migrations`: projeto utilizado como startup project para criação e execução das migrations.
* `DominoPontaDeQuina.Tests`: contém os testes automatizados do núcleo do jogo.

## Modelo Persistente

### Usuario

Representa a conta do aplicativo cliente.

Um `Usuario` pode possuir vários `Jogador`, representando os perfis de jogo associados à conta.

A entidade `Usuario` é configurada utilizando **Fluent API** no `DominoDbContext`.

### Jogador

Representa o perfil de um jogador associado a um usuário.

A entidade `Jogador` utiliza **Data Annotations** para configuração das propriedades e do relacionamento com `Usuario`.

### Jogo

Representa uma partida armazenada para consulta do histórico.

A entidade `Jogo` utiliza as **convenções padrão do Entity Framework Core**, sem configurações adicionais específicas.

### ParticipacaoJogo

Relaciona um `Jogador` a um `Jogo`, registrando informações como posição, pontuação e resultado da participação.

## Entity Framework Core

O projeto utiliza o Entity Framework Core para realizar o mapeamento entre as entidades C# e o banco de dados.

O `DominoDbContext` contém os seguintes `DbSet`:

```csharp
DbSet<Usuario>
DbSet<Jogador>
DbSet<Jogo>
DbSet<ParticipacaoJogo>
```

O relacionamento entre `Usuario` e `Jogador` é configurado através da Fluent API.

## Banco de Dados

Foi utilizado o **SQLite** como banco de dados.

O banco é criado localmente no arquivo:

```text
domino.db
```

O arquivo do banco de dados é ignorado pelo Git.

## Pré-requisitos

* .NET 8 SDK
* Entity Framework Core
* Ferramenta `dotnet-ef` 8.x

Para instalar o `dotnet-ef`:

```bash
dotnet tool install --global dotnet-ef --version 8.*
```

Para verificar a instalação:

```bash
dotnet ef --version
```

## Restaurar o Projeto

Após clonar o repositório, restaure as dependências:

```bash
dotnet restore
```

## Compilar

Para compilar toda a solução:

```bash
dotnet build
```

## Migrations

O projeto utiliza:

* `DominoPontaDeQuina.Repository` como projeto que contém o `DominoDbContext`.
* `DominoPontaDeQuina.Migrations` como startup project para as migrations.

### Criar uma Migration

Para criar a migration inicial:

```bash
dotnet ef migrations add Inicial \
  --project DominoPontaDeQuina.Repository \
  --startup-project DominoPontaDeQuina.Migrations
```

### Aplicar a Migration

Para criar/atualizar o banco de dados:

```bash
dotnet ef database update \
  --project DominoPontaDeQuina.Repository \
  --startup-project DominoPontaDeQuina.Migrations
```

Após a execução, o banco SQLite `domino.db` será criado ou atualizado de acordo com as migrations existentes.

## Fluxo de Execução

A sequência recomendada para configurar o projeto é:

```bash
dotnet restore
dotnet build
dotnet ef migrations add Inicial --project DominoPontaDeQuina.Repository --startup-project DominoPontaDeQuina.Migrations
dotnet ef database update --project DominoPontaDeQuina.Repository --startup-project DominoPontaDeQuina.Migrations
```

## Escopo

Esta etapa tem como foco a persistência dos dados utilizando Entity Framework Core.

API, endpoints, autenticação e JWT estão fora do escopo desta etapa.

## Tecnologias

* C#
* .NET 8
* Entity Framework Core
* SQLite
* Data Annotations
* Fluent API
* EF Core Migrations
