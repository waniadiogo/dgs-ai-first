# Análise integrada: FAQ × mapa documental × inconsistências PROC-042
*Transcrição da sessão de análise cruzada — Discovery Assistente de IA NovaTech*

---

## Documentos utilizados

### Fornecidos nesta sessão

| Arquivo | Descrição |
|---|---|
| `FAQ.docx` | FAQ de Atendimento — documento colaborativo e informal, criado pelo time de suporte, sem validação formal de Compliance ou Operações |

### Referenciados das etapas anteriores

| Origem | Conteúdo utilizado |
|---|---|
| Etapa 1 — Mapa documental | Agrupamento temático dos documentos, lacunas identificadas (PROC-043, PROC-088), clusters por tema, cobertura por área |
| Etapa 2 — Análise PROC-042 | Tabela de 19 inconsistências entre v1 e v2: fatores de peso, multiplicadores regionais, prazos, regras de desconto, regra de transição, ausência de revogação formal |

---

## Prompt

**Entrada do usuário:**

> Utilizando:
> * mapa documental da etapa 1;
> * inconsistências encontradas na etapa 2;
> * FAQ completo de atendimento;
>
> Analise:
> 1. Se o FAQ utiliza informações da versão antiga ou da versão nova do PROC-042.
> 2. Se existem práticas operacionais não documentadas.
> 3. Se os atendentes estão compensando falhas da documentação oficial.
> 4. Quais respostas do FAQ representam risco para o futuro assistente de IA.
> 5. Quais informações devem ser formalizadas antes do go-live.

---

## Conteúdo extraído do FAQ

Documento autodescrito como "colaborativo e informal", com aviso explícito de que não possui validação formal de Compliance ou Operações e que dados podem estar desatualizados. Recomenda validação nas fontes oficiais (POL, PROC e SLA).

### Devoluções e cargas danificadas

- **Cargas perigosas:** processo padrão não permite devolução. Atendente direciona ao setor de Gestão de Riscos (ramal 4500), sem negar a possibilidade de imediato.
- **Cargas danificadas em trânsito:** não seguem o fluxo comum de devolução. Cliente deve registrar em até 48 horas após o recebimento (fotos e laudo). Reembolso integral se culpa da NovaTech comprovada, via setor Jurídico — `sinistros@novatech.com.br`.

### Fretes e descontos

- **Cálculo de frete especial (+500 kg):** utiliza multiplicadores regionais. FAQ alerta para coexistência das duas versões e orienta usar a v2, salvo contrato do cliente que siga a tabela antiga.
- **Descontos:** atendentes sem autonomia para conceder descontos. Casos automáticos por volume (mais de 10 fretes especiais/mês) seguem a PROC-042. Demais casos: encaminhar ao Comercial.
- **Frete expresso para cargas perigosas:** permitido, exige autorização do Compliance e documentação ANTT em dia. Prazo prático: cerca de 2 dias.

### Tiers e SLAs

- **Tier "Platinum":** não existe. Tiers vigentes: Gold, Silver e Standard. Possível confusão com programa descontinuado em 2022.
- **Prazos de SLA:** resposta vs. resolução — 2h/24h (Gold), 4h/48h (Silver), 8h/72h (Standard), com prazos reduzidos para incidentes críticos conforme SLA-2024.

### Rastreamento e seguros

- **Atrasos no tracking:** Norte — até 10 dias úteis sem atualização; Sul/Sudeste — mais de 3 dias é anormal. Chamado com prioridade alta para cliente Gold ou carga acima de R$ 50.000.
- **Seguro de carga adicional:** contratos a partir de 2023 — 0,3% (mercadoria padrão) e 0,8% (carga perigosa). Contratos anteriores: validar com o Comercial.

---

## Saída

### Classificação de cada afirmação do FAQ

