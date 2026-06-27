# Exercício 1.3 — Especificação de Requisitos de RAG

> **Projeto:** Assistente de Atendimento com RAG — NovaTech Logística
> **Versão do documento:** 1.1
> **Status:** Em revisão
> **Data:** Junho 2026

---

## 1. Contexto

### Cenário

A NovaTech é uma empresa de logística que opera com uma equipe de atendimento ao cliente responsável por resolver dúvidas operacionais, consultar procedimentos e comunicar políticas internas aos clientes. O volume de consultas exige agilidade e consistência de informação — dois atributos que o modelo atual não garante.

### Problemas identificados

Durante a fase de discovery, foram mapeados os seguintes problemas estruturais:

| # | Problema | Impacto operacional |
|---|----------|-------------------|
| 1 | Documentação dispersa entre SharePoint, Confluence e planilhas | Atendente precisa buscar em múltiplos sistemas para responder uma única consulta |
| 2 | PROC-042 possui duas versões ativas com instruções conflitantes | Risco de respostas inconsistentes e decisões operacionais incorretas |
| 3 | FAQ mantido informalmente, sem validação ou curadoria formal | Respostas potencialmente desatualizadas ou imprecisas em circulação |
| 4 | Ausência de governança documental unificada | Sem controle de versão, responsabilidade ou ciclo de vida dos documentos |
| 5 | Tempo médio de 12 minutos por consulta para localizar informação | Baixa produtividade e aumento do tempo de resolução para o cliente |
| 6 | 15% das consultas escaladas para supervisores por falta de informação acessível | Sobrecarga de liderança e aumento do custo operacional |

### Objetivo do assistente de IA

Implantar um assistente baseado em RAG (*Retrieval-Augmented Generation*) que permita à equipe de atendimento consultar a base documental corporativa em linguagem natural, recebendo respostas precisas, rastreáveis e fundamentadas exclusivamente na documentação oficial vigente.

O assistente não substitui o julgamento humano — ele reduz o tempo de acesso à informação e aumenta a consistência das respostas entregues aos clientes.

---

## 2. Inputs Utilizados

### 2.1 Cenário

Empresa de logística com equipe de atendimento ao cliente que depende de documentação técnica dispersa para resolver consultas operacionais. O contexto inclui alta rotatividade de procedimentos, múltiplos sistemas de armazenamento documental e ausência de processo formal de curadoria.

### 2.2 Dados do discovery

- **Fontes documentais ativas:** SharePoint corporativo, Wiki Confluence, planilhas de referência (SLAs, tarifas, rotas)
- **Problema crítico identificado:** PROC-042 com duas versões ativas e instruções divergentes (v2 e v3)
- **FAQ informal:** mantido sem responsável, sem data de revisão, sem processo de aprovação
- **Métricas operacionais coletadas:**
  - Tempo médio de busca: **12 minutos por consulta**
  - Taxa de escalonamento: **15% dos atendimentos**
- **Ausências identificadas:** sem metadados padronizados, sem hierarquia de confiabilidade entre fontes, sem processo de obsolescência documental

### 2.3 Documentação analisada

- Especificação funcional inicial (v1.0) — produzida na primeira iteração do exercício
- Análise crítica gerada por IA — identificando ambiguidades, riscos, requisitos incompletos e cenários não tratados
- Refinamentos aplicados nas versões subsequentes:
  - v1.1: revisão dos metadados obrigatórios (simplificação para 4 campos)

### 2.4 Explicação simplificada do pipeline de RAG

O RAG (*Retrieval-Augmented Generation*) é uma arquitetura que combina dois componentes principais:

```
[Pergunta do atendente]
        │
        ▼
┌─────────────────┐
│   RETRIEVAL     │  ← Busca os trechos mais relevantes na base documental
│  (busca vetorial)│    usando similaridade semântica (embeddings)
└────────┬────────┘
         │  trechos recuperados + score de relevância
         ▼
┌─────────────────┐
│   GENERATION    │  ← Modelo de linguagem gera a resposta
│  (LLM)         │    fundamentada apenas nos trechos recuperados
└────────┬────────┘
         │
         ▼
[Resposta com citação de fonte]
```

