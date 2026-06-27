# Análise comparativa: PROC-042 v1 × PROC-042 v2
*Transcrição da sessão de análise documental — Discovery Assistente de IA NovaTech*

---

## Documentos fornecidos

| Arquivo | Descrição |
|---|---|
| `PROC-042___Procedimento_de_Cálculo_de_Frete_Especial.docx` | PROC-042 v1.0 — Procedimento de Cálculo de Frete Especial, emitido em 03/03/2023 pela Diretoria Comercial |
| `PROC-042-v2___Procedimento_de_Cálculo_de_Frete_Especial.docx` | PROC-042 v2 — Procedimento revisado de Cálculo de Frete Especial, emitido em 10/11/2023 pela Diretoria Comercial |

---

## Prompt

**Entrada do usuário:**

> Com base na análise anterior, os documentos mais críticos são: PROC-042 e PROC-042-v2.
> Analise os conteúdos completos dos dois documentos.
> Identifique:
>
> 1. Diferenças de regras.
> 2. Possíveis conflitos.
> 3. Impactos para um assistente de IA.
> 4. Riscos operacionais.
> 5. Perguntas que devem ser feitas aos stakeholders durante o discovery.
>
> Produza uma tabela comparativa da análise de inconsistências.

**Arquivos anexados:** PROC-042 v1, PROC-042 v2

---

## Saída

### Conteúdo extraído dos documentos

#### PROC-042 v1.0 (03/03/2023)

**Fórmula:** `Valor do frete = Valor base × Multiplicador regional × Fator de peso`

**Fatores de peso:**

| Faixa de peso | Fator |
|---|---|
| 500–1.000 kg | 1.0 |
| 1.001–3.000 kg | 1.2 |
| Acima de 3.000 kg | 1.5 |

**Multiplicadores regionais:**

| Região | Multiplicador |
|---|---|
| Sudeste | 1.0 |
| Sul | 1.2 |
| Centro-Oeste | 1.3 |
| Nordeste | 1.4 |
| Norte | 1.6 |

**Prazo de entrega:** prazo padrão da rota + 2 dias úteis

**Condições especiais:**
- Cargas acima de 5.000 kg: aprovação prévia do gerente de operações regional
- Cargas perigosas (+500 kg): seguem o PROC-043 (não esta fórmula)
- Descontos por volume: clientes com mais de 10 fretes especiais/mês podem negociar descontos diretamente com o Comercial, formalizados em aditivo contratual

---

#### PROC-042 v2 (10/11/2023)

**Fórmula:** idêntica à v1 — `Valor do frete = Valor base × Multiplicador regional × Fator de peso`

**Fatores de peso (atualizados):**

| Faixa de peso | Fator v1 | Fator v2 | Variação |
|---|---|---|---|
| 500–1.000 kg | 1.0 | 1.0 | Sem alteração |
| 1.001–3.000 kg | 1.2 | 1.15 | –4,2% |
| Acima de 3.000 kg | 1.5 | 1.4 | –6,7% |

**Multiplicadores regionais (novos valores):**

| Região | v1 | v2 | Variação |
|---|---|---|---|
| Sudeste | 1.0 | 1.1 | +10% |
| Sul | 1.2 | 1.3 | +8,3% |
| Centro-Oeste | 1.3 | 1.4 | +7,7% |
| Nordeste | 1.4 | 1.5 | +7,1% |
| Norte | 1.6 | 1.8 | +12,5% |

**Prazo de entrega:** prazo padrão da rota + 3 dias úteis (antes: +2)

**Condições especiais:**
- Cargas acima de 5.000 kg: mantida aprovação prévia do gerente de operações regional
- Cargas perigosas (+500 kg): seguem o PROC-043 (citado como "em revisão pelo Compliance")
- Descontos por volume (novas regras com percentuais fixos):
  - 8 a 15 fretes especiais/mês: 5% sobre o multiplicador regional
  - Acima de 15 fretes especiais/mês: 10% sobre o multiplicador regional
  - Descontos maiores: aprovação expressa da Diretoria Comercial

**Regra de transição:**
- Chamados abertos **antes de 01/12/2023** ainda em processamento: usar multiplicadores da v1
- Chamados criados **a partir de 01/12/2023**: aplicar obrigatoriamente as taxas da v2

---

### 1. Diferenças de regras

A estrutura da fórmula é idêntica em ambas as versões, mas todos os parâmetros numéricos foram alterados na v2, em direções opostas:

