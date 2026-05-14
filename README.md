# 🔍 Fraud Detection Project

> Detección de fraude mediante Machine Learning aplicado a **transacciones bancarias** y **reclamaciones de seguros**.

---

## 📌 Descripción

Este proyecto implementa un pipeline completo de detección de fraude con datos sintéticos , cubriendo dos dominios de alto impacto:

| Dominio | Tasa de Fraude Simulada | Registros |
|---|---|---|
| Transacciones Bancarias / Tarjetas | ~1.5 % | 50,000 |
| Reclamaciones de Seguros | ~12 % | 30,000 |

El flujo abarca desde la **generación y exploración de datos**, pasando por **feature engineering**, hasta el **entrenamiento, evaluación y comparación de modelos**.

---

## 🏗️ Estructura del Proyecto

```
fraud-detection-project/
│
├── src/
│   ├── fraud_detection_banking.py    # Módulo transacciones bancarias
│   └── fraud_detection_insurance.py  # Módulo reclamaciones de seguros
│
├── reports/
     └── figures/                      # Gráficos generados automáticamente
```

---

## 🤖 Modelos Implementados

Se entrena y compara una **triada de modelos** por dominio:

| Modelo | Ventaja Principal |
|---|---|
| `Logistic Regression` | Baseline interpretable, rápido |
| `Random Forest` | Robusto, maneja bien el desbalanceo |
| `Gradient Boosting` | Mayor poder predictivo, mejor AUC |

### Técnicas para datos desbalanceados
- **Oversampling** de la clase minoritaria (SMOTE-like via `resample`)
- `class_weight="balanced"` en modelos lineales y Random Forest

---

## 📊 Features Clave

### 🏦 Bancario
| Feature | Descripción |
|---|---|
| `amount_log` | Monto de transacción (escala log) |
| `is_night` | Transacción entre 22:00–06:00 |
| `is_foreign` | Transacción en país extranjero |
| `high_velocity` | ≥3 transacciones en la última hora |
| `new_card` | Tarjeta emitida hace menos de 30 días |
| `distance_log` | Distancia desde ubicación habitual (log) |

### 🛡️ Seguros
| Feature | Descripción |
|---|---|
| `amount_log` | Monto de reclamación (escala log) |
| `low_policy_age` | Póliza con menos de 1 año de antigüedad |
| `high_prev_claims` | ≥3 reclamaciones previas |
| `late_report` | Reporte tardío (>15 días post-evento) |
| `no_witnesses` | Sin testigos registrados |
| `police_report` | Si existe reporte policial |

---

## 📈 Métricas de Evaluación

Dado el fuerte desbalanceo de clases, se priorizan:

- **ROC-AUC** — Discriminación general del modelo
- **Average Precision (AP)** — Área bajo la curva Precision-Recall
- **F1-Score** — Balance precisión/recall sobre la clase fraude

---

## 🚀 Instalación y Uso

### 1. Clonar el repositorio
```bash
git clone https://github.com/tu-usuario/fraud-detection-project.git
cd fraud-detection-project
```

### 2. Crear entorno virtual
```bash
python -m venv venv
source venv/bin/activate        # Linux / macOS
venv\Scripts\activate           # Windows
```

### 3. Instalar dependencias
```bash
pip install -r requirements.txt
```

### 4. Ejecutar el pipeline completo
```bash
python run_all.py
```

O ejecutar cada módulo por separado:
```bash
python -m src.fraud_detection_banking
python -m src.fraud_detection_insurance
```

---

## 📷 Visualizaciones Generadas

Tras ejecutar el pipeline, encontrarás en `reports/figures/`:

| Archivo | Descripción |
|---|---|
| `banking_class_dist.png` | Distribución legítimo vs fraude (bancario) |
| `banking_roc_curves.png` | Curvas ROC — 3 modelos bancarios |
| `banking_pr_curves.png` | Curvas Precision-Recall (bancario) |
| `banking_confusion_matrix.png` | Matriz de confusión del mejor modelo |
| `banking_feature_importance.png` | Importancia de variables (bancario) |
| `insurance_claim_dist.png` | Distribución de montos y reclamaciones previas |
| `insurance_roc_curves.png` | Curvas ROC — 3 modelos de seguros |
| `insurance_feature_importance.png` | Importancia de variables (seguros) |
| `insurance_confusion_matrix.png` | Matriz de confusión del mejor modelo |

---

## 🧠 Decisiones de Diseño

**¿Por qué datos sintéticos?**
Permiten controlar exactamente las distribuciones de fraude y simular patrones reales (alta actividad nocturna, transacciones internacionales, etc.) sin comprometer datos sensibles reales.

**¿Por qué no usar SMOTE de imbalanced-learn?**
Para mantener las dependencias mínimas y la reproducibilidad. El oversampling manual mediante `resample` de sklearn produce resultados equivalentes para este caso de uso.

**¿Por qué Gradient Boosting y no XGBoost/LightGBM?**
`GradientBoostingClassifier` de sklearn es suficiente para demostrar el concepto sin dependencias adicionales. En producción, LightGBM es recomendable por velocidad.

---

## 🔮 Próximos Pasos

- [ ] Integrar SHAP para explicabilidad de predicciones individuales
- [ ] Añadir Isolation Forest para detección de anomalías no supervisada
- [ ] Dashboard interactivo con Streamlit
- [ ] Exportar modelos serializados con joblib
- [ ] Pipeline de inferencia en tiempo real (FastAPI)

---

## 📚 Referencias

- [Credit Card Fraud Detection Dataset — Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud)
- [Imbalanced-learn documentation](https://imbalanced-learn.org/)
- [Scikit-learn — Ensemble Methods](https://scikit-learn.org/stable/modules/ensemble.html)
- Bahnsen, A. C. et al. (2016). *Feature engineering strategies for credit card fraud detection.* Expert Systems with Applications.

---

## 📝 Licencia

MIT License — libre uso, modificación y distribución con atribución.

---

<p align="center">
  Desarrollado con Python 🐍 · scikit-learn · pandas · matplotlib
</p>