**Princípio fundamental para este projeto:** o assistente só responde quando os trechos recuperados ultrapassam um limiar mínimo de confiança. Abaixo desse limiar, informa explicitamente que não encontrou resposta suficiente na base — nunca gera conteúdo sem respaldo documental.

---

## 3. Primeira Versão da Especificação

A especificação inicial foi estruturada em seis seções, cobrindo os requisitos funcionais básicos do assistente.

### 3.1 Fontes de dados

Devem ser indexados:
- SharePoint corporativo
- Wiki Confluence
- Planilhas de referência vigentes

Não devem ser indexados:
- Documentos obsoletos
- Documentos arquivados
- Documentos sem responsável definido

Documentos obsoletos devem permanecer acessíveis para auditoria, porém não utilizados para geração de respostas.

### 3.2 Tratamento de documentos contraditórios

Quando existirem versões conflitantes de um mesmo procedimento:
- O assistente não deve escolher uma versão automaticamente
- Deve informar a existência do conflito
- Deve apresentar as versões encontradas
- Deve indicar a necessidade de validação humana

### 3.3 Ausência de resposta

Quando não houver informação suficiente:
- O assistente deve informar explicitamente que não encontrou resposta
- Não deve utilizar conhecimento geral da internet
- Deve sugerir documentos relacionados quando possível
- Deve permitir escalonamento para supervisor

### 3.4 Atualização

Novos documentos aprovados devem estar disponíveis para consulta em até 24 horas após publicação.

### 3.5 Rastreabilidade

Toda resposta deve:
- Indicar a fonte utilizada
- Exibir documento e versão
- Permitir acesso ao trecho utilizado na resposta

### 3.6 Critérios de sucesso

- Reduzir o tempo médio de busca de 12 para menos de 2 minutos
- Reduzir escalonamentos para supervisor
- Aumentar consistência das respostas

---

## 4. Prompt de Revisão Utilizado

O prompt abaixo foi utilizado para solicitar análise crítica da especificação produzida na seção anterior:

```
Atue como um Product Specialist especialista em soluções RAG corporativas.

Analise a especificação abaixo.
Identifique:
- ambiguidades
- requisitos incompletos
- riscos
- critérios não mensuráveis
- cenários não tratados

Apresente sugestões de melhoria.
```

**Decisões de design do prompt:**

- **Papel explícito** (`Atue como...`): direciona o modelo para um frame de avaliação crítica, não de geração de conteúdo
- **Lista de categorias de análise**: garante cobertura sistemática dos tipos de gap mais comuns em especificações de produto
- **Ausência de restrições de formato**: permite que o modelo organize a resposta na estrutura mais adequada para o conteúdo encontrado
- **Instrução de melhoria** ao final: transforma a análise em entregável acionável, não apenas diagnóstico

---

## 5. Feedback Recebido do Claude

A análise crítica identificou **23 pontos** distribuídos em cinco categorias.

### 5.1 Riscos

| ID | Severidade | Descrição |
|----|-----------|-----------|
| R1 | Crítico | Hierarquia de fontes por plataforma (SharePoint > Confluence) é arbitrária — um documento Confluence aprovado e recente seria preterido por um SharePoint desatualizado |
| R2 | Crítico | PROC-042 não pode entrar no go-live sem resolução prévia — conflito ativo desde o primeiro dia degrada a credibilidade do assistente |
| R3 | Alto | Ausência de controle de acesso por perfil — conteúdo sensível (tarifas negociadas, dados contratuais) pode ser exposto a todos os atendentes |
| R4 | Alto | Sem plano para indisponibilidade das fontes — assistente pode operar com base desatualizada sem sinalização |
| R5 | Alto | Logs contêm dados pessoais sem política de LGPD definida — base legal, mascaramento e prazo não especificados |
| R6 | Médio | Planilhas como fonte são estruturalmente frágeis — sem versionamento nativo ou controle de aprovação |

### 5.2 Ambiguidades

