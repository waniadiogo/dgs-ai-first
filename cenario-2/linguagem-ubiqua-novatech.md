# Linguagem Ubíqua — Domínio NovaTech
## Assistente de IA para Atendimento ao Cliente

**Versão:** 1.0  
**Data:** Junho de 2026  
**Etapa:** 1 — Discovery e modelagem de domínio  
**Método:** Domain-Driven Design (DDD)

> Este glossário define os termos canônicos do domínio de negócio da NovaTech para uso consistente entre equipes de produto, engenharia e operações. Para cada termo, documenta-se a definição oficial, o contexto de uso, as ambiguidades identificadas, os riscos específicos para um LLM e a recomendação de uso.

---

## Legenda de risco para o assistente de IA

| Nível | Significado |
|---|---|
| Risco crítico | O LLM tende a produzir resposta incorreta ou de falsa autoridade sem tratamento explícito |
| Risco alto | O LLM pode errar em casos de borda sem contexto adicional |
| Risco baixo | Documentação formal clara; risco residual identificado mas gerenciável |

---

## 1. Frete especial
**Contexto:** Frete especial · **Risco:** Crítico

### Definição oficial
Modalidade de transporte aplicável a cargas com peso superior a 500 kg. O custo é calculado pela fórmula: valor base × multiplicador regional × fator de peso. O prazo de entrega recebe acréscimo fixo de dias úteis para manuseio.

### Contexto de uso
Gatilho: peso declarado acima de 500 kg. Não se confunde com frete expresso ou prioritário — no domínio NovaTech, "frete especial" refere-se exclusivamente à faixa de peso ≥ 500 kg.

### Possíveis ambiguidades
O termo pode evocar qualquer modalidade diferenciada de transporte (expresso, prioritário, dedicado). Sem a âncora de peso, o termo é ambíguo.

### Possíveis interpretações incorretas por um LLM
O LLM pode misturar parâmetros de PROC-042 v1 e v2, produzindo um valor de frete incorreto. Não existe resposta correta sobre cálculo sem saber a versão aplicável ao contrato do cliente.

### Recomendação de uso consistente
Nunca calcular frete especial sem identificar a versão contratual vigente do PROC-042. Se indisponível, sinalizar a ambiguidade e encaminhar ao time Comercial para confirmação.

---

## 2. Carga perigosa
**Contexto:** Gestão de riscos · **Risco:** Crítico

### Definição oficial
Mercadoria classificada nas classes 1 a 6 da ANTT (Resolução nº 5.947/2021): explosivos (classe 1), gases (classe 2), líquidos inflamáveis (classe 3), sólidos inflamáveis (classe 4), oxidantes e peróxidos (classe 5), substâncias tóxicas e infectantes (classe 6). A classificação é determinada pela documentação ANTT, não pela percepção do atendente.

### Contexto de uso
Aciona exceções em três contextos: Devolução (inelegível para processo padrão — encaminha ramal 4500), Frete Especial (remete ao PROC-043, ausente da base) e SLA (qualquer irregularidade de documentação ou rastreamento = incidente crítico).

### Possíveis ambiguidades
Clientes podem usar "produto sensível", "material restrito" ou "carga especial" referindo-se a cargas perigosas. Sem a classificação ANTT formal, o enquadramento não pode ser confirmado pelo assistente.

### Possíveis interpretações incorretas por um LLM
O LLM pode tentar responder perguntas sobre frete ou devolução de carga perigosa com base no FAQ (que descreve práticas informais), ignorando que o procedimento formal (PROC-043) está ausente da base. Risco de falsa autoridade em área com consequências legais e de segurança.

### Recomendação de uso consistente
Qualquer menção a carga perigosa deve acionar encaminhamento imediato para Gestão de Riscos (ramal 4500). O assistente não responde sobre cálculo, devolução ou seguro de carga perigosa — encaminha.

---

## 3. Cliente Gold
**Contexto:** Atendimento ao cliente · **Risco:** Baixo

### Definição oficial
Tier de cliente com contrato anual acima de R$ 500.000 OU mais de 200 operações por mês. Revisão semestral. Benefícios exclusivos: gerente de conta dedicado, relatório mensal detalhado, disponibilidade de portal 99,5%, relógio de SLA crítico sem pausa fora do horário comercial.