| Trecho do FAQ | Fonte identificada | Risco para IA | Prática não documentada | Formalizar antes do go-live |
|---|---|---|---|---|
| Orientação de usar PROC-042 v2 (salvo contrato antigo) | Misto v1+v2 — regra criada pelo time | Alto | Sim | Sim |
| Limiar de desconto: mais de 10 fretes/mês | PROC-042 v1 (desatualizado) | Crítico | Não | Sim |
| Frete expresso para cargas perigosas: Compliance + ANTT + 2 dias | Sem documento formal | Crítico | Sim | Sim |
| Devolução de carga perigosa: não negar imediatamente | POL-001 (consistente) | Médio | Sim (nuance) | Não |
| Avaria em trânsito: 48h, fotos, laudo, Jurídico, e-mail | Sem documento formal (PROC-088 ausente) | Crítico | Sim | Sim |
| Inexistência do tier Platinum | SLA-2024 (consistente) | Médio | Não | Não |
| Prazos de SLA (2h/24h, 4h/48h, 8h/72h) | SLA-2024 (consistente) | Médio | Não | Não |
| Prazos regionais de tracking (Norte 10 dias, Sul/SE 3 dias) | Sem documento formal | Crítico | Sim | Sim |
| Critério R$ 50.000 para prioridade de rastreamento | Sem documento formal — contradiz SLA-2024 (R$ 100.000) | Crítico | Sim | Sim |
| Alíquotas de seguro: 0,3% e 0,8% | Sem documento formal | Crítico | Sim | Sim |

---

### 1. O FAQ usa a versão antiga ou nova do PROC-042?

O FAQ apresenta situação mista e reveladora: reconhece que a v2 existe e orienta usá-la por padrão, mas operacionaliza regras da v1 em pelo menos um ponto crítico.

**Correto e atualizado para a v2:** a orientação de usar a PROC-042 v2 "salvo contrato específico com a tabela antiga" é operacionalmente correta — mas é uma regra criada pelo próprio time, sem respaldo em documento formal emitido pela Diretoria Comercial.

**Desatualizado — usa critério da v1:** o limiar de "mais de 10 fretes especiais/mês" para desconto automático é o critério da PROC-042 v1. A v2 reduziu para 8 fretes/mês e substituiu a negociação livre por percentuais fixos (5% e 10%). Clientes com 8 a 10 fretes mensais têm direito a 5% de desconto automático pela v2, mas o FAQ os invisibiliza.

**Conclusão:** o FAQ usa a v2 para a decisão de qual versão aplicar, mas usa a v1 para o conteúdo das regras de desconto. É um documento em transição incompleta.

---

### 2. Práticas operacionais não documentadas

Cinco práticas identificadas que existem na operação mas não aparecem em nenhum documento formal:

**Frete expresso para cargas perigosas:** processo com três etapas específicas — autorização do Compliance, documentação ANTT em dia, prazo prático de 2 dias — sem correspondência em nenhum PROC ou POL. O PROC-043, que deveria cobrir isso, está ausente da base.

**Fluxo de avaria em trânsito:** prazo de 48 horas para registro, exigência de fotos e laudo, encaminhamento ao Jurídico pelo e-mail `sinistros@novatech.com.br`. O PROC-088, referenciado pela POL-001, não existe. O FAQ é a única fonte desse fluxo.

**Prazos de tolerância de rastreamento por região:** 10 dias úteis (Norte), 3 dias (Sul/Sudeste). Regras operacionais desenvolvidas empiricamente pelo time, sem registro formal.

**Critério de prioridade por valor de carga:** R$ 50.000 para abertura de chamado prioritário de rastreamento. O SLA-2024 define R$ 100.000 como limiar de incidente crítico — limiares diferentes, propósitos possivelmente distintos, nunca explicados formalmente.

**Nuance de atendimento em devolução perigosa:** instrução de "não negar imediatamente" é de baixo risco, mas representa uma escola de atendimento oral não registrada em documento.

---

### 3. Os atendentes estão compensando falhas da documentação oficial?

Sim — de forma sistemática, em três padrões distintos:

**Compensação de vácuo documental:** trechos sobre avaria em trânsito, frete expresso para cargas perigosas, prazos regionais de tracking e alíquotas de seguro existem porque os documentos formais correspondentes (PROC-088, PROC-043, política de seguros) não existem ou não estão disponíveis. O time criou as respostas por necessidade operacional.

**Compensação de conflito não resolvido:** a orientação de "usar a v2 salvo contrato específico" existe porque a Diretoria Comercial não emitiu revogação formal da v1. Os atendentes resolveram o problema de governança por conta própria e registraram a solução no FAQ.

