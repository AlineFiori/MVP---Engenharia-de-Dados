# 🤰 Pipeline de Engenharia de Dados & Machine Learning: Predição de Risco Gestacional

Repositório contendo o projeto completo de desenvolvimento de uma solução de **Engenharia e Ciência de Dados** utilizando o ecossistema **Databricks** (PySpark, Delta Lake e MLlib/Scikit-Learn). O projeto aborda a análise e predição de riscos de saúde em gestantes com base no dataset *Maternal Health Risk*.

---

## 🛠️ Arquitetura do Projeto (Arquitetura Medalhão)
O pipeline foi construído seguindo os padrões da **Medallion Architecture**, garantindo qualidade progressiva, rastreabilidade e governança dos dados:

1. **Camada Bronze (Raw):** Persistência imutável dos dados brutos originais obtidos via GitHub, adicionando metadados de rastreabilidade de ingestão (`data_ingestao`).
2. **Camada Silver (Refined & Data Quality):** 
   * Implementação de um **Circuit Breaker** (validação automatizada de integridade que bloqueia o pipeline caso encontre anomalias, como idade < 10 ou pressão <= 0).
   * Padronização de nomes de colunas, remoção de duplicatas e tratamento de valores nulos.
3. **Camada Gold (Curated & Features):**
   * Engenharia de atributos (*features*) de valor para o negócio: criação de faixas etárias (destacando a **Idade Materna Avançada / 35+**), indicadores de hipertensão (`flag_hipertensao`) e codificação da variável alvo (`target_risk_encoded`).
   * Aplicação de rotinas de otimização de performance (`OPTIMIZE`/`VACUUM`) e governança de acesso (`GRANT SELECT`).

---

## 📊 Principais Descobertas e Análises (Camada Gold)
Por meio de consultas em Spark SQL e análises estatísticas, o projeto respondeu a hipóteses cruciais de negócio:
* **Grupo 35+:** Representa aproximadamente **30,31%** da amostra avaliada.
* **Alto Risco em 35+:** Cerca de **39,42%** das gestantes na faixa etária avançada concentram classificações de alto risco.
* **Correlação de Pearson ($r = 0.1830$):** Apontou uma correlação positiva fraca entre idade materna isolada e o nível de risco, comprovando clinicamente que complicações na gravidez derivam de uma **dinâmica multifatorial** (envolvendo glicemia e pressão arterial), justificando a necessidade de algoritmos preditivos avançados.

---

## 🤖 Modelagem Preditiva (Machine Learning)
Foram testados e otimizados três algoritmos de classificação supervisionada (*Logistic Regression*, *Decision Tree* e *Random Forest*):
* **Tratamento de Dados:** Utilização de `StandardScaler` para normalização e mitigação de convergência, além de `class_weight='balanced'` para lidar com o desbalanceamento de classes severo (evitando que o modelo ignorasse o risco moderado/médio).
* **Modelo Campeão:** **Random Forest (Otimizado)**
  * **Acurácia Global:** 64,84%
  * **Sensibilidade (Recall) em Alto Risco:** **83%** (alto poder de detecção de casos críticos, reduzindo drasticamente falsos negativos essenciais na triagem médica).

---

## 🚀 Como Executar o Projeto
1. Clone o repositório para o seu ambiente ou importe o notebook para o seu workspace do **Databricks**.
2. Certifique-se de configurar o acesso ao arquivo CSV do dataset no repositório de origem.
3. Execute as células sequencialmente para simular o fluxo completo: Ingestão Bronze ➡️ Qualidade e Limpeza Silver ➡️ Engenharia Gold ➡️ Validação de Hipóteses ➡️ Treinamento de Machine Learning.

---

## 👩‍💻 Autora
* **Aline Fiori Gonçalves**
* **Matrícula:** 4052025000106