### Contexto de uso
Define os prazos de SLA aplicáveis: primeira resposta em 2h úteis (geral) ou 30 minutos (crítico); resolução em 24h úteis (geral) ou 4h (crítico).

### Possíveis ambiguidades
Clientes podem se autodeclarar Gold sem confirmação contratual. O tier declarado deve ser verificado no sistema via número de contrato antes de aplicar SLAs diferenciados.

### Possíveis interpretações incorretas por um LLM
O LLM pode aceitar a autodeclaração do cliente como verdadeira e aplicar SLAs Gold sem verificação. Pode também confundir com o tier "Platinum" — inexistente na NovaTech, descontinuado em 2022.

### Recomendação de uso consistente
Sempre solicitar número de contrato para confirmação de tier antes de aplicar SLAs diferenciados. Tier Platinum não existe — orientar o cliente sobre os três tiers vigentes (Gold, Silver, Standard).

---

## 4. Cliente Silver
**Contexto:** Atendimento ao cliente · **Risco:** Baixo

### Definição oficial
Tier com contrato anual entre R$ 100.000 e R$ 500.000 OU entre 50 e 200 operações por mês. Revisão semestral. Sem gerente dedicado. Relatório mensal resumido.

### Contexto de uso
SLA geral: resposta em 4h úteis, resolução em 48h úteis. SLA crítico: resposta em 1h, resolução em 8h.

### Possíveis ambiguidades
Um cliente pode atingir o critério de volume (50–200 ops/mês) sem atingir o de valor, ou vice-versa. Qualquer um dos dois critérios é suficiente para o tier Silver — lógica OR, não AND.

### Possíveis interpretações incorretas por um LLM
O LLM pode exigir que o cliente atenda os dois critérios simultaneamente, quando basta um.

### Recomendação de uso consistente
Usar o critério OR explicitamente ao descrever elegibilidade: "contrato acima de R$ 100k OU mais de 50 operações/mês".

---

## 5. Cliente Standard
**Contexto:** Atendimento ao cliente · **Risco:** Baixo

### Definição oficial
Tier de entrada — todos os clientes que não atingem os critérios de Gold ou Silver. Revisão anual. Sem gerente dedicado. Relatório sob demanda. SLAs formais vigentes definidos no SLA-2024.

### Contexto de uso
SLA geral: resposta em 8h úteis, resolução em 72h úteis. SLA crítico: resposta em 2h, resolução em 24h.

### Possíveis ambiguidades
"Standard" pode ser interpretado como ausência de SLA ou atendimento sem compromisso. Na NovaTech, Standard tem SLAs formais contratualmente definidos.

### Possíveis interpretações incorretas por um LLM
O LLM pode tratar Standard como "sem SLA" ou como tier inferior sem prazos definidos. Todos os tiers têm SLAs formais.

### Recomendação de uso consistente
Reforçar que Standard é o tier de entrada com compromissos contratuais vigentes — não é atendimento best-effort.

---

## 6. SLA
**Contexto:** Atendimento ao cliente · **Risco:** Alto

### Definição oficial
Service Level Agreement — compromisso contratual formal que define prazos máximos de primeira resposta e de resolução para cada tier de cliente, segmentado em chamados gerais e incidentes críticos. Medido pelo timestamp de abertura do chamado no Azure DevOps.

### Contexto de uso
SLA de resposta = primeiro retorno ao cliente (mesmo que seja "estamos verificando"). SLA de resolução = problema efetivamente solucionado. São dois SLAs distintos, não intercambiáveis. O relógio pausa fora do horário comercial (08h–18h) para chamados gerais, mas não pausa para incidentes críticos de clientes Gold.

### Possíveis ambiguidades
Clientes usam "SLA" para qualquer expectativa de prazo, incluindo prazo de entrega logístico. No domínio NovaTech, SLA refere-se exclusivamente a prazos de atendimento de chamados.

### Possíveis interpretações incorretas por um LLM
O LLM pode confundir SLA de atendimento com prazo de entrega logístico — dois conceitos completamente distintos. Pode também ignorar a regra de pausa do relógio fora do horário comercial, aplicando os mesmos critérios para chamados gerais e para incidentes críticos Gold.

