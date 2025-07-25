# Guia de Implementação: Projeto Calçados Omega

Este guia contém os passos necessários para implementar completamente a aplicação Calçados Omega conforme solicitado no exercício.

## Passo 1: Criar o Projeto

1. Abra o Visual Studio ou Visual Studio Code.
2. Crie um novo projeto ASP.NET Core MVC:
    - Visual Studio: Arquivo > Novo > Projeto > ASP.NET Core Web App (Model-View-Controller)
    - Linha de comando: dotnet new mvc -n OmegaCalcados
3. Verifique se o projeto foi criado com a estrutura básica (Controllers, Models, Views, wwwroot).

**Referência:** [Seção 2.1: Criação do Projeto](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#21-cria%C3%A7%C3%A3o-do-projeto)

## Passo 2: Configurar o Layout Base

1. Localize o arquivo Views/Shared/_Layout.cshtml.
2. Atualize o layout para incluir o nome da empresa (Calçados Omega) e menu de navegação conforme necessário.
3. Assegure-se de que o layout inclua referências aos arquivos CSS e JavaScript necessários.

**Referência:** [Seção 5.2: Layouts](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#52-layouts)

## Passo 3: Implementar os Modelos

1. Crie os modelos necessários na pasta Models:
    
    - ModeloCalcado.cs: Para representar os modelos de calçado
    - TurnoProducao.cs: Para representar os turnos de produção
2. Adicione as propriedades e validações necessárias usando Data Annotations.
    

**Referência:**

- [Seção 4.1: Classes de Modelo](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#41-classes-de-modelo)
- [Seção 4.2: Data Annotations](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#42-data-annotations)

## Passo 4: Implementar Controllers

1. Crie os controllers necessários:
    
    - HomeController.cs: Para a página inicial
    - ModeloCalcadoController.cs: Para gerenciar modelos de calçado
    - TurnoProducaoController.cs: Para gerenciar turnos de produção
2. Implemente as actions necessárias em cada controller:
    
    - Index: Para listar itens
    - Details: Para mostrar detalhes de um item
    - Create: Para criar novos itens (GET e POST)
    - Outras actions específicas (como PorCategoria)

**Referência:**

- [Seção 3.1: Definição e Estrutura](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#31-defini%C3%A7%C3%A3o-e-estrutura)
- [Seção 3.2: Actions](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#32-actions)

## Passo 5: Implementar Views

1. Crie as views necessárias para cada controller:
    
    - Views/Home/Index.cshtml: Página inicial
    - Views/ModeloCalcado/Index.cshtml: Lista de modelos
    - Views/ModeloCalcado/Details.cshtml: Detalhes de um modelo
    - Views/TurnoProducao/Index.cshtml: Lista de turnos
    - Views/TurnoProducao/Create.cshtml: Formulário para criar turno
    - Views/TurnoProducao/Details.cshtml: Detalhes de um turno
2. Implemente views parciais para componentes reutilizáveis:
    
    - _ModeloCard.cshtml: Card para exibir um modelo

**Referência:**

- [Seção 5.1: Sintaxe Razor](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#51-sintaxe-razor)
- [Seção 5.3: Views Parciais](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#53-views-parciais)

## Passo 6: Configurar Rotas Personalizadas

1. Adicione rotas personalizadas no arquivo Program.cs:
    - Rota para categorias de modelos
    - Rota para detalhes de modelos
    - Mantenha a rota padrão

**Referência:**

- [Seção 6.1: Rotas Convencionais](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#61-rotas-convencionais)
- [Seção 6.2: Atributos de Rota](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#62-atributos-de-rota)

## Passo 7: Implementar Formulários e Validação

1. Crie formulários com validação para criar novos turnos de produção.
2. Implemente a validação client-side e server-side.
3. Exiba mensagens de erro apropriadas quando a validação falhar.

**Referência:**

- [Seção 7.1: Criação de Formulários](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#71-cria%C3%A7%C3%A3o-de-formul%C3%A1rios)
- [Seção 7.2: Validação com Data Annotations](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#72-valida%C3%A7%C3%A3o-com-data-annotations)
- [Seção 7.3: Validação no Controller](https://app.outlier.ai/playground/6883ef7b1c55c06befbcc85b#73-valida%C3%A7%C3%A3o-no-controller)

## Passo 8: Testar a Aplicação

1. Execute a aplicação (dotnet run ou F5 no Visual Studio).
2. Verifique se todas as páginas são carregadas corretamente.
3. Teste a navegação entre as páginas.
4. Teste a criação de novos turnos.
5. Verifique se as validações estão funcionando corretamente.

## Passo 9: Aprimoramentos Finais

1. Adicione feedback visual (mensagens de sucesso/erro).
2. Melhore o estilo visual conforme necessário.
3. Garanta que a aplicação seja responsiva (funcione bem em dispositivos móveis).
4. Certifique-se de que o código está organizado e segue as boas práticas.

**Observação:** Como este é um exercício didático, os dados estão sendo armazenados em memória. Em uma aplicação real, você utilizaria um banco de dados (como mostrado na seção 8).