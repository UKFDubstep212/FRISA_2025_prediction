# Portafolio de Proyectos Académicos

**Hugo Edel Gamboa Sesma — A00841605**

**Enrique Alexander Luna Sánchez - A00574602**

**Alejandro Israel Manducano Rojo - A00841759**

Tecnológico de Monterrey · 2025–2026

Tres proyectos aplicados en manufactura, humanitarismo y física computacional, desarrollados en colaboración con organizaciones reales.

---

## 1. Reducción de Desperdicios en Manufactura de Anillos Rolados

**Área:** Machine Learning interpretable
**Socio industrial:** FRISA
**Curso:** Análisis de Ciencia de Datos (2026)

FRISA estimaba perder alrededor del 40% del material adquirido en excesos innecesarios durante la manufactura de anillos rolados. Se desarrollaron modelos de ML para identificar piezas candidatas a reducción de exceso volumétrico sin incrementar el riesgo de defectos.

El análisis exploratorio reveló un sesgo sistemático en el algoritmo de configuración de excesos de FRISA —no limitado a familias específicas de piezas— y que el exceso adicional no reduce los defectos, sino que se asocia con tipos de falla distintos (pozos, descascarados, traslapes).

**Modelo principal:** Árbol de Decisión con selección de variables por RFE.

| Métrica | Valor |
|---|---|
| Precisión (cross-validation) | 93% |
| AUC-ROC | 0.85 |
| Material recuperable estimado | ~125 toneladas métricas |

**Mi contribución:** Ingeniería de variables de exceso volumétrico, definición del umbral de clasificación, selección por RFE, evaluación con cross-validation estratificada e interpretación operativa de resultados.

**Stack:** Python · scikit-learn · pandas · NumPy · XGBoost · dtreeviz · Google Colab

---
*Tecnológico de Monterrey — Ingeniería y Ciencias*
