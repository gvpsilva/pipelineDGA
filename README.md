# Pipeline DGA: Consumo, Harmonização e Enriquecimento de Dados Governamentais Abertos

[![UFMG](https://img.shields.io/badge/UFMG-DCC-red.svg)](https://www.dcc.ufmg.br/)
[![Metodologia](https://img.shields.io/badge/Metodologia-Design%20Science%20Research%20(DSR)-blue.svg)](#metodologia)
[![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow.svg)](#cronograma-e-etapas)

Repositório dedicado ao projeto e desenvolvimento do **Pipeline DGA**, um artefato voltado à integração, padronização, limpeza e enriquecimento de Dados Governamentais Abertos (DGA) no Brasil.

---

## 📌 Contextualização e Problema

A transparência pública e a disponibilização de Dados Governamentais Abertos (DGA) são pilares fundamentais para o controle social e a inovação. No entanto, no ecossistema brasileiro, a recuperação e o reúso de dados por meio de APIs públicas enfrentam sérias barreiras:

- **Fricção de Dados (*Data Friction*):** Formatos heterogêneos, ausência de padronização semântica e sintática entre órgãos.
- **Fragmentação Estrutural:** Endpoints dispersos, documentações incompletas ou ausentes e assimetria técnica entre provedores governamentais.
- **Fragilidade de Integrações:** Aplicações clientes tornam-se vulneráveis a quebras e alterações de esquemas não versionados, dificultando a rastreabilidade da proveniência dos dados.

### Pergunta de Pesquisa
> *Como projetar e implementar um pipeline de dados (ETL) para consumo de APIs governamentais abertas, capaz de mitigar a fricção de dados e viabilizar o enriquecimento e o reúso das informações?*

---

## 🎯 Objetivos

### Objetivo Geral
Projetar, desenvolver e documentar um pipeline de **ETL (Extract, Transform, Load)**, viabilizando o consumo automatizado, a harmonização e o enriquecimento de Dados Governamentais Abertos (DGA).

### Objetivos Específicos
1. **Diagnóstico Empírico:** Mapear e diagnosticar a estrutura, maturidade e limitações das APIs governamentais selecionadas.
2. **Modelagem Pipeline:** Projetar o fluxo de ETL para higienização, normalização e enriquecimento cruzado das bases.
3. **Desenvolvimento do Protótipo:** Construir um protótipo funcional para validar a ingestão e transformação contínua dos dados.
4. **Documentação:** Garantir a rastreabilidade das decisões de projeto por meio de *Architecture Decision Records* (ADRs) e diagramas formais (C4 / UML).
5. **Avaliação e Validação:** Aferir a qualidade, resiliência e integridade das informações geradas em comparação com as fontes brutas originais.

---

## 🏗️ Fluxo de execução do pipeline.
