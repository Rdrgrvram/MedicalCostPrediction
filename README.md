# Medical Cost Prediction

## Objetivo del caso

Predecir el costo médico individual (`charges`) a partir de variables demográficas y de salud (edad, sexo, IMC, número de hijos, hábito de fumar y región), aplicando un pipeline de preprocesamiento y modelado supervisado (regresión). El objetivo es identificar los factores con mayor impacto en el costo y entregar un modelo con buen desempeño predictivo e interpretable para apoyar decisiones (p. ej., estimación de primas).

> Ajusta este párrafo si tu enfoque, target o dataset son distintos.

## Dataset

- **Nombre:** Medical Cost Personal Datasets (insurance.csv)
- **Página Kaggle:** https://www.kaggle.com/datasets/mirichoi0218/insurance
- **Descarga vía API de Kaggle:** https://www.kaggle.com/api/v1/datasets/download/mirichoi0218/insurance
- **Variables:** age, sex, bmi, children, smoker, region, charges
- **Filas:** 1338

El archivo ya está incluido en este repositorio en `data/insurance.csv` (52 KB), para que el notebook corra de inicio a fin sin pasos manuales, incluyendo en Google Colab. Si prefieres descargarlo tú mismo, usa cualquiera de los enlaces de arriba y reemplaza el archivo en la misma ruta.

## Estructura del repositorio

```
.
├── data/              # insurance.csv (dataset incluido para reproducibilidad)
├── notebooks/         # Notebook final (.ipynb) con el pipeline completo
├── report/            # Informe en PDF (3-6 páginas)
├── slides/            # Presentación (PPTX)
├── requirements.txt   # Dependencias para reproducir el entorno
└── README.md
```


## Instrucciones para reproducir

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/TU_USUARIO/MedicalCostPrediction.git
   cd MedicalCostPrediction
   ```
2. Crear y activar un entorno virtual:
   ```bash
   python -m venv venv
   venv\Scripts\activate      # Windows
   # source venv/bin/activate  # Linux/Mac
   ```
3. Instalar dependencias:
   ```bash
   pip install -r requirements.txt
   ```
4. El dataset (`data/insurance.csv`) ya viene incluido en el repositorio — no hace falta descargarlo aparte.
5. Ejecutar el notebook:
   ```bash
   jupyter notebook notebooks/MedicalCostPrediction_Model.ipynb
   ```

**En Google Colab:** el notebook no encuentra `data/insurance.csv` si solo subes el archivo `.ipynb` suelto — necesita el repositorio completo. En una celda, antes de correr el resto del notebook:
```python
!git clone https://github.com/TU_USUARIO/MedicalCostPrediction.git
%cd MedicalCostPrediction/notebooks
```
Así el notebook encuentra el dataset en `../data/insurance.csv`, igual que al correrlo localmente.

## Resultados principales

Modelo de regresión final: Polinomios grado 2 + Ridge (alpha=1.0, seleccionado por validación cruzada). Evaluado sobre el 20% de prueba (268 observaciones):

| Métrica | Valor |
|---|---|
| MAE  | 2,689.80 |
| RMSE | 4,591.33 |
| R²   | 0.8642 |

Modelo de clasificación para imputar `smoker`: regresión logística, ROC-AUC = 0.5267 en validación (apenas por encima del azar).

**Principales hallazgos:**
- El término de interacción `bmi × smoker` es el de mayor peso sobre el costo predicho: el efecto del IMC en el gasto es mucho mayor en fumadores que en no fumadores.
- La ganancia del modelo polinomial sobre una regresión lineal simple es modesta (MAE similar, RMSE prácticamente igual), por lo que el modelo lineal sigue siendo una alternativa razonable si se prioriza interpretabilidad.
- Las variables demográficas disponibles (age, sex, bmi, children, region) tienen poder predictivo muy limitado para recuperar `smoker` (ROC-AUC ≈ 0.53); estas imputaciones no deben tratarse como dato observado.

Detalle completo de metodología, tablas y limitaciones en `report/Informe_MedicalCostPrediction.pdf`.

## Autor

Rodrigo Rivera — rodrigo.rivera@ucb.edu.bo
