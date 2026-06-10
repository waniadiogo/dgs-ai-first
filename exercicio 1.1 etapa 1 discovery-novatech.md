# Discovery — Assistente de IA Corporativo NovaTech
*Transcrição da sessão de discovery*

---

## Documentos fornecidos

| Arquivo | Descrição |
|---|---|
| `FAQ.docx` | FAQ de Atendimento — documento informal, criado pelos atendentes, sem validação formal |
| `POL-001___Política_de_Devolução_de_Mercadorias.docx` | POL-001 v3.1 — Política de Devolução de Mercadorias |
| `PROC-042___Procedimento_de_Cálculo_de_Frete_Especial.docx` | PROC-042 v1.0 — Procedimento de Cálculo de Frete Especial (mar/2023) |
| `PROC-042-v2___Procedimento_de_Cálculo_de_Frete_Especial.docx` | PROC-042 v2 — Procedimento revisado de Cálculo de Frete Especial (nov/2023) |
| `SLA.docx` | SLA-2024 v2024.1 — Tabela de SLA por Tipo de Cliente |

---

## Prompt 1

**Entrada do usuário:**

> Você é um consultor de discovery para implantação de um assistente de IA corporativo.
>
> Contexto: A NovaTech possui documentação distribuída em SharePoint, Confluence e planilhas.
>
> Documentos disponíveis:
> 1. POL-001 – Política de Devolução de Mercadorias — Regras para devolução em até 7 dias. Exceções para cargas perigosas.
> 2. PROC-042 – Procedimento de Cálculo de Frete Especial — Fórmula para fretes acima de 500kg. Multiplicadores por região.
> 3. SLA-2024 — Prazos de atendimento para clientes Gold, Silver e Standard.
> 4. PROC-042-v2 — Versão revisada do procedimento de cálculo de frete. Possui multiplicadores diferentes da versão anterior.
> 5. FAQ Atendimento — Documento informal criado pelos atendentes. Não possui validação formal.
>
> Tarefa:
> 1. Agrupe os documentos por tema.
> 2. Identifique possíveis lacunas de informação.
> 3. Identifique documentos que apresentam maior risco para um assistente de IA.
> 4. Gere hipóteses para investigação no discovery.

**Arquivos anexados:** FAQ.docx, POL-001, PROC-042 v1, PROC-042 v2, SLA.docx

---

## Saída 1

### Agrupamento por tema

Três clusters temáticos identificados após leitura integral dos documentos:

**Cluster A — Logística de fretes**
- PROC-042 v1 (mar/2023) e PROC-042 v2 (nov/2023)
- Referência ao PROC-043 (cargas perigosas), citado como "em revisão pelo Compliance" — ausente na base

**Cluster B — Políticas de atendimento**
- POL-001 v3.1 (devoluções pós-entrega)
- SLA-2024 v2024.1
- Referência ao PROC-088 (cargas em trânsito) — ausente na base

**Cluster C — Suporte operacional**
- FAQ Atendimento — documento transversal, cobre fretes, devoluções, SLA e rastreamento num único arquivo informal

---

### Lacunas de informação

**PROC-043 — Cargas perigosas acima de 500kg**
Referenciado na PROC-042 v1, na v2 e pelo FAQ como "em revisão pelo Compliance". Ausente na base. Toda pergunta sobre frete especial de carga perigosa fica sem documento formal de resposta.

**PROC-088 — Cargas em trânsito**
Citado na abertura da POL-001, que define seu escopo apenas para devoluções "após a entrega". Perguntas sobre avarias, rastreamento ou atrasos de cargas ainda em rota não têm documento formal.

**Política de descontos por volume**
- PROC-042 v1: descontos para clientes com >10 fretes/mês devem ser "negociados diretamente com o Comercial e formalizados em aditivo contratual"
- PROC-042 v2: substitui por percentuais fixos (5% para 8–15 fretes/mês; 10% para >15 fretes/mês)
- Sem indicação formal de revogação da v1 → dois regimes conflitantes vigentes simultaneamente

**Seguro de carga adicional**
FAQ menciona taxas de 0,3% (mercadoria padrão) e 0,8% (carga perigosa) para contratos a partir de 2023. Não há procedimento formal correspondente.

**Rastreamento e prazos regionais**
FAQ define critérios por região (Norte: até 10 dias úteis; Sul/Sudeste: >3 dias = anormal). Sem documento formal de suporte.

---

### Documentos de maior risco para o assistente de IA

