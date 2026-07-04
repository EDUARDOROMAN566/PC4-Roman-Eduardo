# PC4 — Control Estadístico de Procesos — Instrucciones para Claude Code

Este archivo se lee automáticamente al abrir esta carpeta con Claude Code.
Objetivo: resolver la PC4 completa (cálculo + Excel + HTML + prompts + declaración IA) y publicarla en GitHub, en un solo flujo, durante el examen.

Estudiante: Eduardo Roman (GitHub: EDUARDOROMAN566)
Curso: TE603 — Control Estadístico de Procesos, FIIS-UNI
Profesor: Mg. Marcel Darío Zárate Flores

**Uso de IA explícitamente permitido por el enunciado del examen** ("Puedes apoyarte en herramientas de inteligencia artificial... para verificar cálculos o redactar interpretaciones"), pero la nota evalúa el razonamiento — cada sección de interpretación debe justificar la decisión metodológica ("por qué esa carta y no otra"), no solo dar el resultado numérico.

## Estructura del examen (confirmada en 3 versiones distintas: Microsoft, JP Morgan, Anthropic)

El examen SIEMPRE trae 4 problemas con el mismo patrón, solo cambia el contexto de negocio y los números. 90–110 min, 20 pts:

| # | Arquetipo | Puntaje típico | Lo que pide |
|---|---|---|---|
| 1 | Carta de control por **variables** (subgrupos con varias mediciones, n=4 o 5) | 5 pts | Identificar X̄-R/X̄-S, resolver en Excel+Minitab, interpretar una señal (tendencia o pico puntual) |
| 2 | Carta de control por **atributos** (defectuosos/defectos por lote) | 5 pts | Identificar p/np/c/u según si n es fijo o variable, resolver, interpretar vs. meta de negocio |
| 3 | **Capacidad de proceso** con índice Taguchi | 6 pts | Calcular Cp, Cpl, Cpu, Cpk manualmente + Cpm, interpretar centrado/capaz, configurar Minitab |
| 4 | **Análisis crítico** — error metodológico de un "analista junior" | 4 pts | Detectar por qué la carta elegida por el personaje ficticio está mal, proponer la correcta |

Cuando llegue el enunciado real, primero mapea cada problema a su arquetipo (1–4) antes de calcular nada — el profesor reordena o cambia contexto pero el arquetipo se mantiene.

---

## 0. Antes de empezar (verificar, no preguntar)

- Verifica que `gh auth status` esté logueado. Si no lo está, DETENTE y pide al usuario correr `gh auth login` manualmente (no puede automatizarse en segundos).
- Verifica que existe `template/` en esta carpeta con `css/estilos.css` ya listo — reutilízalo tal cual, no rediseñes desde cero salvo que el usuario lo pida.
- Cronómetro mental: con 4 problemas y tiempo limitado, no dediques más de ~10 minutos a la parte de cálculo/HTML por problema; el resto es armado y push.

## 1. Leer el enunciado del examen

Para cada problema, extrae:
- Variable medida y unidad
- Tamaño de subgrupo o de lote (n) — y si **n es constante o variable** entre lotes (esto decide p vs np, c vs u)
- Los valores/datos
- Si son datos de **variables** (mediciones continuas) o **atributos** (conteo de defectuosos/defectos)
- Si hay LSL/USL/Target → es un problema de capacidad (arquetipo 3)
- Si el enunciado describe a un "analista junior" que ya tomó una decisión → es el arquetipo 4, buscar el error

## 2. ARQUETIPO 1 — Identificar la carta de variables

| Situación | Carta |
|---|---|
| Variable continua, n ≤ 10 | X̄-R |
| Variable continua, n > 10 | X̄-S |
| Variable continua, n = 1 (mediciones individuales genuinas) | I-MR |

## 2bis. ARQUETIPO 2 — Identificar la carta de atributos