### Recomendação de uso consistente
Diferenciar explicitamente "SLA de atendimento" (chamados) de "prazo de entrega" (logística). Ao citar prazos de SLA, especificar sempre o tier e o tipo (geral vs. crítico).

---

## 7. Incidente crítico
**Contexto:** Atendimento ao cliente · **Risco:** Crítico

### Definição oficial
Classificação formal de chamado que aciona prazos de SLA reduzidos para todos os tiers. Um chamado é crítico se atender ao menos um de quatro critérios (SLA-2024, seção 3): (1) carga com valor declarado acima de R$ 100.000 com status desconhecido há mais de 6 horas; (2) carga perigosa com qualquer irregularidade de documentação ou rastreamento; (3) mais de 5 chamados do mesmo cliente nas últimas 24 horas sobre o mesmo problema; (4) qualquer situação que envolva risco à segurança de pessoas.

### Contexto de uso
Aplica prazos críticos independentemente do tier. Para clientes Gold, o relógio não pausa fora do horário comercial em incidentes críticos.

### Possíveis ambiguidades
O FAQ (item 27) menciona prioridade alta para cargas acima de R$ 50.000 — critério diferente e sem base no SLA-2024. Os dois valores (R$ 50k e R$ 100k) circulam simultaneamente no vocabulário do time de atendimento.

### Possíveis interpretações incorretas por um LLM
O LLM pode aplicar o limiar de R$ 50.000 do FAQ como critério de incidente crítico, quando o documento formal (SLA-2024) define R$ 100.000 com condição adicional (status desconhecido há 6h+). São critérios distintos com finalidades distintas.

### Recomendação de uso consistente
Usar exclusivamente os quatro critérios formais do SLA-2024. O limiar de R$ 50.000 do FAQ aplica-se a prioridade de rastreamento operacional, não à classificação formal de incidente crítico.

---

## 8. Devolução
**Contexto:** Devolução de mercadorias · **Risco:** Alto

### Definição oficial
Processo formal de retorno de mercadoria já entregue ao destinatário, aberto via Portal do Cliente em até 7 dias úteis após a confirmação de recebimento no sistema de tracking. Inclui triagem (4h úteis), coleta reversa (2 dias úteis após aprovação) e reembolso ou crédito (5 dias úteis após recebimento no CD).

### Contexto de uso
Aplica-se apenas a mercadorias já entregues. Carga em trânsito não é devolução — é interceptação (PROC-088, ausente da base). Carga danificada em trânsito não é devolução — é sinistro (encaminha para sinistros@novatech.com.br).

### Possíveis ambiguidades
Clientes usam "devolução" para três situações distintas com fluxos completamente diferentes: (1) devolução pós-entrega (POL-001); (2) interceptação de carga em trânsito (PROC-088 ausente); (3) sinistro por avaria em trânsito (FAQ, sem POL formal).

### Possíveis interpretações incorretas por um LLM
O LLM pode aplicar as regras da POL-001 a situações de carga em trânsito ou avaria, que têm processos completamente distintos. O escopo da POL-001 é explicitamente limitado a mercadorias após a entrega.

### Recomendação de uso consistente
Antes de responder sobre devolução, identificar o estado da carga: entregue (POL-001) / em trânsito (encaminhar — PROC-088 ausente) / danificada em trânsito (sinistros@novatech.com.br).

---

## 9. Procedimento vigente
**Contexto:** Geral · **Risco:** Crítico

### Definição oficial
Documento normativo ou procedimental formalmente ativo para fins de aplicação. Pressupõe: versionamento explícito, responsável formal identificado e ausência de documento posterior que o revogue expressamente. Na base NovaTech, nenhum documento possui marcação formal de vigência ou obsolescência.

### Contexto de uso
Central na decisão sobre qual versão do PROC-042 aplicar. Nenhuma das duas versões está marcada como vigente ou obsoleta no SharePoint — ambas coexistem sem hierarquia documental.

### Possíveis ambiguidades
"O procedimento mais recente" não equivale a "procedimento vigente". A PROC-042 v2 (nov/2023) não revogou formalmente a v1 (mar/2023) — a própria v2 reconhece a validade da v1 para chamados anteriores a 01/12/2023.

### Possíveis interpretações incorretas por um LLM
O LLM tende a assumir que o documento de data mais recente é o vigente — heurística natural, mas incorreta neste domínio, onde coexistência de versões é o estado atual.

