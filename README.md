# PC4 — Control Estadístico de Procesos (Versión 6, GOOGLE)

Entrega de la Práctica Calificada N°4, TE603, FIIS-UNI.

## Resumen por problema

| # | Tema | Carta/índice | Diagnóstico |
|---|---|---|---|
| P1 | Discos fallidos por rack (n=240 constante) | np | En control (np̄=4.30, UCL=10.46, LCL=0) |
| P2 | Tiempo de respuesta de servidor | Capacidad + Taguchi | Cpk=0.833 (no capaz), Cpm=0.892 — cumple SLA "por poco" |
| P3 | Temperatura pasillo frío (n=4) | X̄-R | Fuera de control — tendencia sostenida al alza (21.95°C → 23.15°C) |
| P4 | Análisis conceptual | Cpk vs. control / Cp-Cpk vs. Pp-Ppk / c vs. I-MR | Conceptual, sin datos a graficar |

## Estructura

- `index.html` — sitio de entrega completo (abrir con `template/css/estilos.css` en la misma carpeta relativa)
- `excel/solucion.xlsx` — 4 hojas con fórmulas de Excel reales, recalculado con LibreOffice sin errores
- `prompts/prompts.md` — registro de prompts usados
- `declaracionIA.md` — declaración de uso de IA (plantilla)

## Pendiente antes de la entrega final

- [ ] Ejecutar las cartas y el análisis de capacidad en **Minitab real** y adjuntar capturas (rutas de menú ya documentadas en `index.html`)
- [ ] Completar la sección "reflexión personal" en `prompts/prompts.md`
- [ ] Completar el porcentaje de apoyo de IA en `declaracionIA.md`
- [ ] Habilitar GitHub Pages manualmente si la API no lo activó automáticamente (Settings → Pages → Source: main /(root))
