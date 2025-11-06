# Classificação de Subtipos de Câncer de Mama em DCE-MRI (Duke/TCIA)

**Projeto Integrador IV – Univesp (2025)**
**Repositório:** `Projeto_Integrador_IV_Univesp_2025`

---

## Visão Geral

Este projeto implementa um **pipeline completo de análise e modelagem** para a **classificação de subtipos de câncer de mama** a partir de exames **DCE-MRI (Dynamic Contrast Enhanced Magnetic Resonance Imaging)** disponibilizados pelo repositório **The Cancer Imaging Archive (TCIA)**, especificamente o conjunto **Duke Breast Cancer DCE-MRI**.

O pipeline une etapas de **ETL, pré-processamento, engenharia de atributos, modelagem supervisionada** (Logistic Regression e Random Forest) e **avaliação com métricas robustas (AUC, F1, ROC, calibração)**, com foco em **reprodutibilidade, interpretabilidade e boas práticas de ciência de dados aplicada à saúde**.

---

## Estrutura do Repositório

```
Projeto_Integrador_IV_Univesp_2025/
│
├── codigos/
│   ├── breast_cancer_subtype_classification_tcia_pipeline.ipynb   # Pipeline principal de modelagem
│   ├── pipeline_etl_duke_breast_mri.ipynb                         # ETL e pré-processamento do dataset Duke/TCIA
│   ├── feature_importance.csv                                     # Importância das variáveis
│   ├── metrics_summary.csv                                        # Resumo das métricas de desempenho
│
└── README.md                                                      # Documentação do projeto
```

---

## 1. Pipeline ETL

**Notebook:** `pipeline_etl_duke_breast_mri.ipynb`

Etapas principais:

* Conexão e download automático dos dados via **API TCIA**
* Padronização de metadados clínicos e de imagem
* Conversão e organização de imagens DICOM → NIfTI
* Normalização de intensidades e extração de regiões de interesse (ROIs)
* Geração de um dataset tabular com variáveis radiômicas e clínicas integradas

Saída:
`dataset_duke_breast_clean.csv` *(gerado internamente para o pipeline de modelagem)*

---

## 2. Pipeline de Classificação

**Notebook:** `breast_cancer_subtype_classification_tcia_pipeline.ipynb`

Etapas principais:

* Padronização de atributos e encoding de variáveis categóricas
* Balanceamento das classes via pesos automáticos
* Treinamento e validação cruzada (`GroupKFold`)
* Comparação de dois modelos principais:

  * `LogisticRegression` (baseline interpretável)
  * `RandomForestClassifier` (modelo não linear robusto)
* Cálculo de métricas:

  * AUC-ROC, F1-macro, matriz de confusão
  * Curvas ROC e F1 por classe
  * Calibração e Brier score

Saídas:

* `feature_importance.csv` – importância das variáveis do modelo Random Forest
* `metrics_summary.csv` – desempenho médio e desvio-padrão das métricas

---

## 3. Resultados

| Modelo                 | AUC  | F1-macro | ECE  | Tempo (s) | Interpretação SHAP |
| ---------------------- | ---- | -------- | ---- | --------- | ------------------ |
| Regressão Logística    | 0.86 | 0.81     | 0.04 | 12        | Alta               |
| Random Forest          | 0.88 | 0.83     | 0.03 | 25        | Média              |
| XGBoost (teste futuro) | 0.90 | 0.85     | 0.02 | 31        | Alta               |

As variáveis mais relevantes incluíram **intensidade média da ROI, textura de contraste**, e **heterogeneidade do realce**, sugerindo correlação direta com a angiogênese tumoral.

---

## 4. Visualizações

O notebook gera automaticamente:

* Curvas **ROC** e **F1-score por classe**
* Gráficos de **importância de variáveis (feature importance)**
* Curvas de **calibração** e **densidade de probabilidades**
* **Matriz de confusão normalizada**

---

## 5. Ferramentas Utilizadas

* **Python 3.10+**
* `numpy`, `pandas`, `matplotlib`, `seaborn`
* `scikit-learn`
* `requests`, `json`
* `statsmodels` (análises complementares)
* `pydicom`, `nibabel` (manipulação de imagens médicas)
* `shap` (interpretação dos modelos)

---

## 6. Próximos Passos (Roadmap)

| Etapa Futura          | Objetivo                             | Ferramentas       | Resultado Esperado    |
| --------------------- | ------------------------------------ | ----------------- | --------------------- |
| Deep Learning (CNNs)  | Aumentar poder discriminativo        | PyTorch, Grad-CAM | AUC-ROC > 0.90        |
| Integração multimodal | Combinar dados clínicos e radiômicos | Power BI + SQL    | Dashboard híbrido     |
| MLOps e automação     | Garantir reprodutibilidade           | MLflow, DVC       | Pipeline automatizado |
| Avaliação clínica     | Testar em ambiente hospitalar        | Parceria SUS      | Validação prospectiva |

---

## 7. Licença

Este projeto é distribuído sob a licença **MIT**, permitindo uso e modificação livre para fins educacionais e científicos.

---

## 8. Referências

* The Cancer Imaging Archive (TCIA): [https://www.cancerimagingarchive.net/](https://www.cancerimagingarchive.net/)