**Risco crítico — PROC-042 v1 + v2 em coexistência**
Ambas no SharePoint sem hierarquia ou revogação formal. O assistente pode usar a versão errada dependendo da ordem de recuperação. Os multiplicadores diferem em todas as regiões (ex.: Norte: 1.6 → 1.8) e os fatores de peso também mudam (ex.: 1.001–3.000 kg: 1.2 → 1.15). Há ainda uma regra de transição temporal (data de abertura do chamado antes ou depois de 01/12/2023).

**Risco crítico — FAQ Atendimento**
O documento se autodescreve como informal e sem validação. Apesar disso, contém informações numéricas precisas (0,3%, 0,8%, R$50.000, 48 horas, 10 dias úteis) que um LLM tende a tratar com a mesma confiança de um documento normativo. É o único documento que cobre temas sem procedimento formal — tornando-o de facto a única fonte, mesmo sendo não oficial.

**Risco moderado-alto — PROC-042 v2 com regra de transição**
A regra de transição exige que o assistente saiba a data de abertura do chamado para determinar qual tabela aplicar. Sem esse contexto, qualquer resposta sobre frete especial tem 50% de chance de estar errada.

**Risco baixo — POL-001 e SLA-2024**
Documentos formais, versionados, com estrutura clara. O risco residual é a referência à PROC-088 ausente (casos de carga em trânsito).

---

### Hipóteses para investigação no discovery

| ID | Hipótese |
|---|---|
| H1 | Versão vencedora do PROC-042 não está definida formalmente — investigar e-mail, ata ou decisão da Diretoria Comercial |
| H2 | O FAQ está sendo usado como substituto de procedimentos inexistentes — levantar quais perguntas frequentes não têm PROC correspondente |
| H3 | O PROC-043 está em revisão e pode estar bloqueando decisões — verificar status com Compliance |
| H4 | Contratos pré-2023 operam com regras de seguro e desconto diferentes — mapear volume de clientes afetados |
| H5 | O critério de "incidente crítico" pode estar sendo aplicado de forma inconsistente — SLA-2024 vs. critérios adicionais do FAQ |
| H6 | A regra de transição do PROC-042 pode já ser irrelevante operacionalmente — verificar se há chamados pré-01/12/2023 ainda em aberto |

**Próximos passos recomendados:** entrevistas com o gestor de Compliance (status PROC-043), Diretoria Comercial (versionamento PROC-042) e líderes do atendimento (lacunas cobertas pelo FAQ).

---

## Prompt 2

**Entrada do usuário:**

> Faça um mapa de temas cobertos e hipóteses de gaps.

---

## Saída 2

Gerado mapa interativo com seis temas e 13 hipóteses de gap, com indicador de cobertura por tema e lista de conteúdos cobertos/parciais/ausentes por documento.

### Resumo do mapa por tema

| Tema | Cobertura | Status |
|---|---|---|
| SLAs e tiers | 85% | Sólido — gap residual em critério de prioridade Gold |
| Devoluções | 70% | Bem coberto para casos padrão — PROC-088 ausente |
| Cálculo de frete | 55% | Conflito entre versões v1 e v2 |
| Descontos por volume | 40% | Critérios conflitantes entre v1 e v2 |
| Rastreamento | 25% | Cobertura quase exclusivamente informal (FAQ) |
| Seguro de carga | 15% | Sem documentação formal |

### Hipóteses de gap mapeadas

| ID | Tema | Hipótese |
|---|---|---|
| H1 | Frete | Sem revogação formal da PROC-042 v1 — risco de recuperação da versão errada |
| H2 | Frete | Regra de transição (01/12/2023) pode ser operacionalmente encerrada |
| H3 | Frete | PROC-043 ausente — cargas perigosas +500kg sem cobertura formal |
| H4 | Devoluções | PROC-088 ausente — cargas em trânsito sem procedimento formal |
| H5 | Devoluções | FAQ é única fonte para cargas danificadas em trânsito — risco de falsa autoridade |
| H6 | SLA | Critério de prioridade Gold (R$50k) está no FAQ, não no SLA-2024 |
| H7 | SLA | Tier "Platinum" inexistente — necessidade de resposta categórica com base formal |
| H8 | Rastreamento | Sem procedimento formal de tracking — exclusão do FAQ cria cobertura zero |
| H9 | Rastreamento | FAQ mistura regras formais e práticas informais sem distinção |
| H10 | Seguro | Alíquotas (0,3% e 0,8%) existem apenas no FAQ — risco de falsa autoridade |
| H11 | Seguro | Contratos pré-2023 sem critério documentado — assistente não consegue responder |
| H12 | Descontos | Regra aplicável depende do contrato — assistente sem acesso a dado contratual |
| H13 | Descontos | Critério v1 pode não estar formalmente revogado |

---

*Sessão gerada em: 2026-06-06*
*Arquivos de saída desta sessão: `discovery-novatech.md`*