- **Fatores de peso:** caíram nas duas faixas superiores (–4,2% e –6,7%)
- **Multiplicadores regionais:** subiram em todas as cinco regiões (+7,1% a +12,5%)
- **Prazo de entrega:** acrescido de 1 dia útil na v2 (+3 dias em vez de +2)
- **Descontos por volume:** regime completamente diferente — de negociação livre com aditivo contratual (v1) para percentuais fixos (5% e 10%) com limiar reduzido de 10 para 8 fretes/mês (v2)
- **Regra de transição:** presente apenas na v2; inexistente na v1

O efeito líquido no valor final do frete depende da combinação região × faixa de peso. Em algumas combinações a v2 resulta em frete mais caro; em outras pode ser mais barato porque o fator de peso caiu mais do que o multiplicador regional subiu.

---

### 2. Possíveis conflitos

**Conflito de governança (raiz):** a v2 foi emitida pela mesma Diretoria Comercial que emitiu a v1, mas não contém cláusula de revogação, não referencia a v1 como "substituída" e coexiste no mesmo repositório SharePoint sem diferenciação de status. Qualquer sistema de busca — humano ou automatizado — encontrará dois documentos igualmente válidos.

**Conflito de regime de descontos:** os dois regimes são incompatíveis. Um cliente que negociou aditivo contratual de 8% sob a v1 (negociação livre) está em qual regime? A v2 não responde.

**Conflito por estado temporal:** a regra de transição cria um terceiro estado — chamados antigos em processamento operam sob a v1 enquanto novos chamados operam sob a v2, tornando possível que dois fretes idênticos para o mesmo cliente sejam calculados de forma diferente no mesmo mês.

---

### 3. Impactos para o assistente de IA

O problema fundamental é que ambos os documentos são igualmente recuperáveis por um sistema de RAG sem qualquer sinal de qual é autoritativo. O assistente não possui, por padrão, acesso à data de abertura do chamado — dado necessário para aplicar a regra de transição.

**Quatro cenários de falha:**

| Cenário | Causa | Consequência |
|---|---|---|
| Assistente usa a v1 | Recupera documento mais antigo | Multiplicadores regionais menores e fatores de peso maiores — frete subestimado para a empresa |
| Assistente usa a v1 para chamado pós-01/12/2023 | Não tem acesso à data do chamado | Informa prazo +2 dias em vez de +3 dias |
| Assistente informa desconto fixo (v2) a cliente com aditivo v1 | Não conhece histórico contratual | Expectativa que o Comercial não pode honrar |
| Assistente combina componentes das duas versões | Fragmentação na indexação | Multiplicador da v2 + fator de peso da v1 — valor incorreto e sem detecção fácil |

Nenhum desses erros é detectável pelo usuário em tempo real. O cliente recebe um número aparentemente preciso gerado por uma fórmula aplicada com confiança.

---

### 4. Riscos operacionais

**Risco financeiro:** dependendo da combinação região × peso, o frete calculado pela versão errada pode ser até 12,5% diferente do valor correto. Em contratos de alto volume, essa diferença acumula mensalmente.

**Risco contratual:** clientes com SLA de entrega baseado no prazo da v1 (+2 dias) podem contestar a NovaTech se o assistente informar +3 dias e a operação entregar conforme a v1 — ou vice-versa.

**Risco de relacionamento:** um cliente que recebeu proposta de desconto com base nos percentuais da v2 (regra fixa de 5%) e cujo aditivo histórico previa negociação livre terá experiência contraditória ao acionar o Comercial.

**Risco sistêmico:** implantar o assistente com os dois documentos na base e sem regra de desambiguação é implantar uma fonte permanente de inconsistência detectável pelos clientes antes de ser detectada pela equipe interna.

---

### 5. Tabela comparativa de inconsistências

