# Prompts utilizados — PC4 (Versión 6, contexto GOOGLE)

## Prompt 1 — Mapeo de arquetipos
**Objetivo:** Identificar qué tipo de carta/índice corresponde a cada uno de los 4 problemas del examen antes de calcular.
**Respuesta de la IA:** P1 → carta np (n=240 constante, conteo de unidades defectuosas); P2 → capacidad de proceso con índice Taguchi (Cp/Cpl/Cpu/Cpk/Cpm); P3 → carta X̄-R (n=4, variable continua); P4 → conceptual (Cpk vs. control estadístico, Cp/Cpk vs. Pp/Ppk, carta c vs. I-MR).
**Qué se modificó:** Ninguno de los mapeos requirió corrección — coincide con el patrón fijo documentado en CLAUDE.md.
**Reflexión personal:** _[completar antes de la entrega final]_

## Prompt 2 — Cálculo y verificación de fórmulas
**Objetivo:** Generar el archivo Excel con fórmulas reales (no valores pegados) para las 4 hojas, y verificar que no haya errores de fórmula tras el recálculo.
**Respuesta de la IA:** Script en Python (openpyxl) que construye las 4 hojas con fórmulas de Excel nativas (PROMEDIO, RAIZ, MIN, etc.), recalculado con LibreOffice headless para confirmar cero errores (#DIV/0!, #VALUE!, etc.).
**Qué se modificó:** Se corrigió un error de sintaxis (celda `D3ignore` inválida) detectado en la primera ejecución.
**Reflexión personal:** _[completar antes de la entrega final]_

## Prompt 3 — Interpretación de negocio
**Objetivo:** Redactar las interpretaciones de cada problema en el contexto de negocio de Google (data centers, SLA de Search, Trust & Safety, RAID).
**Respuesta de la IA:** Texto que conecta cada resultado estadístico con una decisión de ingeniería/negocio concreta (ej. Cpk<1.0 a escala de miles de millones de consultas, tendencia de temperatura como riesgo para hardware).
**Qué se modificó:** _[completar]_
**Reflexión personal:** _[completar antes de la entrega final — justificar en tus propias palabras por qué cada carta/índice es la correcta, como pide el examen]_

---

**IMPORTANTE:** Completa los campos de "reflexión personal" antes de la entrega — el examen pide que defiendas cada decisión metodológica como razonamiento propio, no solo el resultado numérico generado por IA.
