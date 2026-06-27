# Registro de Iteração — Harness de Produto NovaTech

**Projeto:** Assistente de IA NovaTech — Gestão Documental  
**Fase:** Estruturação → Governança (Harness de Produto)  
**Data:** Janeiro de 2024  
**Participantes:** Product Specialist (DB1) + Claude (assistente de IA)

---

## 1. Contexto fornecido

### 1.1 Cenário da NovaTech

A NovaTech é uma empresa de médio porte do setor de logística com 1.200 funcionários. Sua operação depende de documentação interna extensa: manuais de procedimento operacional, políticas de compliance, tabelas de SLA por tipo de cliente, regras de cálculo de frete e normas de segurança de carga.

A documentação está distribuída em três fontes:
- SharePoint corporativo (~800 documentos em PDF e Word)
- Wiki interna no Confluence (~400 páginas)
- Pasta de rede com planilhas de referência atualizadas mensalmente

**Problema central:** a equipe de atendimento ao cliente (45 pessoas) gasta em média 12 minutos por chamado buscando informações nessas fontes. Isso gera atrasos, respostas inconsistentes e frustração dos atendentes e clientes.

**Solução contratada:** a DB1 foi contratada para construir um assistente de IA que permite aos atendentes fazer perguntas em linguagem natural e receber respostas fundamentadas na documentação oficial, com indicação da fonte. O assistente será integrado ao ambiente Microsoft da NovaTech (Teams + SharePoint).

### 1.2 Status do projeto no momento da iteração

- Discovery concluído
- ADRs com decisões arquiteturais aprovadas (modelo LLM, estratégia de contexto, tratamento de documentos contraditórios, build vs buy)
- Pipeline de RAG funcional com primeiros endpoints implementados
- Bot do Teams respondendo perguntas de teste
- AGENTS.md, specs SDD, skills e guardrails definidos na fase de estruturação
- Necessidade: estruturar o harness de produto antes do go-live

### 1.3 Conceito de Harness de Produto (conforme briefing)

> Define quais métricas de qualidade são monitoradas, como o feedback de usuários é processado, e como mudanças no assistente são validadas antes de ir a produção (regression testing de produto).

---

## 2. Insumo principal — Guardrails (documento 2_3_product-rules-guardrails.md)

O documento foi fornecido como anexo e lido integralmente antes da elaboração do harness. Abaixo, síntese estruturada dos guardrails que fundamentam o harness produzido.

### 2.1 Obrigações (DEVE)

| ID | Regra |
|---|---|
| RULE-001 | Citar fonte em toda resposta (identificador + seção). Formato mínimo: `[POL-001 § 3.1]` |
| RULE-002 | Incluir campo `source_document` no JSON de retorno em toda resposta, mesmo com confiança baixa |
| RULE-003 | Responder em português formal (norma culta brasileira). Sem contrações, gírias ou abreviações |
| RULE-004 | Quando duas versões coexistirem: usar a mais recente, informar a anterior, aplicar disposições transitórias |
| RULE-005 | Escalar ao supervisor quando não houver base documental para confiança alta ou média |

### 2.2 Proibições (NÃO DEVE)

| ID | Regra |
|---|---|
| RULE-006 | Não gerar valores numéricos (prazos, multiplicadores, percentuais, SLAs) que não estejam literalmente no corpus |
| RULE-007 | Não afirmar que carga perigosa (classes 1–6 ANTT) pode ser devolvida pelo fluxo padrão. Sempre encaminhar ao ramal 4500 |
| RULE-008 | Não inventar ou confirmar tiers além de Gold, Silver e Standard. Se mencionado "Platinum" ou outro, informar que não existe |
| RULE-009 | Não usar FAQ-Atendimento como fonte normativa primária (documento informal, não validado por Compliance) |
| RULE-010 | Não fornecer percentuais de seguro de carga sem alertar que variam por contrato e que Comercial deve ser consultado para contratos anteriores a 2023 |

### 2.3 Tratamento de Incerteza (QUANDO EM DÚVIDA)

| ID | Regra |
|---|---|
| RULE-011 | Prefixar resposta com aviso de confiança baixa; definir `"confidence": "low"`; sugerir escalação |
| RULE-012 | Quando versões coexistirem sem hierarquia formal: usar a mais recente, informar ambiguidade, não mascarar contradição |
| RULE-013 | Verificar ativamente restrições de RULE-007 antes de responder qualquer questão sobre carga perigosa |

### 2.4 Glossário canônico (termos do domínio)

| Termo | Definição |
|---|---|
| CT-e | Conhecimento de Transporte Eletrônico — chave de identificação de chamados de devolução |
| Tier | Gold, Silver ou Standard. Nenhum outro valor válido |
| Frete especial | Aplicável a cargas acima de 500 kg. Regido pela PROC-042 |
| Multiplicador regional | Fator por região de destino no cálculo do frete especial (PROC-042-v2 § 2.1) |
| Fator de peso | Fator por faixa de peso no cálculo do frete especial |
| Incidente crítico | Chamado com ao menos um critério de criticidade (SLA-2024 § 3) |
| SLA de resposta | Prazo máximo para primeiro retorno ao cliente |
| SLA de resolução | Prazo máximo para encerramento do chamado |
| Carga perigosa | Classes 1–6 ANTT (Resolução nº 5.947/2021) |
| Coleta reversa | Logística reversa agendada pela NovaTech após aprovação de devolução |
| Disposições transitórias | Regras de migração entre versões para chamados em processamento |
| source_document | Campo obrigatório no JSON de resposta do assistente |
| Gestão de Riscos | Setor responsável por cargas perigosas e exceções. Ramal 4500 |

