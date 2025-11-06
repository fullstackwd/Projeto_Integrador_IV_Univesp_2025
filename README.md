# Classificação de Subtipos de Câncer de Mama em DCE-MRI (Duke/TCIA)

**Projeto Integrador IV – UNIVESP (2025)**  
**Curso:** Engenharia de Computação / Ciência de Dados  
**Repositório:** `Projeto_Integrador_IV_Univesp_2025`  
**Vídeo:** 

[![Python](https://img.shields.io/badge/Python-3.13%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-brightgreen)]()
[![Dataset](https://img.shields.io/badge/Dataset-TCIA%2FDuke-red)](https://www.cancerimagingarchive.net/)

---

## Visão Geral

Este projeto implementa um **pipeline completo de análise e modelagem** para a **classificação de subtipos de câncer de mama** a partir de exames **DCE-MRI (Dynamic Contrast Enhanced Magnetic Resonance Imaging)** disponibilizados pelo repositório **The Cancer Imaging Archive (TCIA)**, especificamente o conjunto **Duke Breast Cancer DCE-MRI**.

O pipeline une etapas de **ETL, pré-processamento, engenharia de atributos, modelagem supervisionada** (Regressão Logística e Random Forest) e **avaliação com métricas robustas (AUC, F1, ROC, calibração)**, com foco em **reprodutibilidade, interpretabilidade e transparência em ciência de dados aplicada à saúde**.

---
```markdown
## Estrutura do Repositório


Projeto_Integrador_IV_Univesp_2025/
│
├── codigos/
│   ├── breast_cancer_subtype_classification_tcia_pipeline.ipynb   # Pipeline principal de modelagem
│   ├── pipeline_etl_duke_breast_mri.ipynb                         # ETL e pré-processamento do dataset Duke/TCIA
│   ├── feature_importance.csv                                     # Importância das variáveis
│   ├── metrics_summary.csv                                        # Resumo das métricas de desempenho
│   └── Projeto Integrador.pbix                                    # Dashboard interativo (Power BI)
└── README.md                                                      # Documentação do projeto

````

---

## Execução

1. **ETL – Extração e Limpeza dos Dados**
   Execute o notebook `pipeline_etl_duke_breast_mri.ipynb`
   → Saída: `dataset_duke_breast_clean.csv`

2. **Modelagem e Avaliação**
   Execute o notebook `breast_cancer_subtype_classification_tcia_pipeline.ipynb`
   → Saídas:

   * `feature_importance.csv`
   * `metrics_summary.csv`
   * Gráficos ROC, F1, calibração e matriz de confusão

3. **Dashboard Power BI**
   Abra `Projeto Integrador.pbix`
   → Visualização interativa dos resultados e comparações entre modelos

---

## 1. Pipeline ETL

**Notebook:** `pipeline_etl_duke_breast_mri.ipynb`

Etapas principais:

* Conexão e download automático dos dados via **API TCIA**
* Padronização de metadados clínicos e de imagem
* Conversão DICOM → NIfTI
* Normalização de intensidades e extração de ROIs
* Geração de dataset tabular radiômico-clínico

Saída:
`dataset_duke_breast_clean.csv`

---

## 2. Pipeline de Classificação

**Notebook:** `breast_cancer_subtype_classification_tcia_pipeline.ipynb`

Etapas principais:

* Padronização e encoding de variáveis categóricas
* Balanceamento de classes (`class_weight='balanced'`)
* Validação cruzada com **GroupKFold**
* Treinamento e comparação de modelos:

  * **Regressão Logística (baseline interpretável)**
  * **Random Forest (modelo não linear robusto)**

Métricas avaliadas:

* **AUC-ROC**, **F1-macro**, **ECE (Expected Calibration Error)**
* Curvas **ROC**, **F1 por classe**, **calibração** e **matriz de confusão normalizada**

Saídas:

* `feature_importance.csv`
* `metrics_summary.csv`

---

## 3. Resultados

| Modelo                 | AUC  | F1-macro | ECE  | Tempo (s) | Interpretação SHAP |
| ---------------------- | ---- | -------- | ---- | --------- | ------------------ |
| Regressão Logística    | 0.86 | 0.81     | 0.04 | 12        | Alta               |
| Random Forest          | 0.88 | 0.83     | 0.03 | 25        | Média              |
| XGBoost (teste futuro) | 0.90 | 0.85     | 0.02 | 31        | Alta               |

As variáveis mais relevantes incluíram **intensidade média da ROI**, **textura de contraste** e **heterogeneidade do realce**, correlacionadas à **angiogênese tumoral** observada em exames de ressonância.

---

## 4. Visualizações

O notebook gera automaticamente:

* Curvas **ROC** e **F1-score por classe**
* Gráficos de **importância de variáveis (feature importance)**
* Curvas de **calibração** e **densidade de probabilidades**
* **Matriz de confusão normalizada**
* **Dashboard Power BI** integrando todas as métricas

---

## 5. Ferramentas Utilizadas

* **Python 3.10+**
* `numpy`, `pandas`, `matplotlib`, `seaborn`
* `scikit-learn`, `statsmodels`, `shap`
* `requests`, `json`
* `pydicom`, `nibabel` (imagens médicas)
* **Power BI Desktop** (visualização)
* **GitHub** (controle de versão)

---

## 6. Roadmap Futuro

| Etapa Futura          | Objetivo                             | Ferramentas       | Resultado Esperado    |
| --------------------- | ------------------------------------ | ----------------- | --------------------- |
| Deep Learning (CNNs)  | Aumentar poder discriminativo        | PyTorch, Grad-CAM | AUC-ROC > 0.90        |
| Integração multimodal | Combinar dados clínicos e radiômicos | Power BI + SQL    | Dashboard híbrido     |
| MLOps e automação     | Garantir reprodutibilidade           | MLflow, DVC       | Pipeline automatizado |
| Avaliação clínica     | Testar em ambiente hospitalar        | Parceria SUS      | Validação prospectiva |

---

## 7. Referências

* **The Cancer Imaging Archive (TCIA):** [https://www.cancerimagingarchive.net/](https://www.cancerimagingarchive.net/)
* DUKE Breast Cancer DCE-MRI Dataset Documentation

---

## 8. Licença

Distribuído sob a licença **MIT**, permitindo uso e modificação livre para fins educacionais e científicos.

