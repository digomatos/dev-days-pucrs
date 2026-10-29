---
title: "Lição 9 - Explorar e criar canvases"
description: "Use o canvas Database Explorer existente e depois crie e revise um canvas de triagem vinculado ao repositório."
authors:
  - geektrainer
lastUpdated: 2026-09-17
---

Até agora, você orientou agentes pelo chat. No entanto, grande parte do trabalho não acontece em uma conversa, mas em um quadro, documento ou checklist. Os **canvases** oferecem a você e ao agente uma superfície compartilhada exatamente para esse tipo de trabalho, dentro do aplicativo. Nesta lição, você primeiro usará um canvas incluído na Tailspin Toys e depois criará um para o backlog no qual vem trabalhando.

Nesta lição, você vai:

- entender o que é um canvas e quando usá-lo.
- usar o canvas Database Explorer existente para examinar dados do projeto.
- criar um canvas compartilhado de quadro Kanban para fazer a triagem do backlog.
- examinar e testar o novo canvas sem implementar outro recurso.

## Cenário

A Tailspin Toys já inclui um canvas para explorar seu banco de dados. Depois de usá-lo para entender como um canvas transforma dados do projeto em uma superfície interativa, você criará um quadro reutilizável para escolher o próximo trabalho sem iniciar outro recurso.

## O que é um canvas?

Um [canvas][canvas-docs] é uma superfície interativa e compartilhada para um artefato de trabalho, como um plano, um quadro de triagem, um checklist de lançamento, um painel ou um documento. Embora o chat seja ótimo para descrever intenções e analisar ambiguidades, a maior parte do trabalho acontece em uma *superfície*. Os canvases permitem colaborar com o agente diretamente nessa superfície.

Os canvases são **bidirecionais**: o agente pode atualizar o canvas enquanto trabalha, e você pode editar a mesma superfície. Quando você cria um canvas, o agente o desenvolve com base no prompt e no fluxo de trabalho. Você pode pedir que ele adicione, remova ou revise recursos durante o processo. Depois de criado, o canvas é aberto no painel direito do aplicativo.

Alguns exemplos comuns incluem:

- **Canvases Markdown** para planejar o dia e priorizar issues e pull requests.
- **Quadros Kanban agênticos** nos quais pessoas e agentes adicionam cards e movem o trabalho entre colunas.
- **Quadros de triagem de issues** que resumem as principais issues e os temas recorrentes de um repositório.

## Por que usar um canvas?

Use um canvas quando uma tarefa exigir estrutura, iteração e verificação e o chat não for suficiente. Um canvas permite:

- fundamentar o trabalho do agente em um artefato real adequado ao seu fluxo de trabalho.
- orientar ou corrigir o trabalho diretamente na superfície compartilhada e depois permitir que o agente continue a partir das suas alterações.
- acompanhar o progresso como alterações visíveis em um artefato, e não apenas como respostas no chat.

## Usar o canvas Database Explorer

Comece pelo canvas Database Explorer existente no projeto. Usar um exemplo funcional permite observar como um canvas com escopo de repositório se comporta antes de criar o seu.

### Preparar e abrir o canvas

1. Confirme que o pull request (PR) da filtragem foi integrado e atualize sua branch `main` local.
2. Volte ao aplicativo GitHub Copilot e selecione **+** ao lado de **Sessions** na barra lateral esquerda para criar uma nova sessão.
3. Selecione o repositório **tailspin-toys**.
4. Crie a sessão em **New worktree**, usando a `main` atualizada como branch de origem.
5. Selecione o modo **Interactive** e peça ao Copilot que prepare as dependências e o banco de dados local usando os scripts do projeto:

      ```plaintext
      Inspect the project's package scripts, install its dependencies, and use its existing database setup and seed scripts if the local database has not already been initialized. Preserve any existing database and its contents. Check that the scripts and the existing Database Explorer extension use the same database. You may install dependencies and configure necessary local files, but do not modify application or extension code, overwrite the database, commit, push, or open a pull request. Report what you ran, the database paths, and any blockers before opening the canvas.
      ```

      **Tradução:** Examine os scripts do projeto, instale as dependências e use os scripts existentes de configuração e seed do banco de dados se o banco local ainda não tiver sido inicializado. Preserve qualquer banco existente e seu conteúdo. Confira se os scripts e a extensão Database Explorer existente usam o mesmo banco. Você pode instalar dependências e configurar os arquivos locais necessários, mas não deve modificar o código da aplicação ou da extensão, sobrescrever o banco, fazer commit, push ou abrir um pull request. Informe o que executou, os caminhos dos bancos e quaisquer bloqueios antes de abrir o canvas.

6. Revise a saída dos scripts e os caminhos dos bancos. O laboratório usou os scripts de instalação de dependências e `npm run db:setup` do projeto. Examine os scripts antes de executar comandos de configuração ou seed sobre um banco existente; não o redefina nem sobrescreva. Se o carregamento falhar ou os caminhos dos bancos forem diferentes, peça ao Copilot que investigue o problema antes de continuar.
7. Depois de instalar as dependências e configurar o caminho do banco local, execute `/restart-session` para recarregar a extensão na mesma sessão, preservando o histórico e o mesmo worktree. Não crie outra sessão ou worktree.
8. Peça ao Copilot que abra o canvas existente e confira suas tabelas e seus dados:

      ```plaintext
      Open the repository's existing Database Explorer canvas. Use its read-only actions to list the tables and verify the seeded data. Do not modify application or extension code, overwrite the database, commit, push, or open a pull request.
      ```

      **Tradução:** Abra o canvas Database Explorer existente no repositório. Use suas ações somente leitura para listar as tabelas e verificar os dados do seed. Não modifique o código da aplicação ou da extensão, sobrescreva o banco, faça commit, push ou abra um pull request.