| ID | Localização | Descrição |
|----|------------|-----------|
| A1 | §1.1 | "Vigente" não tem definição operacional — critério de vigência não especificado |
| A2 | §1.3 | "Responsável técnico" como nome de pessoa cria fragilidade quando há rotatividade |
| A3 | §2 | Critério de detecção de "versões conflitantes" não definido — similaridade semântica ou conflito formal por código? |
| A4 | §3.1 | Vedação ao "conhecimento geral da internet" é tecnicamente inverificável em modelos de linguagem |
| A5 | §5.2 | Fluxo de correção após feedback "incorreto" não definido — sem SLA, responsável ou notificação |
| A6 | §5 | "Fonte primária" implica múltiplas fontes, mas comportamento com múltiplos documentos não especificado |
| A7 | §4 | "24h após publicação" — publicação é o save do arquivo, o status aprovado, ou a aprovação pelo curador? |

### 5.3 Requisitos incompletos

| ID | Descrição |
|----|-----------|
| I1 | Sem requisitos de interface e UX — canal de acesso, formato de resposta e histórico de conversa não definidos |
| I2 | Sem requisitos de desempenho — latência alvo, uptime e comportamento em degradação ausentes |
| I3 | Processo de curadoria sem responsável, SLA ou capacidade definida |
| I4 | Sem plano de rollout — critérios de aceite, piloto e treinamento de atendentes não especificados |
| I5 | Multimodalidade não tratada — PDFs escaneados, imagens, fluxogramas e tabelas complexas sem definição de escopo |

### 5.4 Critérios de sucesso não mensuráveis

| ID | KPI problemático | Problema identificado |
|----|-----------------|----------------------|
| K1 | Consistência ≥ 90% positivo | Depende de feedback voluntário com viés de seleção — não representa qualidade real |
| K2 | Cobertura ≥ 80% das perguntas | Universo de "perguntas" indefinido — 80% de qual conjunto de referência? |

### 5.5 Cenários não tratados

| ID | Cenário |
|----|---------|
| C1 | Pergunta ambígua ou mal formulada pelo atendente — sistema entrega resposta irrelevante sem sinalizar |
| C2 | Documento atualizado durante consulta ativa — atendente age com base em versão desatualizada |
| C3 | Atendente discorda da resposta — sem mecanismo estruturado para reportar com contexto |
| C4 | Consulta sobre dados em tempo real (status de pedido, capacidade de rota) — fora do escopo RAG, mas sem redirecionamento |
| C5 | Ingestão de documento com conteúdo sensível ou restrito — sem triagem de confidencialidade |

---

## 6. Ajustes Realizados

| Ponto identificado | Impacto | Ajuste realizado |
|-------------------|---------|-----------------|
| Metadados obrigatórios excessivamente complexos (5 campos com ambiguidades) | Alto — dificuldade de adoção e preenchimento inconsistente nas fontes | Simplificação para 4 campos obrigatórios: proprietário (nome), data de publicação, versão e status. Status reduzido a dois valores: vigente ou obsoleto |
| Campo "responsável técnico (nome e área)" cria fragilidade em rotatividade | Médio — documentos ficam órfãos quando responsável muda de cargo ou sai | Renomeado para "proprietário" com vínculo ao nome da pessoa. Recomendação de processo de reatribuição adicionada como decisão em aberto |
| Status de validação com 4 valores (rascunho/aprovado/arquivado/obsoleto) gerava ambiguidade operacional | Alto — critério de exclusão da base dependia de interpretação dos estados intermediários | Simplificação para binário: vigente (indexado e ativo) ou obsoleto (preservado para auditoria, fora da base ativa) |
| Documentos sem metadados eram simplesmente excluídos da indexação | Médio — risco de perda de conteúdo relevante sem visibilidade para o time | Novo comportamento: indexados com status pendente e inativos até correção, gerando fila de triagem visível |
| Regra de exclusão referenciava "responsável técnico" — termo removido da spec | Baixo — inconsistência de vocabulário entre seções | Seção 1.2 atualizada para "documentos sem proprietário definido" |
| Hierarquia de fontes por plataforma favorecia origem em vez de qualidade documental | Crítico — respostas potencialmente incorretas com aparência de autoridade | Hierarquia reformulada: prevalece status de aprovação, depois data de aprovação, depois número de versão. Plataforma removida como critério |
| PROC-042 tratado como item de backlog pós-lançamento | Crítico — conflito ativo desde o go-live destrói credibilidade do assistente | Movido para pré-requisito de go-live: nenhum documento com flag `CONFLITO_DETECTADO` pode estar na base ativa na data de lançamento |
| Critério "≥ 90% positivo" dependente de feedback voluntário enviesado | Alto — KPI não representa qualidade real das respostas | Complementado com avaliação amostral mensal: curador revisa 50 respostas aleatórias por rubrica de 3 critérios (fonte correta, resposta completa, sem distorção) |
| Universo de referência para "80% de cobertura" indefinido | Alto — impossível medir sem conjunto de perguntas de referência | Definido: catalogar 100–200 perguntas mais frequentes antes do go-live para uso como benchmark de cobertura |