### Recomendação de uso consistente
O assistente não deve inferir vigência por data de emissão. Quando a vigência não estiver explicitamente definida, sinalizar a ambiguidade e solicitar confirmação antes de aplicar qualquer versão.

---

## 10. Documento obsoleto
**Contexto:** Geral · **Risco:** Crítico

### Definição oficial
Documento formalmente substituído por versão posterior, com indicação explícita de revogação no sistema de gestão documental. Na base NovaTech, nenhum documento possui marcação formal de obsolescência.

### Contexto de uso
Relevante para PROC-042 v1, que deveria ser obsoleto após a emissão da v2, mas não foi arquivado nem revogado formalmente no SharePoint.

### Possíveis ambiguidades
Ausência de marcação de obsolescência ≠ vigência presumida. Significa que o processo de governança documental não foi concluído — o status é indeterminado, não vigente.

### Possíveis interpretações incorretas por um LLM
O LLM pode tratar um documento sem marcação de obsolescência como vigente por omissão. A lógica correta é: sem marcação explícita de vigência ou obsolescência, o documento tem status indeterminado.

### Recomendação de uso consistente
Implementar metadado de status documental no pipeline de RAG com três estados: vigente / obsoleto / indeterminado. Documentos com status indeterminado devem acionar comportamento de ressalva obrigatória, não de resposta direta.

---

## 11. FAQ
**Contexto:** Geral · **Risco:** Crítico

### Definição oficial
Documento colaborativo mantido informalmente pelo time de atendimento, sem versionamento controlado, sem responsável formal e sem validação por Compliance ou Operações. Representa conhecimento prático acumulado ao longo de 2 anos, não normativa oficial.

### Contexto de uso
Única fonte disponível para rastreamento regional por rota, seguro de carga adicional e carga danificada em trânsito. Para esses temas, o FAQ é a única referência existente — mesmo sem autoridade normativa.

### Possíveis ambiguidades
O FAQ contém dados numéricos precisos (0,3%, 0,8%, R$ 50.000, 48h, 10 dias úteis) que aparentam autoridade formal. Precisão numérica não implica validação normativa.

### Possíveis interpretações incorretas por um LLM
O LLM não distingue automaticamente entre a autoridade de um documento normativo (POL, PROC, SLA) e um FAQ informal. Tratará "0,3% do valor declarado" com o mesmo nível de confiança que "7 dias úteis" da POL-001, mesmo sendo fontes com estatutos radicalmente diferentes.

### Recomendação de uso consistente
Separar o FAQ dos documentos normativos no pipeline de RAG com metadado de tipo e autoridade. Respostas baseadas exclusivamente no FAQ incluem ressalva: "conforme prática do time de atendimento — confirme com o responsável antes de aplicar". Nunca citar FAQ como base para decisões com impacto financeiro ou legal.

---

## 12. Atendimento
**Contexto:** Atendimento ao cliente · **Risco:** Baixo

### Definição oficial
Conjunto de atividades realizadas pelo time de suporte ao cliente para triagem, resposta e resolução de chamados abertos via Portal do Cliente. Está sujeito aos prazos do SLA-2024 e à hierarquia de prioridade por tier.

### Contexto de uso
Distingue-se de Gestão de Riscos (área especializada, ramal 4500), Comercial (negociações contratuais e descontos) e Jurídico (sinistros e avarias). O atendimento padrão não tem autonomia para decisões comerciais, concessão de descontos ou tratamento de riscos.

### Possíveis ambiguidades
Clientes usam "atendimento" para qualquer contato com a NovaTech. No domínio, "atendimento" é o canal de chamados via portal, não a empresa como um todo.

### Possíveis interpretações incorretas por um LLM
O LLM pode responder perguntas sobre desconto, sinistro ou carga perigosa como se fossem competência do atendimento padrão, quando cada uma dessas situações exige encaminhamento para área específica.

### Recomendação de uso consistente
Mapa de encaminhamentos do assistente: desconto → Comercial; sinistro/avaria → sinistros@novatech.com.br; carga perigosa → ramal 4500 (Riscos); SLA diferenciado → Comercial para análise de viabilidade.

---

## 13. Prazo de entrega
**Contexto:** Rastreamento e entrega · **Risco:** Alto

