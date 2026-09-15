# ADR-002: Diagnóstico e Mapeamento de Metadados de APIs Assistido por IA

* **Status:** Aprovado
* **Data:** 15 de setembro de 2026
* **Autor:** Gabriel Vieira Pereira da Silva (Desenvolvedor / Pesquisador)

---

## 1. Contexto

### 1.1. Problema
Conforme identificado no [ADR-001], a heterogeneidade sintática e a despadronização dos contratos de interface (Swagger/OpenAPI) de órgãos públicos impõem um esforço manual extenuante na fase de levantamento de metadados. O mapeamento manual de dezenas de rotas, campos aninhados e regras de obrigatoriedade consome tempo excessivo e é suscetível a erros de interpretação semântica. 

Além disso, tentar integrar bases distintas sem uma análise crítica de domínio pode induzir a correlações espúrias ou anti-patterns de dados (como junções com alta densidade de valores nulos ou incompatibilidade de cardinalidade).

### 1.2. Cenário Técnico e Ferramental
* **Abordagem Experimental:** Execução empírica da **Fase 1 (Diagnóstico Assistido por IA e Mapeamento dos Metadados)** prevista no [ADR-001].
* **Tecnologia Adotada:** Emprego do agente conversacional inteligente **Gemini Spark** para conduzir a engenharia reversa de contratos de API, extração de parâmetros, avaliação de pertinência lógica e proposição de objetos consolidados.
* **Fontes Inspecionadas:** APIs do Instituto Brasileiro de Geografia e Estatística (IBGE), do Senado Federal e da Câmara dos Deputados.

---

## 2. Decisão Arquitetural

Adotar o **Gemini Spark** como ferramenta assistiva para a aceleração do diagnóstico de interfaces e design de esquemas de integração, orientando a atuação da IA por meio de duas diretrizes centrais:

1. **Princípio da Validação Semântica Estrita:** A IA é instruída a correlacionar endpoints somente quando houver aderência lógica e de domínio real comprovada, refutando ativamente cruzamentos sintáticos forçados.
2. **Geração Automatizada de Contratos e Metadados de Associação:** A ferramenta é responsável por extrair dicionários de dados, identificar a obrigatoriedade de parâmetros e formalizar os esquemas dos objetos resultantes pós-ETL, gerando a documentação técnica diretamente em artefatos auditáveis (`.doc` e `.md`).

---

## 3. Metodologia e Execução dos Experimentos

O diagnóstico assistido por IA foi validado empiricamente por meio de dois experimentos de integração distintos:

### 3.1. Experimento 1: Avaliação de Viabilidade Semântica e Cardinalidade (IBGE)
* **Objetivo:** Avaliar a capacidade do modelo de discernir se dois endpoints governamentais possuem correlação legítima, prevenindo associações inconsistentes.
* **Rotas Submetidas:**
  * Endpoint 1: `GET https://servicodados.ibge.gov.br/api/v3/agregados` (Pesquisas e tabelas estatísticas do SIDRA)
  * Endpoint 2: `GET https://servicodados.ibge.gov.br/api/v1/localidades/paises` (Divisão político-geográfica internacional da ONU/ISO)
* **Prompt Executado:**
  > *"Tenho duas rotas distintas e quero ver se elas se correlacionam, apenas correlacione se realmente tiver sentido lógico e/ou semantico. Endpoint 1: https://servicodados.ibge.gov.br/api/v3/agregados | Endpoint 2: https://servicodados.ibge.gov.br/api/v1/localidades/paises. Quero que crie um documento com os metados originais de cada endpoint e os metados após gerar o relacionamento."*