---

## 7. Versão Final da Especificação

### 7.1 Fontes de Dados

#### Fontes indexadas

| Fonte | Conteúdo | Conector |
|-------|----------|----------|
| SharePoint corporativo | Procedimentos operacionais, POPs, normativas internas | Microsoft Graph API |
| Wiki Confluence | Base de conhecimento, FAQs, documentação de processos | Confluence REST API |
| Planilhas de referência vigentes | SLAs, códigos de status, tarifas, rotas | Google Sheets / Excel API |

#### Fontes excluídas da indexação ativa

Não devem ser utilizados para geração de respostas:
- Documentos com status **obsoleto**
- Documentos com status **pendente** (metadados incompletos)
- Documentos sem proprietário definido

> Documentos obsoletos permanecem acessíveis nas fontes de origem para fins de auditoria. A exclusão é apenas da base ativa do assistente.

#### Metadados obrigatórios

Todo documento deve obrigatoriamente conter os quatro campos abaixo. Documentos com metadados incompletos são indexados com **status pendente** e ficam inativos até a correção:

| Campo | Descrição | Valores |
|-------|-----------|---------|
| **Proprietário** | Nome da pessoa responsável pelo documento | Texto livre |
| **Data de publicação** | Data em que o documento foi publicado na fonte de origem | Data |
| **Versão** | Número de versão (ex: 1.0, v2, 3) | Texto livre |
| **Status** | Estado atual do documento | `vigente` ou `obsoleto` |

---

### 7.2 Tratamento de Documentos Contraditórios

#### Definição de conflito

Conflito é identificado em duas dimensões:

- **Conflito formal:** mesmo código de processo (ex: PROC-042) com múltiplas versões ativas simultaneamente
- **Conflito semântico:** trechos de documentos distintos com instruções contraditórias para o mesmo cenário, detectadas por comparação de embeddings

#### Comportamento esperado

Quando conflito for detectado e não resolvível pela hierarquia abaixo:

1. O assistente **não seleciona** uma versão automaticamente
2. **Informa** ao atendente que há versões conflitantes
3. **Apresenta** as versões com identificação clara (código, versão, data de publicação)
4. **Indica** a necessidade de validação humana antes de prosseguir

**Exemplo de resposta ao atendente:**

> *"Encontrei versões conflitantes para este procedimento. A versão mais recente (PROC-042 v3, publicada em mar/2026) indica [X]. Existe também a versão v2 (out/2025) com instrução diferente. Recomendo confirmar com seu supervisor antes de prosseguir. Este conflito foi registrado para revisão documental."*

#### Hierarquia de resolução automática

Quando a hierarquia abaixo resolver o conflito sem ambiguidade, o assistente responde sinalizando a fonte utilizada:

1. Documento com status `vigente` prevalece sobre `obsoleto` ou `pendente`
2. Data de publicação mais recente prevalece em caso de empate de status
3. Número de versão maior prevalece em caso de empate de data
4. Se nenhuma regra resolver: assistente não responde e encaminha para curadoria

> **Nota:** a plataforma de origem (SharePoint vs. Confluence) não é critério de hierarquia — apenas atributos documentais são considerados.

#### Registro de conflitos

Todo conflito detectado — resolvido ou não pela hierarquia — deve ser:
- Registrado automaticamente com flag `CONFLITO_DETECTADO`
- Encaminhado para a fila de revisão da área responsável com prioridade alta
- Identificado com as versões conflitantes e data de detecção

> **Pré-requisito de go-live:** nenhum documento com flag `CONFLITO_DETECTADO` pode estar na base ativa na data de lançamento. O PROC-042 deve ser resolvido antes da implantação.

