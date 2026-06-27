# NovaTech — Discovery do Assistente de IA para Atendimento ao Cliente

> Exportação completa da conversa de product discovery.
> Arquivos gerados listados ao final do documento.

---

## Prompt 1 — Jornada do Atendente

**Entrada do usuário:**

> Você é um Product Specialist responsável pelo discovery de um assistente de IA para atendimento ao cliente.
>
> **Contexto:**
> A NovaTech possui um assistente de IA baseado em documentação corporativa.
>
> **Dados do discovery:**
> - Os atendentes consultam em média 4 fontes por chamado.
> - 35% das dúvidas são sobre prazo de entrega.
> - 25% sobre regras de frete.
> - 20% sobre devoluções.
> - 20% outros assuntos.
> - Em 15% dos casos a resposta não é encontrada e o chamado é escalado ao supervisor.
>
> Crie uma jornada completa do atendente contendo:
> 1. Fluxo principal
> 2. Fluxo de fallback
> 3. Fluxo de feedback
> 4. Atores envolvidos
> 5. Pontos de decisão
> 6. Guardrails específicos para logística
>
> Organize em formato estruturado para posterior transformação em diagrama.

**Arquivo gerado:** `jornada_atendente_novatech` *(diagrama SVG inline)*

**Saída — estrutura documentada:**

### Atores envolvidos
| Ator | Papel |
|---|---|
| Atendente | Front-line — aciona e valida a IA |
| IA NovaTech | Assistente — consulta documentação corporativa |
| Supervisor | Escalada — resolve casos sem cobertura |
| Cliente | Solicitante — origina o chamado |

### Fluxo principal
1. Chamado recebido pelo cliente
2. Atendente abre a IA e digita a dúvida
3. **Ponto de decisão 1:** dúvida é sobre logística? (80%) ou outros assuntos (20%)
   - Logística: prazo (35%), frete (25%), devolução (20%)
4. IA consulta documentação — até 4 fontes por chamado
5. **Ponto de decisão 2:** resposta encontrada? (85% sim / 15% não)
6. Atendente recebe a resposta e revisa
7. **Ponto de decisão 3:** resposta é adequada?
   - Não → loop de refinamento da consulta → retorna ao passo 4
   - Sim → resposta entregue ao cliente
8. Chamado encerrado + coleta de feedback

### Fluxo de fallback (15% dos casos)
1. IA sinaliza ausência de resposta (documentação insuficiente)
2. Atendente notifica e encaminha ao supervisor
3. Supervisor resolve e documenta a lacuna de conhecimento
4. Base de conhecimento atualizada com o novo caso

### Fluxo de feedback
1. Atendente avalia a resposta: **Útil / Imprecisa / Incompleta**
2. IA registra o sinal (thumbs up / down)
3. Ciclo de melhoria: revisão semanal de padrões

### Guardrails específicos — logística
| Criticidade | Guardrail |
|---|---|
| Alta | Prazo de entrega: nunca prometer data sem consultar sistema |
| Alta | Frete: exibir sempre versão vigente com data de referência |
| Alta | Devolução: validar prazo legal (CDC) antes de orientar |
| Alta | Bloqueio automático: não responder sem fonte confirmada |
| Média | Alerta de desatualização: avisar se doc. tiver mais de 30 dias |

---

## Prompt 2 — Revisão de Riscos e Oportunidades

**Entrada do usuário:**

> Revise a jornada gerada.
> Identifique:
> - riscos operacionais
> - possíveis pontos de frustração
> - momentos de perda de confiança no assistente
> - oportunidades de melhoria
>
> Sugira ajustes para tornar a experiência mais segura para o atendimento ao cliente.

**Arquivo gerado:** `analise_riscos_jornada_novatech` *(painel HTML interativo inline)*

**Saída — achados consolidados:**

### Métricas da revisão
| Categoria | Quantidade |
|---|---|
| Riscos operacionais | 7 |
| Pontos de frustração | 5 |
| Rupturas de confiança | 4 |
| Oportunidades de melhoria | 6 |