| Situación | Carta |
|---|---|
| Conteo de **unidades defectuosas** (pasa/no pasa), n variable entre lotes | **p** |
| Conteo de **unidades defectuosas**, n constante entre lotes | **np** |
| Conteo de **defectos** (pueden ser varios por unidad), área/oportunidad de inspección constante | **c** |
| Conteo de **defectos**, área/oportunidad de inspección variable | **u** |

Distinción clave que el examen repite en el arquetipo 4: "defectuoso" (la unidad entera pasa o falla, se cuenta con np/p) vs. "defecto" (se pueden contar varias ocurrencias por unidad u oportunidad, se cuenta con c/u). Un conteo de fallas sobre una base de inspección fija de tamaño n (ej. "9 de 320 dispositivos fallaron") es una proporción de unidades defectuosas con n variable → **p**, nunca c ni I-MR.

## 3. Fórmulas de referencia — Carta de variables (X̄-R, el caso más común en este curso)

```
X̄ᵢ = promedio del subgrupo i
Rᵢ = max(subgrupo i) - min(subgrupo i)
X̿ = promedio de todos los X̄ᵢ
R̄ = promedio de todos los Rᵢ

UCL_X̄ = X̿ + A2·R̄        LCL_X̄ = X̿ - A2·R̄        CL_X̄ = X̿
UCL_R  = D4·R̄            LCL_R  = D3·R̄            CL_R  = R̄
```

Constantes por tamaño de subgrupo (tabla estándar Montgomery):

| n | A2 | D3 | D4 |
|---|-----|-----|-------|
| 2 | 1.880 | 0 | 3.267 |
| 3 | 1.023 | 0 | 2.574 |
| 4 | 0.729 | 0 | 2.282 |
| 5 | 0.577 | 0 | 2.114 |
| 6 | 0.483 | 0 | 2.004 |
| 7 | 0.419 | 0.076 | 1.924 |
| 8 | 0.373 | 0.136 | 1.864 |
| 9 | 0.337 | 0.184 | 1.816 |
| 10 | 0.308 | 0.223 | 1.777 |

Reglas de interpretación a chequear siempre:
1. ¿Algún punto fuera de UCL/LCL? → fuera de control.
2. Regla de rachas: ¿7+ puntos consecutivos subiendo, bajando, o del mismo lado del CL? → causa especial aunque estén dentro de límites.
3. Carta R primero, luego X̄ (si R está fuera de control, los límites de X̄ no son confiables todavía).

## 3bis. Fórmulas de referencia — Cartas de atributos (ARQUETIPO 2)

**Carta p (n variable entre lotes):**
```
p̄ = Σ(defectuosos_i) / Σ(n_i)
Para cada lote i:
  UCLᵢ = p̄ + 3·√(p̄(1-p̄)/nᵢ)      LCLᵢ = max(0, p̄ - 3·√(p̄(1-p̄)/nᵢ))      CL = p̄
```
Los límites varían por lote porque nᵢ varía — en Excel, calcula UCL/LCL como columna, no como valor único.

**Carta np (n constante entre lotes):**
```
np̄ = Σ(defectuosos_i) / k        (k = número de lotes)
p̄  = np̄ / n
UCL = np̄ + 3·√(np̄(1-p̄))        LCL = max(0, np̄ - 3·√(np̄(1-p̄)))       CL = np̄
```

**Carta c (conteo de defectos, área constante):**
```
c̄ = Σ(defectos_i) / k
UCL = c̄ + 3·√c̄        LCL = max(0, c̄ - 3·√c̄)        CL = c̄
```

**Carta u (conteo de defectos, área/n variable):**
```
ū = Σ(defectos_i) / Σ(nᵢ)
UCLᵢ = ū + 3·√(ū/nᵢ)        LCLᵢ = max(0, ū - 3·√(ū/nᵢ))        CL = ū
```

## 3ter. Fórmulas de referencia — Capacidad de proceso (ARQUETIPO 3)

Dados μ (media muestral X̄), σ (desviación estándar s), LSL, USL, y Target T:

```
Cp  = (USL - LSL) / (6σ)                    ← capacidad potencial, ignora centrado
Cpu = (USL - μ) / (3σ)                      ← margen hacia el límite superior
Cpl = (μ - LSL) / (3σ)                      ← margen hacia el límite inferior
Cpk = min(Cpu, Cpl)                         ← capacidad real considerando centrado
Cpm = (USL - LSL) / (6·√(σ² + (μ - T)²))    ← índice de Taguchi, penaliza desviarse del TARGET aunque esté dentro de spec
```

Criterios de interpretación estándar (úsalos en la sección de interpretación, no solo reportes el número):
- Cp ≈ Cpk → proceso centrado. Cp >> Cpk → proceso capaz en potencia pero descentrado.
- Cpk < 1.0 → proceso no capaz. 1.0–1.33 → capaz marginalmente. > 1.33 → capaz.
- Cpm < Cpk cuando μ ≠ T → el proceso puede "cumplir spec" (Cpk aceptable) pero estar lejos del target, lo cual Cpm penaliza — este es el punto que el examen pide explicar cuando el proceso está descentrado respecto a T pero dentro de LSL/USL.

**Minitab:** Stat > Quality Tools > Capability Analysis > Normal (o Assistant > Capability Analysis). En el cuadro de diálogo, el campo **"Target"** dentro de las opciones debe llenarse con T para que el reporte incluya Cpm automáticamente junto a Cp/Cpk.

## 3quater. ARQUETIPO 4 — Errores metodológicos típicos que pone el examen

Basado en los patrones vistos en las 3 versiones, el "error del analista junior" siempre es una de estas dos trampas:

1. **Usar I-MR para un conteo de atributos con n variable.** El personaje ve "un solo valor por lote/versión" y asume que es una medición individual continua → en realidad es un conteo de defectuosos/defectos sobre una base de n variable → la carta correcta es **p** (si es defectuosos) o **u** (si es defectos), cuyos límites varían con n, algo que I-MR no captura.
2. **Confundir c con np/p (o viceversa).** El personaje ve "son conteos" y usa c sin notar que hay un denominador fijo de "éxito/fracaso" (n constante) → la carta correcta es **np** (defectuosos, n fijo) — c es para defectos sin un denominador de pasa/no-pasa.

Para resolver el arquetipo 4: (a) nombra el error conceptual exacto (variable vs. atributo, o defecto vs. defectuoso, o ignorar n variable), (b) da la carta correcta con su fórmula de límites, (c) en una frase, qué patrón visual en esa carta señalaría un problema real (ej. "un punto fuera de UCL en una sucursal específica indica una causa especial local, no una fluctuación normal").

## 4. Generar el Excel (`excel/solucion.xlsx`) — una hoja por problema

- El examen trae 4 problemas → el workbook debe tener **una hoja por problema** (ej. "P1 Carta Variables", "P2 Carta Atributos", "P3 Capacidad", "P4 Análisis Crítico"), no todo apilado en una sola hoja.
- Usa **openpyxl** con **fórmulas de Excel reales** (`=AVERAGE(...)`, `=MAX()-MIN()`, `=SQRT(...)`), nunca valores calculados en Python y pegados como texto.
- Reutiliza el layout de estilo de `template/excel/` si existe (tabla de datos → parámetros → límites → gráfico de líneas con LCS/LC/LCI) para las hojas 1 y 2; para la hoja de capacidad (P3), usa un layout tabla-de-inputs → fórmulas de Cp/Cpl/Cpu/Cpk/Cpm en celdas separadas y visibles; para P4 basta una hoja con los datos, el cálculo de la carta correcta, y una celda de texto con la explicación del error.
- Después de guardar, ejecuta el recálculo con LibreOffice (`soffice --headless --convert-to xlsx --calculate` o el script de recálculo si está disponible) y confirma cero errores de fórmula antes de continuar.

## 5. Generar el sitio de entrega (`index.html`) — una sección por problema