### Definição oficial
Tempo estimado entre a coleta da mercadoria e a confirmação de entrega ao destinatário, calculado em dias úteis com base na rota. Para frete especial (≥ 500 kg), acrescenta-se prazo adicional de manuseio definido na PROC-042 vigente (+2 dias pela v1; +3 dias pela v2).

### Contexto de uso
Tema de 35% das dúvidas recebidas. Composto por três componentes: prazo padrão da rota (sem documento formal na base atual), prazo adicional de frete especial (em conflito entre v1 e v2) e prazos regionais de anomalia (apenas no FAQ).

### Possíveis ambiguidades
Clientes frequentemente confundem prazo de entrega logístico com SLA de resolução de chamado de atendimento. São conceitos completamente distintos: um é operacional, o outro é contratual de serviço.

### Possíveis interpretações incorretas por um LLM
O LLM pode fornecer o prazo adicional da versão errada do PROC-042 (+2 dias vs. +3 dias). Pode também aplicar os prazos regionais do FAQ (Norte = 10 dias, Sul/Sudeste = anomalia em 3 dias+) como se fossem normativos, sem ressalva de autoridade.

### Recomendação de uso consistente
Para prazo de frete especial, sempre sinalizar a versão do PROC-042 utilizada. Para prazos regionais, citar o FAQ com ressalva explícita de autoridade. Nunca confundir com SLA de chamado ao responder sobre prazo de entrega.

---

## Termos adicionais identificados na documentação

---

## 14. Coleta reversa
**Contexto:** Devolução de mercadorias · **Risco:** Baixo

### Definição oficial
Serviço de retirada da mercadoria no endereço do cliente para retorno ao centro de distribuição da NovaTech, agendado em até 2 dias úteis após aprovação da triagem de devolução.

### Contexto de uso
Etapa intermediária do processo de devolução, acionada apenas após aprovação de elegibilidade. O custo é do cliente em casos de desistência; é da NovaTech em casos de erro ou avaria da transportadora.

### Possíveis ambiguidades
Clientes podem solicitar "coleta reversa" referindo-se a interceptação de carga ainda em trânsito — processo completamente diferente, sem procedimento formal disponível.

### Possíveis interpretações incorretas por um LLM
O LLM pode aplicar o prazo de 2 dias úteis de coleta reversa a situações de interceptação de carga em trânsito, que têm processo e prazo distintos (PROC-088 ausente).

### Recomendação de uso consistente
Confirmar que a carga já foi entregue e registrada no sistema de tracking antes de iniciar qualquer orientação sobre coleta reversa.

---

## 15. CT-e
**Contexto:** Devolução de mercadorias · **Risco:** Baixo

### Definição oficial
Conhecimento de Transporte Eletrônico — documento fiscal obrigatório que identifica a operação de transporte. Número exigido na abertura de qualquer chamado de devolução via Portal do Cliente.

### Contexto de uso
Sem o CT-e, o chamado de devolução não pode ser aberto no portal. É a chave de identificação da operação no sistema da NovaTech e a base de cálculo para reembolsos proporcionais em devoluções parciais.

### Possíveis ambiguidades
Clientes podem confundir CT-e com nota fiscal (NF-e) ou com o número de rastreamento do sistema. São documentos com finalidades e formatos distintos.

### Possíveis interpretações incorretas por um LLM
O LLM pode aceitar nota fiscal ou código de rastreamento como substitutos do CT-e ao orientar a abertura de chamado de devolução.

### Recomendação de uso consistente
Orientar o cliente a localizar especificamente o número do CT-e — não a NF-e, não o código de rastreamento.

---

## 16. Multiplicador regional
**Contexto:** Frete especial · **Risco:** Alto

### Definição oficial
Fator numérico aplicado ao valor base do frete especial conforme a região de destino. Há duas tabelas vigentes simultaneamente: PROC-042 v1 (mar/2023) e v2 (nov/2023), com valores distintos para todas as cinco regiões.

| Região | v1 | v2 |
|---|---|---|
| Sul | 1,2 | 1,3 |
| Sudeste | 1,0 | 1,1 |
| Centro-Oeste | 1,3 | 1,4 |
| Nordeste | 1,4 | 1,5 |
| Norte | 1,6 | 1,8 |