### Riscos operacionais identificados
1. **Ausência de SLA de resposta da IA** — sem tempo máximo definido, atendente não distingue lentidão de travamento
2. **Validação humana opcional e subjetiva** — sem critério objetivo, respostas incorretas podem chegar ao cliente
3. **Documentação desatualizada sem aviso ativo** — guardrail de 30 dias é reativo; a IA pode responder com política vencida antes do alerta
4. **Lacuna de conhecimento identificada tarde** — 15% de escalada só revela gaps após o cliente ter esperado
5. **Loop de refinamento sem limite de iterações** — chamado pode ficar preso indefinidamente antes de escalar
6. **Supervisor como único fallback** — sem rota alternativa documentada se supervisor estiver indisponível
7. **Feedback não obrigatório** — etapa ignorada sob pressão, comprometendo melhoria contínua

### Pontos de frustração do atendente
1. Digitação manual da dúvida do cliente (retrabalho e erros de interpretação)
2. Sem visibilidade do raciocínio da IA (quais fontes foram consultadas)
3. Escalada sem histórico transferido ao supervisor
4. Triagem de categoria manual (80% logística deveria ser automático)
5. Nenhum indicador de confiança na resposta

### Momentos de perda de confiança na IA
1. Resposta encontrada mas imprecisa — descoberta tardia erode confiança
2. Silêncio no fallback — opaco se pesquisou as 4 fontes ou ocorreu erro técnico
3. Base de conhecimento desatualizada não visível ao atendente
4. Feedback sem fechamento de loop visível — avaliação vira burocracia

### Oportunidades de melhoria
1. **Score de confiança na resposta** — alta / média / baixa correspondência, com revisão obrigatória para baixa
2. **Rastreabilidade de fontes consultadas** — exibir documento e data de vigência
3. **Classificação automática de categoria** — 80% logística permite inferência por linguagem
4. **Limite de refinamento com escalada automática** — após 2 tentativas sem resultado
5. **Feedback integrado ao encerramento** — 1 clique obrigatório + retorno de impacto visível
6. **Supervisor substituto no fallback** — rota secundária para garantir desfecho de todo chamado

---

## Prompt 3 — Guardrails Específicos para RAG de Logística

**Entrada do usuário:**

> Com base no domínio de logística e atendimento ao cliente, sugira guardrails específicos para um assistente RAG corporativo.
> Evite guardrails genéricos.
> Explique o risco mitigado por cada um.

**Arquivo gerado:** `guardrails_rag_logistica_novatech` *(painel HTML interativo inline)*

**Saída — catálogo de guardrails:**

### Prazo de entrega

| # | Criticidade | Guardrail | Risco mitigado |
|---|---|---|---|
| 1 | Crítico | Bloqueio de data absoluta sem consulta ao sistema de rastreio | Documentação descreve SLAs médios, não prazos reais. Data errada gera expectativa falsa, reclamação no Procon e logística reversa. |
| 2 | Crítico | Distinção obrigatória entre prazo de postagem e prazo de entrega | Documentações misturam conceitos. "2 dias" sem distinção gera reclamação e chargeback quando cliente espera entrega no prazo de postagem. |
| 3 | Alto | Alerta automático para regiões com restrição operacional ativa | SLAs assumem operação normal. Em greves ou bloqueios, usar prazo padrão garante insatisfação — cliente aguarda entrega que não chegará no prazo. |

### Regras de frete

| # | Criticidade | Guardrail | Risco mitigado |
|---|---|---|---|
| 4 | Crítico | Versão vigente obrigatória com data explícita na resposta | RAG pode recuperar política revogada com threshold de frete grátis diferente do atual, gerando cobrança indevida ou desconto não autorizado. |
| 5 | Alto | Separação de frete por modalidade antes de calcular valor | Misturar regras de PAC com SEDEX gera valor incorreto — litígios no checkout e reclamações pós-compra. |
| 6 | Alto | Recusa de cálculo de frete quando peso ou dimensão não disponíveis | Estimativas sem dados reais criam comprometimento de preço que a empresa não pode honrar — divergência entre informado e cobrado. |