- Copia `template/css/estilos.css` sin modificar el sistema de diseño (blueprint técnico: fondo `#0B1220`, acentos `#4AE3B5` ok / `#FF6B4A` alerta / `#F2A93B` ámbar, tipografía Space Grotesk + IBM Plex Mono + Inter).
- Estructura obligatoria a nivel de examen completo:
  1. Portada (contexto de empresa del examen, ej. "Microsoft", "JP Morgan", "Anthropic" — tómalo del enunciado)
  2. Nav con anclas a cada problema (P1–P4)
  3. **Una sección completa por problema**, cada una con: enunciado resumido → identificación de la carta/índice (con justificación explícita, no solo el nombre) → desarrollo en Excel (tabla + SVG del gráfico correspondiente) → desarrollo en Minitab (ruta exacta del Assistant) → interpretación en contexto de negocio
  4. Conclusiones generales (una síntesis de los 4 problemas, no repetir cada interpretación)
  5. Bibliografía e IA utilizada (una sola vez, al final)
- Los gráficos SVG van con coordenadas calculadas a partir de los valores reales de cada problema — nunca reuses coordenadas de un ejemplo anterior, recalcula la escala para los datos nuevos.
- Para P2 (atributos) y P3 (capacidad), si el gráfico es más simple (una sola línea con UCL/LCL constante o variable), no fuerces el mismo layout de dos-cartas de P1; ajusta el widget a lo que hay que mostrar.

## 6. Generar `prompts/prompts.md` y `declaracionIA.md`

- `prompts.md`: registra el/los prompts reales usados en la sesión (objetivo, respuesta de la IA, qué se modificó, reflexión personal). Deja la "reflexión personal" como campo que el usuario complete si el examen es cronometrado y no hay tiempo de reflexionar en el momento — pero avísale que debe llenarlo antes de la entrega final.
- `declaracionIA.md`: plantilla con % de apoyo de IA en blanco para que el usuario lo complete con honestidad — nunca inventes ese número.

## 7. Generar `README.md`

Resume: carta usada, diagnóstico (en control / fuera de control y por qué), y checklist de pendientes (Minitab real, completar prompts.md y declaracionIA.md).

## 8. Publicar en GitHub (el paso que debe ser rápido)

Ejecuta en la terminal, en este orden:

```bash
cd <carpeta-del-proyecto>
git init -q
git add .
git commit -q -m "PC4: entrega completa (4 problemas)"
gh repo create PC4-<Apellido>-<Nombre> --public --source=. --remote=origin --push
gh api -X PUT repos/EDUARDOROMAN566/PC4-<Apellido>-<Nombre>/pages \
  -f "source[branch]=main" -f "source[path]=/" 2>/dev/null || \
gh repo edit --enable-pages 2>/dev/null
echo "Repo: https://github.com/EDUARDOROMAN566/PC4-<Apellido>-<Nombre>"
echo "Sitio: https://eduardoroman566.github.io/PC4-<Apellido>-<Nombre>/"
```

Nota: `gh api ... /pages` puede fallar si Pages no está habilitado por API en la cuenta — en ese caso, dile al usuario que confirme manualmente en **Settings → Pages → Source: main /(root)** una sola vez (toma 10 segundos), porque la API de Pages a veces requiere ese primer toggle manual.

Reemplaza `<Apellido>-<Nombre>` por el nombre real del examen (ej. `PC4-Roman-Eduardo`), no un placeholder.

## 9. Verificación final antes de avisar "listo"

- [ ] `solucion.xlsx` sin errores de fórmula
- [ ] `index.html` abre bien localmente (ruta relativa a `css/estilos.css`)
- [ ] Push a GitHub confirmado (`git log` muestra el commit, `gh repo view` no da error)
- [ ] Link de GitHub Pages entregado al usuario
- [ ] Avisar explícitamente qué quedó pendiente de completar a mano (Minitab real, reflexión en prompts.md, % en declaracionIA.md)

No te detengas a pedir confirmación entre pasos 1-9 salvo que el enunciado sea ambiguo (tipo de carta no está claro) o `gh auth status` no esté logueado — en examen cronometrado, ejecuta todo el flujo de corrido y reporta al final.