**Compensação de desatualização:** o limiar de desconto de 10 fretes/mês (v1) ainda está no FAQ porque a v2 não foi comunicada formalmente ao time. Os atendentes não sabem que a regra mudou — ou sabem, mas o FAQ não foi atualizado.

O FAQ cresce cada vez que a documentação oficial falha. Ele é simultaneamente o sintoma do problema e a solução improvisada para ele.

---

### 4. Quais respostas do FAQ representam risco para o futuro assistente de IA?

**Risco crítico:**

| Afirmação do FAQ | Problema | Consequência para o assistente |
|---|---|---|
| Alíquotas de seguro: 0,3% e 0,8% | Sem validação de Compliance. Números precisos sem respaldo formal | Citará valores de seguro com falsa confiança — podem estar errados ou desatualizados |
| Avaria em trânsito: 48h, fotos, laudo, e-mail Jurídico | Sem documento formal. E-mail pode mudar | Instruirá clientes a seguir processo não garantido. E-mail inativo = cliente sem resposta |
| Critério R$ 50.000 para rastreamento prioritário | Contradiz diretamente o limiar de R$ 100.000 do SLA-2024 | Aplicará critério errado — ou usará o valor do documento recuperado primeiro |
| Frete expresso para cargas perigosas: Compliance + ANTT + 2 dias | Sem documento formal. PROC-043 ausente | Descreverá processo sem sustentação. Se o processo mudar, o assistente não saberá |
| Desconto: limiar de 10 fretes/mês | Critério da v1 — v2 usa 8 fretes/mês | Informará a clientes com 8–10 fretes que não têm direito a desconto automático, quando têm |

---

### 5. O que deve ser formalizado antes do go-live

**Urgência máxima — bloqueadores de go-live:**

1. Revogação formal da PROC-042 v1 e comunicação oficial da v2 como versão vigente, com instrução sobre contratos em transição. Sem isso, qualquer priorização da v2 pelo assistente é decisão técnica sem autorização da Diretoria Comercial.

2. Política de seguros de carga adicional aprovada por Compliance ou Operações, validando (ou corrigindo) as alíquotas de 0,3% e 0,8% e definindo regras para contratos pré-2023.

3. Reconciliação do critério de prioridade de rastreamento: R$ 50.000 (FAQ) vs. R$ 100.000 (SLA-2024) — definir se cobrem casos distintos ou se um deles deve ser eliminado.

**Urgência alta — necessários antes do go-live:**

4. Fluxo formal de registro de avaria em trânsito: prazo, documentação exigida, canal de contato — como atualização da POL-001 ou criação do PROC-088.

5. Comunicação formal ao time de atendimento sobre as novas regras de desconto da PROC-042 v2 (8 fretes/mês, percentuais fixos de 5% e 10%) e atualização do FAQ.

**Urgência média — recomendado mas não bloqueador:**

6. Processo de frete expresso para cargas perigosas a ser incluído no PROC-043, quando concluído pelo Compliance.

7. Prazos regionais de tracking (Norte: 10 dias, Sul/SE: 3 dias) a serem validados por Operações e incorporados em procedimento formal.

8. Instrução de atendimento "não negar imediatamente" em devoluções de carga perigosa formalizada como nota na POL-001.

---

### Síntese executiva

O FAQ revela que o time de atendimento opera com um conjunto de regras híbrido — parte da v1, parte da v2, parte criado internamente — e que cinco categorias de informação operacionalmente crítica existem apenas nesse documento informal. O assistente de IA não pode ser implantado usando o FAQ como fonte sem antes separar o que é política validada do que é prática compensatória. O risco não é o FAQ em si — é tratá-lo como equivalente aos documentos formais.

---

## Arquivo gerado nesta sessão

| Arquivo | Conteúdo |
|---|---|
| `analise-faq-vs-proc042-mapa.md` | Este documento — transcrição e análise cruzada completa |

---

*Sessão gerada em: 2026-06-06*
*Contexto: terceira parte da sessão de discovery — NovaTech Assistente de IA Corporativo*
*Sessões anteriores: `discovery-novatech.md` (etapa 1), `analise-proc042-vs-v2.md` (etapa 2)*