### 2.5 Restrições de código relevantes para o harness

```typescript
// Contrato obrigatório de resposta
interface AssistantResponse {
  answer: string;
  source_document: { id: string; section: string; version: string; };
  confidence: 'high' | 'medium' | 'low';
  low_confidence_reason?: string; // Obrigatório quando confidence === 'low'
}

// Tiers válidos — nunca adicionar sem aprovação do Product Specialist
type CustomerTier = 'Gold' | 'Silver' | 'Standard';

// Classes de carga perigosa — bloqueiam o fluxo de devolução padrão
const DANGEROUS_CARGO_CLASSES = [1, 2, 3, 4, 5, 6] as const;
```

---

## 3. Prompt recebido

```
Você é um especialista em produtos de IA responsável por definir um harness de produto
para um assistente corporativo baseado em RAG.

Utilizando o que foi fornecido anteriormente:
* o contexto da NovaTech;
* o conceito de Harness de Produto;
* os guardrails definidos anteriormente (DEVE, NÃO DEVE e QUANDO EM DÚVIDA).

Com base nessas informações, elabore um documento de Harness de Produto que contemple:

1. Processo de feedback
   * Como o feedback dos atendentes deve ser tratado.
   * Quando uma melhoria deve gerar ajuste de prompt, inclusão de novo documento ou
     reindexação da base.

2. Regression testing de produto
   * Como validar que alterações no prompt ou na base documental não pioraram respostas
     existentes.
   * Como verificar que os guardrails continuam sendo respeitados antes da publicação.

3. Human-in-the-Loop (HITL)
   * Em quais situações mudanças no assistente precisam de aprovação humana antes de ir
     para produção.
   * Quem deve realizar essa aprovação.

Organize a resposta como um documento de produto, utilizando linguagem clara e objetiva,
adequada para um Analista de Negócios. Evite detalhar implementações técnicas e foque
nas regras de negócio, processos e responsabilidades.
```

---

## 4. Decisões tomadas na elaboração

### 4.1 Formato de entrega
- Documento Word (.docx) com formatação profissional, cabeçalho, rodapé e numeração de páginas.
- Linguagem: português formal, sem jargões técnicos de desenvolvimento.
- Público-alvo: Analista de Negócios, Product Specialist, Compliance NovaTech.

### 4.2 Estrutura do documento produzido

| Seção | Conteúdo |
|---|---|
| 1. Introdução e Objetivo | Contextualiza o harness e sua relação com os guardrails e ADRs |
| 2. Processo de Feedback | Fontes, classificação (6 categorias), fluxo de tratamento, critérios de priorização |
| 3. Regression Testing | Golden queries, cobertura mínima por RULE, critérios de aprovação, cadência |
| 4. Human-in-the-Loop | Classificação de risco (🟢🟡🔴), fluxo de aprovação, bloqueio de emergência, mudanças que exigem Compliance |
| 5. Papéis e Responsabilidades | Matriz com 5 papéis e suas responsabilidades específicas no harness |
| 6. Cadência Operacional | Tabela de frequências (contínua, diária, semanal, por deploy, quinzenal, trimestral) |
| 7. Glossário | 10 termos canônicos com definição e fonte |
| 8. Referências | 8 documentos do repositório com relevância para o harness |

### 4.3 Principais decisões de produto

**Feedback:**
- Violações de guardrail têm prazo de triagem de 4h (vs. 24–48h para demais categorias), refletindo a criticidade das RULEs.
- A causa raiz determina a ação: não existe ação única para todo feedback negativo. Lacuna documental → inclusão; prompt ambíguo → ajuste de prompt; documento desatualizado → reindexação.
- Erros recorrentes (3+ chamados/semana) disparam correção obrigatória no próximo deploy, sem aguardar ciclo quinzenal.

**Regression Testing:**
- O conjunto de golden queries cobre todas as 13 RULEs, com volume mínimo definido por área de risco.
- Critério de aprovação é binário e não negociável: zero violações de guardrail em qualquer query do conjunto.
- Ciclos periódicos (quinzenais) ocorrem mesmo sem mudanças pendentes, garantindo monitoramento proativo.

**HITL:**
- Três níveis de risco (baixo/médio/alto) com aprovadores distintos evitam tanto gargalos desnecessários quanto mudanças críticas sem revisão adequada.
- Cinco cenários de bloqueio de emergência foram definidos explicitamente, mapeando diretamente as RULEs de maior impacto (007, 008, 006, 009, 002).
- Cinco tipos de mudança foram identificados como exclusivos do Compliance NovaTech, sem exceção mesmo em urgência.

---

## 5. Artefato entregue

| Atributo | Valor |
|---|---|
| Nome do arquivo | `NovaTech_Harness_de_Produto.docx` |
| Versão | 1.0 |
| Status | Em vigor |
| Revisão prevista | Trimestral ou após mudança crítica |
| Documentos de referência | AGENTS.md \| ADRs \| PROC-042-v2 \| POL-001 \| SLA-2024 |

---

## 6. Prompt de solicitação deste registro

```
registre toda a iteração em um arquivo md
```

---

*Registro gerado ao final da sessão. Todos os artefatos desta iteração estão disponíveis
para consulta no repositório do projeto NovaTech.*