9. Confirme visualmente que o painel do Database Explorer abre e exibe suas tabelas e seus dados. Para o seed utilizado no laboratório, o resultado esperado foi de **21 jogos, 5 categorias e 4 publishers**. A tabela `__drizzle_migrations` também apareceu. Selecione `games` e examine suas linhas; use as ações somente leitura do canvas para conferir as contagens. Investigue diferenças em vez de sobrescrever um banco existente para forçar esses resultados.
10. Execute uma consulta somente leitura que mostre cinco jogos com as melhores avaliações:

      ```sql
      SELECT title, star_rating
      FROM games
      ORDER BY star_rating DESC
      LIMIT 5;
      ```

11. Confirme que os resultados contêm no máximo cinco jogos em ordem decrescente de avaliação.
12. Abra **Files** e examine `.github/extensions/database-explorer/extension.mjs`. Observe como o canvas é armazenado com o projeto e restringe as consultas a instruções `SELECT` e `WITH` somente leitura.
13. Revise as alterações locais e confirme que o código da aplicação e da extensão permanece inalterado. A instalação de dependências e a configuração do banco podem criar arquivos locais; não faça commit, push ou abra um PR para essa configuração.

> [!NOTE]
> Durante o teste do laboratório, as tabelas e os dados foram verificados pelas ações do próprio canvas, sem inspeção visual do painel renderizado. Isso confirma as verificações dos dados, não a renderização visual do painel. Confirme o painel visualmente antes de considerar essa parte do exercício concluída. Use o endereço e a porta informados pela sua sessão, se necessário; nenhum dos dois é fixo.

## Criar um canvas para fazer a triagem de issues

Agora, crie outro tipo de superfície compartilhada. Salvar o canvas de triagem no escopo do projeto faz com que ele se torne um recurso do repositório que a equipe pode revisar e reutilizar.

1. Na mesma sessão, digite `/create-canvas` e descreva o canvas que deseja criar:

   ```plaintext
   Create a Kanban triage canvas for this repo's open issues and save it under .github/extensions/. Highlight the three issues you'd prioritize and explain why, with the rest below. Include summaries and links.

   Give each card an "Add to current context" action that adds the issue details without starting work or changing the issue. Make it keyboard-accessible and open it so I can try it.
   ```

      **Tradução:** Crie um canvas Kanban para a triagem das issues abertas deste repositório e salve-o em `.github/extensions/`. Destaque as três issues que você priorizaria e explique o motivo, com as demais abaixo. Inclua resumos e links.

      Adicione a cada card uma ação "Add to current context" (adicionar ao contexto atual) que inclua os detalhes da issue sem iniciar o trabalho nem alterar a issue. Torne o canvas acessível por teclado e abra-o para que eu possa experimentá-lo.

O Copilot cria a extensão de canvas em `.github/extensions` e abre a superfície compartilhada no painel direito do aplicativo. A extensão gerada é conteúdo executável do repositório, não apenas um artefato visual, então você examinará seus arquivos e seu comportamento em seguida.

## Inspecionar e exercitar o canvas

Antes de compartilhar o canvas, compare-o com as issues reais do repositório e teste seus controles. Isso confirma que o conteúdo é preciso, que a interação é acessível e que a ação da issue adiciona contexto sem iniciar o trabalho.

1. Abra **Changes** e confirme que a definição do canvas está vinculada ao repositório em `.github/extensions/`, e não salva apenas para seu usuário ou sessão. Verifique se as extensões existentes e os arquivos da aplicação permanecem inalterados.
2. Compare o quadro com as issues abertas reais e avalie as explicações da classificação.
3. Verifique se os cards e controles são legíveis e utilizáveis por teclado.
4. Selecione **Add to current context** em uma issue e confirme que apenas seus detalhes entram na conversa. Nenhuma implementação ou alteração de estado da issue deve começar.
5. Revise as correções e peça ao Copilot que execute a validação existente aplicável aos arquivos alterados. Registre resultados e bloqueios, em vez de presumir que uma superfície interativa está correta apenas porque foi aberta.
6. Se o canvas precisar de alterações, solicite melhorias específicas dentro do escopo da triagem e repita as verificações afetadas. Não implemente uma das issues do backlog como parte deste trabalho de canvas.

O workshop termina antes da criação de outro PR porque você já praticou o merge manual e o Agent Merge. Em produção, revise e faça o merge do canvas pelo processo normal da sua equipe antes que outras pessoas dependam dele.

## Resumo e próximos passos

Você criou uma superfície compartilhada na qual você e o agente podem colaborar. Você:

- entendeu o que é um canvas e quando usá-lo.
- usou o canvas Database Explorer existente para examinar dados do projeto.
- criou um canvas compartilhado de quadro Kanban para fazer a triagem do backlog.
- examinou e testou o novo canvas sem implementar outro recurso.

Com o backlog acompanhado, você [revisará tudo o que criou e explorará os próximos passos][next-lesson].

## Recursos

- [Trabalhar com extensões de canvas no aplicativo GitHub Copilot][canvas-docs]
- [Canvases no Awesome Copilot][awesome-copilot-canvases]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../10-review/
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[awesome-copilot-canvases]: https://awesome-copilot.github.com/extensions/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app