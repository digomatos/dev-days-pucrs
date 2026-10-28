---
title: "Lição 5 - Publicar o projeto"
description: "Transforme o experimento local do Space Quiz em um repositório público no GitHub."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-05
---

Publique o Space Quiz para gerenciar issues, usar worktrees isoladas e concluir um fluxo de trabalho com pull requests.

Nesta lição, você vai:

- inicializar a pasta como um repositório Git.
- criar um repositório público no GitHub e enviar o código para ele.
- vincular o projeto ao GitHub no aplicativo Copilot.

## Publicar o repositório

Envie o seguinte prompt:

```plaintext
Inicialize esta pasta como um repositório Git, crie um commit inicial e crie na minha conta um novo repositório público do GitHub chamado `space-quiz`. Envie a branch atual e defina-a como branch padrão. Atualize o projeto neste aplicativo para vinculá-lo ao GitHub.
```

> [!WARNING]
> Dependendo das configurações de aprovação, o agente pode pedir confirmação antes de criar um repositório ou enviar código, ou essas ações podem ser aprovadas automaticamente. Com **Approve all**, não espere um prompt de confirmação separado. Revise as configurações, a ação solicitada e o destino antes de enviar o prompt de publicação.

Quando o agente terminar:

1. Abra o novo repositório no GitHub.
2. Confirme que `index.html` está presente.
3. Volte ao aplicativo Copilot e confirme que o projeto está vinculado ao repositório.

## Resumo e próximos passos

Seu projeto agora é um repositório do GitHub. Continue com a [Lição 6: Trabalhar com issues e sessões][next-lesson].

[next-lesson]: ../6-issues-and-sessions/
