# 💳 Fraud-Detection-XGBoost: Modelo de Detecção de Fraudes em Cartões de Crédito

## 📄 Descrição do Projeto

Este projeto consiste no desenvolvimento e otimização de um modelo de **Machine Learning** para a detecção de **fraudes em transações de cartão de crédito**. O principal desafio abordado foi o tratamento de uma base de dados extremamente **desbalanceada**, onde as transações fraudulentas (classe positiva) representam uma parcela ínfima do total.

O foco do projeto é atingir um **alto Recall** (taxa de captura de fraudes) para minimizar a quantidade de fraudes perdidas (Falsos Negativos), mesmo que isso implique em uma taxa maior de Falsos Positivos (alertas falsos).

---

## 🔬 Metodologia e Abordagem

O notebook `entrega_bank_43.ipynb` documenta todo o processo, que incluiu:

### Pré-processamento e Base de Dados
* A base de dados utilizada já estava pré-processada, tendo sido aplicada a **Análise de Componentes Principais (PCA)** para anonimizar as variáveis, conforme o desafio de negócio.
* Dada a natureza do dataset, não foi necessária Análise Exploratória de Dados (EDA) ou visualização gráfica prévia.
* A separação dos dados de treino e teste foi realizada de forma **estratificada** para garantir que a proporção de fraudes fosse mantida em ambos os conjuntos.

### Modelagem e Otimização
* Foi escolhido o algoritmo **XGBoost (Extreme Gradient Boosting)** para a modelagem devido à sua eficiência e robustez em bases de dados desbalanceadas.
* **Tratamento de Desbalanceamento:** Utilizou-se o hiperparâmetro `scale_pos_weight` no `XGBClassifier` para atribuir um peso elevado à classe minoritária (fraude). O valor calculado e aplicado foi de aproximadamente **577.29**, que representa a razão entre a classe majoritária e a minoritária.
* **Ajuste Fino de Hiperparâmetros:** Após uma avaliação inicial, os hiperparâmetros do modelo foram ajustados para favorecer o desempenho:
    * `learning_rate`: reduzido para **0.01**.
    * `max_depth`: reduzido para **3**.
    * `n_estimators`: definido como **200**.
* **Otimização do Threshold:** O **threshold de decisão** do modelo foi otimizado de 0.5 para **0.3**, com base na Curva Precision-Recall, para aumentar ainda mais a sensibilidade (Recall) do modelo na detecção de fraudes.

---

## 🛠️ Tecnologias e Dependências

O projeto foi desenvolvido em **Python** no ambiente Jupyter Notebook e requer as seguintes bibliotecas:

* `pandas`
* `numpy`
* `scikit-learn` (para divisão, métricas e curva Precision-Recall)
* `xgboost` (`XGBClassifier`)
* `matplotlib` (para visualização da curva)

---

## 📊 Resultados Chave do Modelo Final

O modelo final, utilizando o **Threshold de Compromisso (0.3)** e a otimização dos hiperparâmetros, atingiu o objetivo de negócio com as seguintes métricas de desempenho no conjunto de teste:

| Métrica | Valor | Objetivo de Negócio |
| :--- | :--- | :--- |
| **Recall (Captura de Fraudes)** | **0.9286** (92,86%) | **Prioridade Máxima:** Garantir que o mínimo de fraudes seja perdido (Falsos Negativos). |
| **Precision (Acurácia das Previsões)** | 0.0258 (2,58%) | Aceitável, pois o custo de um Falso Positivo (alerta falso) é menor que o custo de uma Fraude Perdida (Falso Negativo). |
| **AUC-ROC** | *Alta* | Indica excelente separabilidade geral das classes. |

### Matriz de Confusão (Threshold 0.3)

| Previsão | Não-Fraude (0) | Fraude (1) |
| :--- | :--- | :--- |
| **Real Não-Fraude (0)** | 53424 (Verdadeiros Negativos) | 3440 (Falsos Positivos) |
| **Real Fraude (1)** | **7** (Falsos Negativos) | 91 (Verdadeiros Positivos) |

**Conclusão:** O modelo demonstrou alta eficácia, resultando em apenas **7 fraudes perdidas (Falsos Negativos)** no conjunto de teste, garantindo um Recall de 92,86%. A estratégia de ajustar o threshold e o peso de classe foi crucial para este resultado.

---