### Devoluções e trocas

| # | Criticidade | Guardrail | Risco mitigado |
|---|---|---|---|
| 7 | Crítico | Verificação de prazo legal (CDC) antes da política interna | Política interna pode ser mais restritiva que a lei, mas a lei prevalece. Negar devolução legal expõe a empresa a Procon, SENACON e multa por prática abusiva. |
| 8 | Alto | Distinção obrigatória entre devolução por arrependimento e por defeito | Tratamentos distintos em prazo, frete e fluxo operacional. Confundir aumenta custo operacional e insatisfação do cliente. |
| 9 | Médio | Bloqueio de orientação de devolução sem número de pedido confirmado | Orientações genéricas sem vínculo ao pedido real podem gerar logística reversa de produto não elegível ou registro duplicado que trava reembolso. |

### Comportamento do RAG — qualidade de recuperação

| # | Criticidade | Guardrail | Risco mitigado |
|---|---|---|---|
| 10 | Crítico | Score de confiança mínimo para exibição de resposta | RAGs sem threshold exibem respostas de documentos tangencialmente relacionados, levando o modelo a alucinar — perigoso quando números e prazos precisam ser exatos. |
| 11 | Alto | Proibição de interpolação entre documentos contraditórios | Documentos corporativos evoluem de forma não-linear. Escolha silenciosa entre fontes conflitantes gera inconsistência sistêmica não auditável. |
| 12 | Alto | Janela temporal de indexação com expiração automática por tipo de documento (TTL) | Tabelas de frete sem expiração mantêm valores revogados ativas e recuperáveis, gerando informação obsoleta com aparência de dado atual. |
| 13 | Médio | Detecção de pergunta composta com separação obrigatória antes de responder | RAGs respondem perguntas compostas com um único trecho, acertando parte e errando outra — e o atendente repassa tudo como resposta validada. |

### Proteção do cliente final

| # | Criticidade | Guardrail | Risco mitigado |
|---|---|---|---|
| 14 | Alto | Bloqueio de comprometimento de ação sem autorização do sistema | Comprometimentos antes de verificação geram falha de expectativa dupla — a empresa fica devendo promessa que não pode cumprir (cancelamento, reembolso, cupom). |
| 15 | Médio | Aviso de exceção regional antes de qualquer orientação de prazo ou frete | SLA de capital comunicado para cliente em zona rural ou de difícil acesso — prazo 2x maior — é o chamado mais custoso de corrigir: expectativa errada já comunicada. |

---

## Prompt 4 — Exportação da Conversa

**Entrada do usuário:**

> Extraia a conversa dessa chat em um arquivo markdown, com as entradas, prompts e documentos que forneci e como saida, nome de cada arquivo gerado.

**Arquivo gerado:** `novatech_discovery_conversa.md` *(este documento)*

---

## Índice de arquivos gerados

| Arquivo | Tipo | Prompt de origem | Descrição |
|---|---|---|---|
| `jornada_atendente_novatech` | SVG inline (diagrama) | Prompt 1 | Jornada completa do atendente com fluxo principal, fallback, feedback, atores e guardrails |
| `analise_riscos_jornada_novatech` | HTML inline (painel interativo) | Prompt 2 | Análise de riscos operacionais, frustrações, rupturas de confiança e oportunidades |
| `guardrails_rag_logistica_novatech` | HTML inline (painel interativo) | Prompt 3 | Catálogo de 15 guardrails específicos para RAG de logística com risco mitigado por cada um |
| `novatech_discovery_conversa.md` | Markdown (este arquivo) | Prompt 4 | Exportação completa da conversa de discovery |

---

*Conversa exportada em 06/06/2026 — NovaTech Product Discovery · Assistente de IA para Atendimento ao Cliente*