---

### 7.3 Comportamento para Ausência de Resposta

#### Restrições

- O assistente informa explicitamente que não encontrou resposta na base
- **Não utiliza** conhecimento paramétrico do modelo para preencher lacunas
- **Não gera** respostas plausíveis sem trechos recuperados acima do limiar de confiança configurado
- Toda resposta deve ser fundamentada em trechos com score ≥ threshold (valor padrão: 0,75 — ajustável nos primeiros 30 dias)

#### Ações de fallback

Quando o assistente não encontrar resposta suficiente:

1. Informa explicitamente ao atendente
2. Sugere documentos relacionados, se o retrieval retornar resultados próximos mas insuficientes
3. Apresenta opção de escalonamento para supervisor com contexto da consulta
4. Registra a pergunta no backlog de documentação para preenchimento pela equipe de conteúdo

**Exemplo de resposta:**

> *"Não encontrei informação suficiente na base atual para responder com segurança. Sua dúvida foi registrada para revisão documental. Para urgências, acione seu supervisor ou consulte [documento relacionado, se disponível]."*

#### Escopo declarado

O assistente responde sobre procedimentos, políticas e referências documentais. Consultas sobre dados transacionais em tempo real (status de pedido, rastreamento, capacidade de rota) estão fora do escopo e devem ser redirecionadas para os sistemas operacionais correspondentes (TMS, ERP).

---

### 7.4 Atualização da Base de Conhecimento

**SLA de disponibilidade:** documentos aprovados devem estar disponíveis para consulta em até **24 horas após receberem status `vigente`** na fonte de origem.

| Mecanismo | Descrição |
|-----------|-----------|
| Sincronização automática | Crawler a cada 4 horas para SharePoint e Confluence. Detecta criações, modificações e mudanças de status |
| Ingestão manual | Interface para upload de documentos externos. Metadados obrigatórios devem ser preenchidos antes da indexação |
| Versionamento | Toda versão anterior é mantida em arquivo imutável. Respostas geradas são vinculadas ao ID e versão do documento utilizado |
| Fluxo de aprovação | Documentos de fontes informais (ex: FAQ legado) passam por aprovação de ao menos um curador antes de entrar na base ativa |
| Obsolescência | Documentos marcados como obsoletos são removidos da base ativa na próxima sincronização, mas preservados para auditoria |
| Monitoramento de SLA | Alerta automático quando documento aprovado ultrapassar 20h sem estar disponível no assistente |

---

### 7.5 Rastreabilidade

#### Informações exibidas em cada resposta

| Elemento | Descrição |
|----------|-----------|
| Nome do documento | Título oficial da fonte primária utilizada |
| Versão | Número de versão no momento da consulta |
| Data de publicação | Data de publicação registrada nos metadados |
| Trecho utilizado | Excerto exato que fundamentou a resposta, acessível com um clique |
| Link para a fonte | Acesso direto ao documento no SharePoint ou Confluence |

Quando a resposta combinar trechos de múltiplos documentos, todas as fontes devem ser exibidas (limite: 3 fontes por resposta). Respostas com mais de uma fonte exibem aviso de síntese ao atendente.

#### Log de interações

Cada consulta é registrada com os seguintes campos:

| Campo | Descrição |
|-------|-----------|
| ID do atendente | Identificação do usuário (mascaramento de dados sensíveis aplicado) |
| Timestamp | Data e hora da consulta |
| Pergunta original | Texto da consulta, com mascaramento automático de CPF, telefone e dados pessoais identificados |
| Trechos recuperados | Chunks retornados e scores de relevância |
| Resposta gerada | Texto entregue ao atendente |
| Avaliação | Resultado do feedback inline |

> Retenção mínima: 90 dias. Acesso restrito a perfis de governança e compliance. Política completa de tratamento de dados a ser validada com o DPO antes do início da coleta.

#### Feedback inline

Toda resposta exibe opção de avaliação ao final:

- **Útil** — resposta adequada e correta
- **Incompleta** — informação parcial, contexto insuficiente
- **Incorreta** — conteúdo diverge do procedimento real

