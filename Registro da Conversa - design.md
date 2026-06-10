# Registro da Conversa — Design de Jornada

**Projeto:** Design de jornada
**Tema:** Jornada do Atendente — Assistente de IA NovaTech
**Data:** Junho de 2026

---

## 1. Entradas e prompts fornecidos pelo usuário

### Prompt inicial (m0001)
> Vou te enviar essa jornada textual. Quero que crie um fluxo visual profissional para apresentação executiva.
>
> O fluxo deve possuir três caminhos:
> 1. Fluxo Principal
> 2. Fluxo de Fallback
> 3. Fluxo de Feedback
>
> Utilize:
> - decisões visuais
> - ícones
> - setas
> - agrupamento por cores
>
> O público é composto por:
> - cliente
> - Product Manager
> - equipe de atendimento
>
> O diagrama deve ser legível para pessoas não técnicas.

### Documento fornecido — Nota de uso no Claude Design (m0003)
> Para o diagrama, sugerimos indicar que os três caminhos devem ser apresentados em
> cores distintas — por exemplo **teal** para o fluxo principal, **âmbar** para o fallback
> e **roxo** para o feedback — e que os pontos de decisão (**D1 a D7**) sejam representados
> como **losangos**, enquanto as ações de cada ator usam **retângulos com a cor do ator
> correspondente**.

### Pedidos seguintes
- **(m0013)** Que tipo de arquivo de saída você pode gerar deste diagrama?
- **(m0014)** Gere uma imagem PNG do diagrama.
- **(m0032)** Estou aguardando a imagem PNG do diagrama.
- **(m0035)** Extraia a conversa dessa chat em um arquivo markdown, com as entradas,
  prompts e documentos que forneci e, como saída, o nome de cada arquivo gerado.

---

## 2. Sistema visual definido (a partir das instruções)

| Elemento | Definição |
|---|---|
| **Fluxo Principal** | Cor **teal** |
| **Fluxo de Fallback** | Cor **âmbar** |
| **Fluxo de Feedback** | Cor **roxo** |
| **Decisões (D1–D7)** | **Losangos** |
| **Ações** | **Retângulos** com a cor do ator |
| **Início / Fim** | Pílulas |
| **Gatilho** | Caixa com borda tracejada |
| **Atores** | Cliente, Atendente, IA NovaTech, Supervisor, Sistema · IA/TI (ícone + cor próprios) |

**Pontos de decisão mapeados:** D1 Categoria da dúvida · D2 Resposta encontrada ·
D3 Score de confiança · D4 Resposta adequada · D5 Tentativas ≥ 2 ·
D6 Supervisor disponível · D7 Motivo do feedback.

---

## 3. Arquivos gerados (saídas)

| Arquivo | Tipo | Descrição |
|---|---|---|
| **`Jornada do Atendente.html`** | HTML interativo | Diagrama-pôster com os 3 caminhos coloridos, decisões D1–D7 em losangos, ícones por ator, setas e conectores entre caminhos, filtros clicáveis (destacar cada caminho), legenda, tabela-resumo das decisões e cards de valor. |
| **`Jornada do Atendente.png`** | Imagem PNG | Exportação estática do diagrama completo, **1480 × 3004 px**, alta resolução — pronta para inserir em slides, documentos ou e-mail. |

> Observação: durante o processo também foram criados arquivos temporários de apoio à
> exportação (`_export_v3.html`, `_test_scaled.png`, `diagram.js`), que foram **removidos**
> ao final. Apenas os dois arquivos acima são as entregas finais.

---

## 4. Outros formatos de saída disponíveis (oferecidos)

A partir do mesmo diagrama, também é possível gerar:

- **PDF** — para impressão / envio / ata.
- **PPTX (PowerPoint)** — apresentação executiva editável (fluxo completo ou um caminho por slide).
- **HTML standalone** — arquivo único offline que preserva a interatividade (filtros e hover).
- **Canva** — design editável pela equipe.