### Contexto de uso
Compõe a fórmula de cálculo do frete especial junto com o fator de peso. A diferença entre versões é significativa — para a região Norte, a variação é de 12,5%.

### Possíveis ambiguidades
Sem saber qual versão se aplica ao contrato, qualquer resposta sobre custo de frete especial tem probabilidade de estar errada.

### Possíveis interpretações incorretas por um LLM
O LLM tende a usar os multiplicadores da v2 (versão mais recente) por heurística de recência, ignorando que a v1 pode ainda se aplicar a contratos ou chamados anteriores a dez/2023.

### Recomendação de uso consistente
Nunca citar multiplicadores sem identificar a versão do PROC-042. Implementar separação de chunks por versão documental no pipeline de RAG para evitar mistura de parâmetros.

---

## 17. Fator de peso
**Contexto:** Frete especial · **Risco:** Alto

### Definição oficial
Multiplicador aplicado ao cálculo do frete especial conforme a faixa de peso. Há divergência entre versões: faixa 1.001–3.000 kg usa 1,2 (v1) ou 1,15 (v2); faixa acima de 3.000 kg usa 1,5 (v1) ou 1,4 (v2). A faixa de 500 kg a 1.000 kg é 1,0 em ambas as versões.

### Contexto de uso
Componente da fórmula de cálculo, distinto do multiplicador regional. Os dois se multiplicam — são fatores independentes aplicados sequencialmente.

### Possíveis ambiguidades
Fator de peso e multiplicador regional são dois componentes distintos da mesma fórmula. Atendentes e clientes às vezes os confundem como um único ajuste de preço.

### Possíveis interpretações incorretas por um LLM
O LLM pode misturar fator de peso da v1 com multiplicador regional da v2, ou vice-versa, produzindo um cálculo híbrido com parâmetros de versões diferentes.

### Recomendação de uso consistente
Nunca combinar parâmetros de versões distintas. Aplica-se ou a v1 integral, ou a v2 integral — nunca parâmetros cruzados entre versões.

---

## 18. Dias úteis
**Contexto:** Atendimento ao cliente · **Risco:** Alto

### Definição oficial
Dias da semana excluindo sábados, domingos e feriados nacionais. Base de contagem para prazos de devolução (POL-001), SLA de atendimento (SLA-2024) e prazo adicional de frete especial (PROC-042).

### Contexto de uso
O relógio de SLA pausa fora do horário comercial (08h–18h) para chamados gerais, mas não pausa para incidentes críticos de clientes Gold. O prazo de devolução (7 dias) conta dias úteis corridos desde o recebimento — sem pausa por horário. O prazo adicional de frete especial é em dias úteis corridos.

### Possíveis ambiguidades
Três semânticas distintas para o mesmo termo: SLA usa relógio com pausa; devolução usa dias completos; frete especial usa dias corridos.

### Possíveis interpretações incorretas por um LLM
O LLM pode aplicar a mesma interpretação de "dias úteis" a todos os contextos, quando SLA de chamados e prazo de devolução têm regras de contagem completamente distintas.

### Recomendação de uso consistente
Especificar sempre o contexto ao usar o termo: "dias úteis conforme POL-001" vs. "horas úteis dentro do relógio SLA-2024" vs. "dias úteis corridos conforme PROC-042".

---

## 19. Sinistro
**Contexto:** Gestão de riscos · **Risco:** Crítico

### Definição oficial
Ocorrência de dano, perda ou avaria em carga durante o transporte, que aciona processo de investigação e eventual reembolso integral pela NovaTech. Gerenciado pelo Jurídico, fora do fluxo de atendimento padrão. Canal de registro: sinistros@novatech.com.br. Prazo para registro: 48h após o recebimento (conforme FAQ — sem POL formal correspondente).

### Contexto de uso
Distinto de devolução: o sinistro implica responsabilidade da NovaTech por dano ocorrido durante o transporte. Na devolução padrão, a carga chegou íntegra e o cliente quer retorná-la.

### Possíveis ambiguidades
Clientes frequentemente usam "devolução" para situações que são sinistro. A distinção operacional: devolução = carga chegou íntegra, cliente quer devolver; sinistro = carga chegou danificada, perdida ou com avaria.

