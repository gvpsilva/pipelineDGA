# ADR-001: Pipeline para consumo de dados abertos

* **Status:** Proposta
* **Data:** 01 de agosto de 2026
* **Autor:** Gabriel Vieira Pereira da Silva (Desenvolvedor)

## 1. Contexto

### 1.1. Problema
A reutilização de Dados Governamentais Abertos (DGA) por sistemas clientes enfrenta barreiras críticas decorrentes da arquitetura de publicação dos órgãos públicos:

* **Fricção de Dados (Data Friction) e Heterogeneidade:** Formatos distintos, inconsistências sintáticas e ausência de padronização semântica elevam drasticamente o custo computacional e humano para processar os dados.
* **Múltiplos Endpoints Desconexos:** Os dados encontram-se dispersos em fontes distintas, sem vínculos semânticos diretos ou vocabulários controlados que viabilizem cruzamentos automáticos. 
* **Fragilidade de Integrações e Quebras de Contrato:** consumo direto das APIs públicas expõe os sistemas a quebras repentinas (breaking changes) decorrentes de alterações estruturais e esquemas não versionados.
Além disso, a falta de documentação adequada impede a rastreabilidade da proveniência da informação

### 1.2. Cenário Técnico e Restrições

* **Bases de Origem e Repositório de Metadados:** O projeto foca especificamente no consumo das APIs do Catálogo de APIs governamentais desenvolvido no âmbito do Observatório de Dados Abertos.
* **Abordagem Experimental com LLMs:** Investigação exploratória de modelos de linguagem (especificamente Google Gemini e OpenAI ChatGPT)
atuando sobre especificações técnicas (OpenAPI/Swagger) para auxiliar na descoberta de correlações entre esquemas e no mapeamento de rotas heterogêneas.

## 2. Decisão Arquitetural
Adotar uma camada intermediária de integração sustentada por um pipeline de ETL (Extração, Transformação e Carga):

--imagem

* **Padrão de Processamento (ETL):**
    * **Extração:** Conexão automatizada, paginação e ingestão isolada das fontes governamentais
    * **Transformação:** Limpeza de ruídos, resolução de ambiguidades, conciliação e enriquecimento cruzado de dados.
    * **Carga:** Disponibilização dos conjuntos harmonizados via estruturas padronizadas legíveis por máquina para consumo analítico e aplicações terceiras.

    
Sendo tudo realizado adotando padrões e práticas de mercado da Engenharia de Software.

## 3. Estratégia de Implementação Faseada
A implementação seguirá os preceitos da metodologia Design Science Research (DSR) em ciclos incrementais.

### Fase 1: Diagnóstico Assistido por IA e Mapeamento dos Metadados

Objetivo:Analisar contratos de interface (Swagger/OpenAPI) e esquemas do Catálogo de APIs, utilizando o Gemini/ChatGPT de forma experimental para identificar pontos de cruzamento 
e compatibilidade semântica

* **Entrega de Valor:** Validação empírica da viabilidade do uso de LLMs no auxílio ao design de esquemas de integração, documentando o ganho de eficiência frente à análise puramente manual

### Fases 2: Prototipação do Pipeline ETL
Implementação prática dos fluxos de Extração, Transformação e Carga. A camada de transformação implementará rotinas de higienização, tratamento de nulos e cruzamento estruturado.
* **Ferramenta:** Realizar a análise comparativa entre Scripts e Ferramenta de integrações de dados.

### Fases 3: Validação, Interoperabilidade e Avaliação da Solução
Confrontar os dados processados e enriquecidos com as fontes brutas governamentais, avaliando fidelidade da informação, integridade e mitigação da fricção. Além disso o registro contínuo das decisões e realizações.

## 4. Justificativa e Trade-offs das Decisões

Camada Intermediária de ETL vs. Consumo Direto pelas Aplicações Clientes:sola as aplicações clientes da instabilidade das APIs governamentais,
unifica esquemas e centraliza as regras de higienização e enriquecimento de dados, 
mas Introduz latência inerente ao processamento intermediário e demanda esforço contínuo de manutenção da camada.

Uso Experimental de LLMs no Diagnóstico dos Esquemas: Acelera a descoberta de relações semânticas e a catalogação de endpoints volumosos e mal documentados.
Risco de alucinação de esquemas, exigindo validação humana e técnica determinística sobre as sugestões geradas.

## 5. Alternativas Consideradas

* **Construção e Mapeamento sem Apoio de IA:** Mapear manualmente centenas de campos, rotas e tipos a partir das páginas Swagger/OpenAPI, processo mais oneroso. 
* **Possibilidades de Disponibilização:** Disponibilizar os dados processados pelo pipeline, em uma arquitetura que permita o uso por diferentes aplicações.  