| Dimensão | PROC-042 v1 | PROC-042 v2 | Status | Impacto |
|---|---|---|---|---|
| Fator de peso 500–1.000 kg | 1.0 | 1.0 | Igual | Nenhum |
| Fator de peso 1.001–3.000 kg | 1.2 | 1.15 | **Conflito** | –4,2% no frete para essa faixa |
| Fator de peso acima de 3.000 kg | 1.5 | 1.4 | **Conflito** | –6,7% no frete para essa faixa |
| Multiplicador Sudeste | 1.0 | 1.1 | **Conflito** | +10% no frete |
| Multiplicador Sul | 1.2 | 1.3 | **Conflito** | +8,3% no frete |
| Multiplicador Centro-Oeste | 1.3 | 1.4 | **Conflito** | +7,7% no frete |
| Multiplicador Nordeste | 1.4 | 1.5 | **Conflito** | +7,1% no frete |
| Multiplicador Norte | 1.6 | 1.8 | **Conflito** | +12,5% no frete |
| Prazo de entrega | +2 dias úteis | +3 dias úteis | **Conflito** | +1 dia — risco contratual |
| Elegibilidade a desconto | >10 fretes/mês | ≥8 fretes/mês | **Conflito** | Limiar menor na v2 |
| Critério de desconto | Negociação livre + aditivo | Percentual fixo (5% ou 10%) | **Conflito** | Regimes incompatíveis |
| Faixa 8–15 fretes/mês | Não definido | 5% sobre mult. regional | **Conflito** | Benefício inexistente na v1 |
| Faixa >15 fretes/mês | Não definido | 10% sobre mult. regional | **Conflito** | Benefício inexistente na v1 |
| Desconto acima do teto | Via Comercial | Via Diretoria Comercial | Atenção | Escalada maior na v2 |
| Indicação de revogação da v1 | Ausente | Ausente | **Conflito** | Ambas vigentes no repositório |
| Hierarquia entre versões | Não definida | Não definida | **Conflito** | Sem critério de prevalência |
| Regra de transição | Inexistente | Presente (data corte 01/12/2023) | Atenção | Requer acesso à data do chamado |
| Status da regra de transição hoje | N/A | Desconhecido — pode ser obsoleta | **Conflito** | Sem confirmação operacional |
| Emissão | 03/03/2023 | 10/11/2023 | Informativo | V2 mais recente |
| Responsável | Diretoria Comercial | Diretoria Comercial | Igual | Nenhum |

---

### 6. Perguntas para os stakeholders no discovery

#### Diretoria Comercial (prioridade máxima)

- A PROC-042 v1 está formalmente revogada ou apenas descontinuada na prática?
- Existe algum documento de decisão (e-mail, ata, memorando) que declare a v2 como substituta exclusiva?
- Em caso de disputa com um cliente, qual das duas versões prevalece?
- A redução dos fatores de peso (de 1.2 → 1.15 e 1.5 → 1.4) representa queda de receita por frete. Qual foi a justificativa comercial?
- A mudança nos parâmetros foi comunicada formalmente aos clientes?
- O efeito líquido (multiplicadores regionais maiores + fatores de peso menores) foi calculado por região e faixa de peso antes da publicação da v2?

#### Operações / Contratos

- Existem contratos vigentes com clientes que referenciam explicitamente a PROC-042 v1 ou seus multiplicadores?
- Esses contratos precisarão ser aditados para refletir a v2, ou a v2 não se aplica retroativamente?
- Qual é o volume atual de chamados ainda em processamento abertos antes de 01/12/2023?
- Clientes com aditivo contratual de desconto firmado sob a v1 passam automaticamente para as regras da v2?

#### TI / SharePoint

- O SharePoint possui controle de versão ativo para documentos de procedimento?
- Os dois arquivos aparecem nos mesmos resultados de busca?
- É possível arquivar a v1 e manter apenas a v2 indexável sem perder o histórico?

#### Gestão do Conhecimento / Compliance

- Existe um processo formal de gestão de ciclo de vida de procedimentos (criação → aprovação → publicação → revogação)?
- Se sim, por que a v2 foi publicada sem revogação formal da v1?
- Essa situação existe em outros PROCs além do 042?

#### Atendimento

- Os atendentes sabem que devem usar a v2 para chamados novos?
- Existe algum comunicado interno que orientou a transição?
- O FAQ foi atualizado quando a v2 foi publicada?

---

### Recomendação para o discovery

Antes de qualquer decisão sobre incluir esses documentos na base do assistente, é preciso resolver o problema de governança — não o problema técnico de qual versão incluir. A questão central a responder com os stakeholders é: **"A PROC-042 v1 está revogada?"**

- Se **sim**: arquivar a v1 no SharePoint, indexar apenas a v2, registrar a regra de transição como nota histórica (se ainda relevante).
- Se **não**: o assistente precisará de uma camada de roteamento por contexto contratual antes de calcular qualquer frete — o que aumenta significativamente a complexidade da implantação.

---

## Arquivos gerados nesta sessão

| Arquivo | Conteúdo |
|---|---|
| `analise-proc042-vs-v2.md` | Este documento — transcrição e análise comparativa completa |

---

*Sessão gerada em: 2026-06-06*
*Contexto: segunda parte da sessão de discovery — NovaTech Assistente de IA Corporativo*
