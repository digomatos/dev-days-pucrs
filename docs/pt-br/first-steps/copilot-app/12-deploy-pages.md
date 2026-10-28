---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-28
---

Dê ao Space Quiz um endereço público publicando-o com o GitHub Pages. O Copilot pode preparar o workflow e orientar o deploy, mas você continua responsável por revisar as alterações e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a publicação no GitHub Pages.
- revisar o workflow e o pull request antes do merge.
- acompanhar o deploy e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site, que consiste em um único arquivo, com o GitHub Pages disponibiliza uma URL pública sem adicionar um servidor ou dependências de tempo de execução.

## Peça ao Copilot para publicar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Ative o GitHub Pages para este repositório. Crie um workflow do GitHub Actions que compile o site, se necessário, e faça o deploy no GitHub Pages. Adicione um README que descreva o aplicativo e explique como executá-lo localmente, caso ainda não exista. Crie um pull request com essas alterações e faça o merge depois que as verificações obrigatórias forem aprovadas. Acompanhe o deploy até ele ser concluído com sucesso e informe a URL final do site publicado. Se você não tiver permissão para ativar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe sucesso até que o site esteja no ar.
```

![Diagrama de três etapas: um pull request do space-quiz já mesclado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede que o agente crie e faça merge de um pull request e altere as configurações do Pages do repositório. Revise as alterações, as permissões de deploy e todos os avisos de aprovação do GitHub antes de continuar.

## Revise o workflow e verifique o deploy

1. Confirme que o README só foi adicionado se ainda não existia no repositório.
2. Revise as alterações do pull request, incluindo as etapas de build e deploy do workflow.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de deploy do GitHub Actions até ela ser concluída com sucesso.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não conseguir habilitar o Pages ou fazer o merge, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que o deploy seja bem-sucedido e o site esteja carregando.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou o deploy em vez de confiar apenas no relato do agente. Continue para a [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
