# Medical Cost Prediction

## Objetivo del caso

Predecir el costo médico individual (`charges`) a partir de variables demográficas y de salud (edad, sexo, IMC, número de hijos, hábito de fumar y región), aplicando un pipeline de preprocesamiento y modelado supervisado (regresión). El objetivo es identificar los factores con mayor impacto en el costo y entregar un modelo con buen desempeño predictivo e interpretable para apoyar decisiones (p. ej., estimación de primas).

> Ajusta este párrafo si tu enfoque, target o dataset son distintos.

## Dataset

- **Nombre:** Medical Cost Personal Datasets (insurance.csv)
- **Descarga:** https://www.kaggle.com/datasets/mirichoi0218/insurance
- **Variables:** age, sex, bmi, children, smoker, region, charges
- **Filas:** ~1338

> Reemplaza el link si usaste otra fuente o versión del dataset.

## Estructura del repositorio

```
.
├── notebooks/        # Notebook final (.ipynb) con el pipeline completo
├── report/           # Informe en PDF (3-6 páginas)
├── slides/           # Presentación (PPTX o PDF)
├── src/              # (Opcional) funciones y utilidades reutilizables
├── requirements.txt  # Dependencias para reproducir el entorno
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
4. Descargar el dataset desde el link indicado arriba y colocarlo en una carpeta `data/` en la raíz del proyecto (no versionada en Git).
5. Ejecutar el notebook:
   ```bash
   jupyter notebook notebooks/MedicalCostPrediction_Model.ipynb
   ```

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

## Autores
Mauricio Orlando Rivera Mayan 
Rodrigo Gabriel Rivera Mayan 
Laura Yackelin Perez Estrada 