### Possíveis interpretações incorretas por um LLM
O LLM pode orientar o processo de devolução (POL-001) para um caso de sinistro, ou vice-versa. Não há documento formal de sinistro na base — apenas o FAQ descreve o processo, com risco de falsa autoridade em contexto com implicações jurídicas.

### Recomendação de uso consistente
Identificar se a carga chegou danificada ou íntegra antes de qualquer orientação. Sinistro → encaminhar para sinistros@novatech.com.br com instrução de registro em 48h e fotos. Ressalva obrigatória: o processo descrito reflete prática informal (FAQ) — sem POL formal disponível.

---

## 20. Anomalia de rastreamento
**Contexto:** Rastreamento e entrega · **Risco:** Alto

### Definição oficial
Situação em que o status de rastreamento de uma carga não apresenta atualização dentro do prazo esperado para a rota, ou permanece como "desconhecido" por período superior ao limiar definido.

### Contexto de uso
Dois critérios coexistem com finalidades diferentes: (1) SLA-2024 define incidente crítico para carga acima de R$ 100.000 com status desconhecido há mais de 6 horas — aciona prioridade de atendimento; (2) FAQ define anomalia operacional de rota para Sul/Sudeste com mais de 3 dias sem atualização e Norte com mais de 10 dias — orienta quando investigar a rota.

### Possíveis ambiguidades
Os dois critérios podem coexistir sem contradição, mas são frequentemente confundidos. Um é contratual (SLA-2024); o outro é operacional (FAQ).

### Possíveis interpretações incorretas por um LLM
O LLM pode misturar os dois critérios ou aplicar o limiar de R$ 50.000 do FAQ (item 27) como critério de incidente crítico, quando o SLA-2024 define R$ 100.000 com condição adicional de 6h de status desconhecido.

### Recomendação de uso consistente
Separar explicitamente: "anomalia operacional de rota" (FAQ, prazos regionais por rota) vs. "incidente crítico de SLA" (SLA-2024, R$ 100k + 6h status desconhecido). Os dois critérios são independentes e não intercambiáveis.

---

## 21. Devolução parcial
**Contexto:** Devolução de mercadorias · **Risco:** Baixo

### Definição oficial
Devolução de volumes individuais em uma entrega com múltiplos volumes. Cada volume segue o mesmo procedimento da POL-001. O reembolso é proporcional ao peso ou valor do volume devolvido, conforme o CT-e.

### Contexto de uso
Aplica-se quando a entrega incluiu vários volumes e o cliente deseja devolver apenas parte deles. A proporcionalidade usa o CT-e como base de cálculo.

### Possíveis ambiguidades
Clientes podem usar "devolução parcial" referindo-se à devolução de unidades dentro de um único volume. Na POL-001, o termo opera com granularidade de volumes (embalagens), não de itens individuais.

### Possíveis interpretações incorretas por um LLM
O LLM pode aplicar lógica de devolução unitária quando a POL-001 opera com granularidade de volumes. A unidade de contagem é o volume do CT-e, não a unidade de produto.

### Recomendação de uso consistente
Confirmar se a entrega incluiu múltiplos volumes (conforme CT-e) antes de orientar sobre proporcionalidade de reembolso.

---

## Apêndice: Mapa de encaminhamentos do assistente

| Situação | Canal correto | Base documental |
|---|---|---|
| Devolução de mercadoria entregue | Portal do Cliente — categoria "Devolução" | POL-001 v3.1 |
| Carga perigosa (qualquer situação) | Ramal 4500 — Gestão de Riscos | POL-001 seção 3.2 |
| Carga danificada em trânsito | sinistros@novatech.com.br | FAQ item 38 (sem POL formal) |
| Carga em trânsito — interceptação | Atendimento — aguarda PROC-088 | PROC-088 ausente |
| Desconto de frete | Comercial | PROC-042 v1/v2 |
| SLA diferenciado | Comercial para análise | SLA-2024 seção 1 |
| Seguro de carga adicional | Comercial — confirmar percentual | FAQ item 22 (sem PROC formal) |
| Frete carga perigosa +500 kg | Gestão de Riscos | PROC-043 ausente |

---

*Documento gerado em: 2026-06-15*  
*Sessão de referência: Etapa 1 — Discovery e Bounded Contexts NovaTech*  
*Autor: análise conduzida com assistência de IA (Claude — Anthropic)*
