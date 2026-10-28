---
title: "Lição 2 - Criar e aprimorar o quiz"
description: "Crie um Space Quiz em um único arquivo e aprimore-o com o navegador integrado e o seletor de elementos."
authors:
  - jamesmontemagno
lastUpdated: 2026-09-28
---

Use um prompt detalhado para criar o Space Quiz, verificar seu comportamento no navegador integrado e fazer um ajuste visual com o seletor de elementos.

Nesta lição, você vai:

- criar um quiz sem dependências em `index.html`.
- testar o quiz no navegador integrado.
- inspecionar o código gerado.
- aprimorar um elemento selecionado preservando a acessibilidade.

## Criar o quiz

Envie o seguinte prompt na sessão `space-quiz`:

```plaintext
Crie um quiz de exploração espacial com 10 perguntas, uma barra de progresso, um contador de pontuação e feedback animado e colorido (verde para respostas corretas, tremor vermelho para respostas incorretas). Mostre uma tela de resultados com uma reação em emoji ao final. Centralize o conteúdo em uma coluna estreita. Use apenas um arquivo `index.html`, sem servidor nem dependências. Capriche no visual, use uma fonte sem serifa, corpo de 14–16 px e respeite `prefers-color-scheme`. Abra no navegador integrado.
```

Quando o agente terminar, responda a várias perguntas e confirme que:

- a barra de progresso avança.
- a pontuação é atualizada.
- as respostas corretas exibem um estado verde.
- as respostas incorretas usam uma animação de tremor em vermelho.
- a tela de resultados aparece após a última pergunta.

## Inspecionar o código gerado

Recolha a barra lateral esquerda e o painel do navegador à direita e revise `index.html` na área de código expandida. Observe como HTML, CSS e JavaScript trabalham juntos em um único arquivo. Restaure os dois painéis quando terminar.

## Aprimorar com a ferramenta de seleção de elementos

1. Na barra de ferramentas do navegador integrado, selecione a ferramenta de seleção de elementos.
2. Selecione o título do quiz ou a área de respostas.
3. Envie este prompt:

   ```plaintext
    Dê ao elemento selecionado a aparência de um painel de controle de missão. Preserve a acessibilidade e os temas claro e escuro.
   ```

4. Observe a atualização do navegador integrado e verifique a alteração.

## Melhorias opcionais

Se quiser continuar experimentando, peça ao agente para:

- adicionar um fundo sutil de estrelas que respeite `prefers-reduced-motion`.
- tornar a tela de resultados mais comemorativa quando a pontuação for 8 ou mais.
- melhorar os estados de foco do teclado e, depois, verificar o quiz sem usar o mouse.

## Resumo e próximos passos

Você criou, inspecionou e aprimorou o Space Quiz. Continue com a [Lição 3: Inspecionar a sessão e testar o quiz][next-lesson].

[next-lesson]: ../3-inspect-and-test/