Avaliações negativas geram ticket automático para a equipe de curadoria com SLA de triagem em até 5 dias úteis e resolução em até 15 dias úteis. O atendente que reportou é notificado quando a correção for aplicada.

---

### 7.6 Critérios de Sucesso

| KPI | Baseline | Meta | Prazo | Método de medição |
|-----|----------|------|-------|-------------------|
| Tempo médio de busca por consulta | 12 minutos | < 2 minutos | 30 dias | Cronometragem amostral pré e pós implantação |
| Taxa de escalonamento para supervisor | 15% | < 5% | 60 dias | Registro de tickets de escalonamento |
| Precisão factual das respostas | Não mensurado | ≥ 90% | 60 dias | Avaliação amostral mensal: 50 respostas por rubrica de 3 critérios (fonte correta / resposta completa / sem distorção) |
| Cobertura da base | Não mensurado | ≥ 80% | 90 dias | % das 150 perguntas de referência respondidas com score ≥ threshold |
| Satisfação do atendente | Não mensurado | ≥ 4,0 / 5,0 | 60 dias | NPS interno mensal |
| Conflitos documentais ativos | Não mensurado | 0 no go-live | Pré-lançamento | Auditoria manual da base antes da implantação |

> **Universo de referência para cobertura:** antes do go-live, a equipe de produto deve catalogar as 150 perguntas mais frequentes identificadas no histórico de escalonamentos e e-mails de atendimento. Esse conjunto será o benchmark fixo de cobertura.

> **Cadência de revisão:** 30, 60 e 90 dias após o go-live, com ajuste de thresholds conforme volume real de consultas e feedback dos atendentes.

---

## 8. Reflexão sobre o Uso da IA

### O papel da iteração com IA na qualidade da especificação

A construção desta especificação seguiu um modelo de **co-criação iterativa** com IA, em que cada ciclo de geração e revisão produziu um entregável mais robusto do que seria possível em uma única passagem.

Três contribuições foram especialmente relevantes:

**1. Antecipação de riscos não óbvios**

A análise crítica da IA identificou riscos que raramente aparecem nas primeiras versões de especificações de produto — em particular, o risco de LGPD nos logs de interação e o problema de controle de acesso por perfil. Ambos teriam impacto legal relevante se descobertos apenas na fase de desenvolvimento ou, pior, após o lançamento. A IA funcionou como um revisor sistemático que não tem os mesmos pontos cegos do autor original.

**2. Identificação de ambiguidades de implementação**

Requisitos que pareciam claros na especificação revelaram-se ambíguos quando analisados do ponto de vista de implementação. A vedação ao "conhecimento geral da internet" é um exemplo direto: a instrução é intuitiva para um product manager, mas tecnicamente irrealizável em modelos de linguagem — o conhecimento paramétrico está nos pesos do modelo e não pode ser desativado. A IA traduziu essa intenção para um requisito verificável: *"toda resposta deve ser fundamentada em trechos recuperados acima do threshold"*.

**3. Estruturação de critérios mensuráveis**

Os critérios de sucesso da versão inicial tinham o problema clássico de especificações de produto: metas sem método de medição. "Aumentar consistência das respostas" não é um critério — é uma intenção. A iteração com IA forçou a especificação dos universos de referência, dos métodos de coleta e da separação entre indicadores de satisfação (feedback inline) e indicadores de qualidade (avaliação amostral por curador).

### Limitações observadas

A IA não substitui o conhecimento de contexto. Os problemas específicos da NovaTech — a existência do PROC-042, a dinâmica da equipe de atendimento, as restrições de LGPD aplicáveis ao setor de logística — precisaram ser fornecidos como input. A IA amplificou e sistematizou esse conhecimento, mas não o gerou.

### Aprendizado para uso em produto

O padrão mais eficaz observado neste exercício foi: **gerar → revisar com papel explícito → refinar especificamente**. Prompts genéricos ("melhore essa especificação") produzem resultados superficiais. Prompts com papel definido, categorias de análise e instrução de ação produzem feedback acionável que pode ser diretamente incorporado ao documento.

---

*Documento gerado como entregável do Exercício 1.3 — Especificação de Requisitos de RAG*
*NovaTech Logística · Product Management · Junho 2026*