* **Resultado e Diagnóstico Arquitetural:**
  * **Identificação de Anti-Pattern:** O modelo identificou corretamente que a correlação direta entre as coleções completas é inviável, uma vez que o SIDRA apura dados restritos ao território brasileiro, gerando 99,5% de nulos caso cruzado com todos os países do mundo.
  * **Identificação da Correlação Específica (N1):** Delimitou que a associação semântica é estritamente válida no nível territorial nacional (N1 - Brasil), permitindo enriquecer tabelas do SIDRA com metadados internacionais da ONU (M49: 76) e ISO 3166 (`"BR"`).
  * **Artefato Entregue:** [Documento Técnico: Análise de Interoperabilidade e Metadados (IBGE Agregados vs. Países)](https://docs.google.com/document/d/1Oj0rcuY-wMWwJDC9ESUQghqNrfu-ZK99uBxhVuZbp-I/edit?usp=sharing).

---

### 3.2. Experimento 2: Extração Estruturada e Interoperabilidade Bicameral (Congresso Nacional)
* **Objetivo:** Analisar especificações OpenAPI/Swagger de duas instituições federativas independentes, mapeando parâmetros e desenhando a fusão do processo legislativo bicameral.
* **Fontes Inspecionadas:**
  * API 1: [Swagger UI - Senado Federal](https://legis.senado.leg.br/dadosabertos/api-docs/swagger-ui/index.html)
  * API 2: [Swagger UI - Câmara dos Deputados](https://dadosabertos.camara.leg.br/swagger/api.html)
* **Prompt Executado:**
  > *"Tenho dois swaggers de apis distintos, quero que extraia os metadados dessas apis, indicando parametros obrigatorios ou nao. Posteriormente busque relacionar as duas apis, encontrando endpoints que possam se relacionar e constuir uma nova informação de maneira enriquecida, construa os metadados desses relacionamento. API 1: https://legis.senado.leg.br/dadosabertos/api-docs/swagger-ui/index.html | API 2: https://dadosabertos.camara.leg.br/swagger/api.html. Gere um documento .doc e um .md para documentar esse processo."*
* **Resultado e Diagnóstico Arquitetural:**
  * **Mapeamento de Parâmetros e Regras de Negócio:** Extração e classificação de parâmetros em *Obrigatórios*, *Opcionais* e *Condicionais* (como a regra do Senado que exige combinações mínimas em `/materia/pesquisa/lista` para prevenir consultas irrestritas).
  * **Chave Natural Composta de Ligação:** Definição da chave `(tipo_proposicao, numero_proposicao, ano_apresentacao)` para conectar as entidades, superando a barreira de identificadores sintáticos isolados (`id` da Câmara vs. `CodigoMateria` do Senado).
  * **Objeto Resultante Enriquecido:** Modelagem da entidade *"Linha do Tempo Bicameral da Matéria"*, atendendo à Boa Prática 31 do W3C DWBP (geração de novos dados analíticos com proveniência W3C PROV-O).
  * **Artefatos Entregues:**
    * Documento Google Docs (.doc): [Mapeamento de Interoperabilidade - Senado e Câmara](https://docs.google.com/document/d/1z7zpu1IsNTPLr7YonCk4S5y0YS2StkcXo0Ie1pNdpY0/edit?usp=sharing)
    * Documento Markdown (.md): [Mapeamento_Interoperabilidade_Senado_Camara.md](https://docs.google.com/document/d/1EbSEovJrSuzXeuzP5MFhEDqmUjV-F_BfmKCPvD0JGr4/edit?usp=sharing)

---

## 4. Justificativa e Trade-offs das Decisões

* **Ganhos de Produtividade e Descoberta Semântica:** Aceleração substancial na inspeção de contratos OpenAPI extensos e na identificação de chaves primárias e estrangeiras implícitas.
* **Mitigação de Alucinação:** O direcionamento explícito para avaliar o sentido lógico/semântico impediu que a IA gerasse cruzamentos fantasiosos entre bases incompatíveis (como comprovado no caso IBGE Agregados vs. Países).
* **Dependência de Validação Humana:** Embora a IA acelere o mapeamento, o arquiteto/engenheiro de software deve manter a revisão determinística das regras de negócio e limites operacionais de taxa (*rate limits*) dos endpoints.

---

## 5. Próximos Passos (Transição para a Fase 2)

Com a aprovação dos esquemas de dados, matrizes de de-para e dicionários de metadados gerados nesta fase:
1. Utilizar os contratos gerados pelo Gemini Spark como especificação formal de entrada para os módulos de **Transformação** do pipeline ETL (Fase 2 do [ADR-001]).
2. Implementar os conectores de extração com suporte a resiliência sistêmica (backoff exponencial e controle de concorrência) para os endpoints validados.
