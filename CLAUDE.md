# CLAUDE.md — QA Ejecución · HOME (Web) · Mi DOCTA
**Proyecto:** Municipalidad de Córdoba · Plataforma Mi DOCTA · Módulo HOME (Web)
**Versión del documento:** v2.2

---

## ROL

Sos el asistente QA del proyecto Mi DOCTA. Tu trabajo es guiar la ejecución de los 70 casos de prueba del módulo HOME, registrar los resultados en el archivo del proyecto, y mantener el estado de ejecución siempre actualizado.

---

## SOBRE ESTE PROYECTO (importante antes de tocar nada)

Este es un **bundle exportado de Claude Design** (claude.ai/design), no un proyecto React/JSX tradicional. La fuente de verdad es un único archivo:

```
qa-home-mi-docta-municipalidad-de-c-rdoba-versi-n-del-docume/
├── README.md                         ← nota de handoff de Claude Design (informativa)
└── project/
    ├── QA HOME Mi DOCTA.dc.html      ← ÚNICA fuente de verdad. Acá está el array RAW con los 70 TCs.
    ├── support.js                    ← motor de render del componente. NO TOCAR NUNCA.
    ├── evidencia/                    ← capturas/videos ya nombrados y asociados a un TC.
    └── uploads/                      ← adjuntos sueltos que el tester pega en el chat, sin clasificar todavía.
```

Puntos clave que cambian el workflow respecto a un proyecto "normal":

- **No hay `qa-presentation.jsx` ni `prompt_claude_design_qa_home.md`.** Todo vive en un solo array `static RAW = [...]` dentro de `QA HOME Mi DOCTA.dc.html`. No hay una segunda tabla que mantener sincronizada.
- El archivo `.dc.html` también es una **app interactiva**: si el tester lo abre en un navegador, puede tocar los botones de estado y tipear tester/fecha/observaciones/evidencia directamente en la UI, y eso se guarda en `localStorage` del navegador (clave `qa-muni-home-v2.2-r13`). Esa persistencia de navegador es *efímera y local a esa máquina/perfil* — el estado que cuenta y el que ve cualquier otra persona es el que está escrito en el array `RAW` del archivo. Si el tester ejecuta desde la UI del navegador, para que el cambio quede versionado hay que igualmente reflejarlo en `RAW` (o pedirle que use "Copiar tabla de estado" y pasarte el resultado).
- **`support.js` es el runtime del componente** (parser de templates, manejo de estado, etc.). Nunca se edita para registrar resultados de un TC.
- La carpeta de evidencia se llama **`evidencia/`** (singular), no `evidencias/`.
- `QA-TESTING-GUIDE.md` y `ENGRAM.md` se mencionan dentro de la UI (plantilla de bug, regla RN-009 de Modo Persona Mayor) pero **no están incluidos en este bundle**. Si necesitás consultarlos, pedíselos al tester/PO — no los inventes.

---

## ARRANQUE — qué hacer al iniciar

Al abrir este proyecto en Claude Code, ejecutar estos pasos **en orden**:

### 1. Analizar el proyecto

Leer y entender:

| Archivo | Qué contiene |
|---------|-------------|
| `project/QA HOME Mi DOCTA.dc.html` | Array `RAW` con los 70 TCs. **Acá se editan los resultados.** |
| `project/evidencia/` | Capturas/videos ya asociados a un TC (convención de nombres abajo). |
| `project/uploads/` | Adjuntos sueltos pasados por el tester en el chat, sin clasificar aún — puede haber evidencia pendiente de mover/renombrar a `evidencia/`. |
| `README.md` (del bundle) | Nota de handoff de Claude Design, solo contexto. |

### 2. Mostrar resumen del estado actual

Leer el array `RAW` de `QA HOME Mi DOCTA.dc.html` y mostrar:

```
📋 ESTADO DE EJECUCIÓN — HOME (Web) · Mi DOCTA v2.2
──────────────────────────────────────────────────
Total TCs:   70
✅ OK:        X
❌ NO OK:     X
🔒 Blocked:   X
⚪ Pendiente: X

Progreso: X% completado
──────────────────────────────────────────────────
Próximos P1 pendientes:
  · TC-HOME-XXX · <nombre>
  · ...
```

### 3. Preguntar con qué TC empezar

```
¿Con qué TC empezamos?
(Podés decirme el ID, el número, o el nombre. Ej: "013", "TC-HOME-013", "el de la grilla DOCTA MIA".)
```

---

## WORKFLOW — ciclo de ejecución de un TC

### PASO A — Mostrar la ficha completa del TC

Antes de pedir el resultado, mostrar en formato claro los campos del objeto en `RAW`: `objetivo`, `pre.u` / `pre.e` (precondición), `pasos`, `resultadoEsperado`, y `alertas` si existen. Ejemplo con TC-HOME-013 (pendiente):

```
──────────────────────────────────────────────────
🧪 TC-HOME-013 · Grilla DOCTA MIA del sidebar (8 íconos)
   Tipo: Happy path · Prioridad: P1 · Sección: 2 · Sidebar
──────────────────────────────────────────────────

1️⃣  OBJETIVO
    Verificar que los 8 íconos de la grilla propia del sidebar navegan
    correctamente. DISTINTA al modal 'Descubrí más'.

2️⃣  PRECONDICIÓN
    Usuario: Usuario logueado.
    Estado previo: Sidebar expandido (TC-HOME-008 OK).

3️⃣  PASOS
    1. Click en 'Salud' → módulo Salud.
    2. Click en 'Automotor' → módulo Automotor.
    ... (8 ítems)

4️⃣  RESULTADO ESPERADO
    Cada ícono navega a su módulo. 'Ver más' abre el modal.

⚠️  ALERTA
    'Automotor' (sidebar) vs 'Vehículos' (modal): ¿mismo módulo? Confirmar con PO.

──────────────────────────────────────────────────
Cuando estés listo para ejecutar, avisame.
Una vez ejecutado, pasame:
  · Resultado: OK / NO OK / Blocked
  · Ambiente: TEST / PRUEBA / el que corresponda (si no lo pasás, reutilizo el de la sesión)
  · Evidencia: nombre del archivo (ej: TC-HOME-013_ok.png) o arrastrá la imagen al chat
  · Tester: tu nombre
  · Fecha: (si no la pasás, uso la de hoy)
  · Observación/Bug: (solo si hay algo que documentar)
──────────────────────────────────────────────────
```

### PASO B — Esperar los datos del tester

Esperar **al menos**:
- **Resultado** (OK / NO OK / Blocked) — obligatorio
- **Evidencia** — recomendado; si no la pasa, dejar el campo sin agregar
- **Tester** — si ya se lo pasó antes en la sesión, reutilizar (ej. "Carla Contreras", "Mikael Marovich")
- **Fecha** — si no la pasa, usar la fecha de hoy en formato `YYYY-MM-DD`
- **Ambiente** — si ya se definió para la sesión (`ambiente [URL/nombre]`), reutilizar
- **Observación/Bug** — solo si el resultado es NO OK o Blocked

El tester puede pasar los datos en cualquier formato. Ejemplos válidos:
```
OK, evidencia: TC-HOME-013_ok.png, tester: Carla
```
```
NO OK. El ícono Automotor no navega. Bug ID: BUG-201. Captura: TC-HOME-013_nok.png
```
```
Blocked - ambiente caído
```

### PASO C — Confirmar antes de guardar

Antes de editar el archivo, mostrar el resumen:

```
Voy a guardar esto en TC-HOME-013:

  Estado:       ✅ OK
  Tester:       Carla Contreras
  Fecha:        2026-08-14
  Ambiente:     PRUEBA
  Evidencia:    TC-HOME-013_ok.png
  Observación:  (ninguna)

¿Confirmo? (sí / no / corregir)
```

### PASO D — Editar `project/QA HOME Mi DOCTA.dc.html`

Una vez confirmado, buscar el objeto con `id:"TC-HOME-013"` dentro de `static RAW = [...]` y **agregar/actualizar únicamente** estos campos al final del objeto (siguiendo el mismo estilo que los TCs ya ejecutados):

- `ambiente`
- `status` (`"ok"` / `"nok"` / `"blocked"` / omitido = pendiente)
- `tester`
- `fecha`
- `evidencia` (string legible, ej. `"TC-HOME-013_ok.png"` o `"TC-HOME-013_ok_a.png, TC-HOME-013_ok_b.png"` si son varias)
- `evidenciaSrc` (ruta real hacia `evidencia/`, string o array si son varias: `"evidencia/TC-HOME-013_ok.png"`)
- `observaciones`

**Antes (TC pendiente, sin estos campos):**
```javascript
{ id:"TC-HOME-013", name:"Grilla DOCTA MIA del sidebar (8 íconos)", tipo:"Happy path", prioridad:"P1", seccion:"2 · Sidebar",
  objetivo:"...",
  pre:{ u:"Usuario logueado.", e:"Sidebar expandido (TC-HOME-008 OK)." },
  pasos:[...],
  resultadoEsperado:"...",
  alertas:["⚠️ 'Automotor' (sidebar) vs 'Vehículos' (modal): ¿mismo módulo? Confirmar con PO."] },
```

**Después (ejemplo OK):**
```javascript
{ id:"TC-HOME-013", name:"Grilla DOCTA MIA del sidebar (8 íconos)", tipo:"Happy path", prioridad:"P1", seccion:"2 · Sidebar",
  objetivo:"...",
  pre:{ u:"Usuario logueado.", e:"Sidebar expandido (TC-HOME-008 OK)." },
  pasos:[...],
  resultadoEsperado:"...",
  alertas:["⚠️ 'Automotor' (sidebar) vs 'Vehículos' (modal): ¿mismo módulo? Confirmar con PO."],
  ambiente:"PRUEBA", status:"ok", tester:"Carla Contreras", fecha:"2026-08-14",
  evidencia:"TC-HOME-013_ok.png", evidenciaSrc:"evidencia/TC-HOME-013_ok.png",
  observaciones:"" },
```

**Después (ejemplo NO OK):**
```javascript
  ambiente:"TEST", status:"nok", tester:"Carla Contreras", fecha:"2026-08-14",
  evidencia:"TC-HOME-013_nok.png", evidenciaSrc:"evidencia/TC-HOME-013_nok.png",
  observaciones:"BUG-201: el ícono Automotor no navega." },
```

**Después (ejemplo Blocked):**
```javascript
  status:"blocked", observaciones:"Ambiente PRUEBA caído. Reintentar cuando vuelva." },
```
(Blocked normalmente no lleva `tester`/`fecha`/`evidencia` — mirá los ejemplos ya existentes en TC-HOME-050 a 054 y 061 como referencia de formato.)

> ⚠️ **Regla crítica:** Modificar ÚNICAMENTE los campos mutables (`ambiente`, `status`, `tester`, `fecha`, `evidencia`, `evidenciaSrc`, `observaciones`). No tocar `id`, `name`, `tipo`, `prioridad`, `seccion`, `objetivo`, `pre`, `pasos`, `resultadoEsperado` ni `alertas`. No tocar nada dentro de `support.js` ni la lógica del componente (`static STATUS`, `static TIPO`, `static PRIO`, `static KEY`, los métodos de la clase).

### PASO E — Confirmar cambios y preguntar siguiente

```
✅ TC-HOME-013 guardado como OK.

📊 Progreso actualizado: X/70 (X% completado) · X OK · X NO OK · 6 Blocked · X Pendientes

¿Con cuál seguimos?
```

---

## MANEJO DE EVIDENCIA (IMÁGENES Y VIDEOS)

Cuando el tester arrastra una imagen o video al chat de Claude Code:

1. **Leer/ver el archivo** y confirmar que es la captura del TC correcto.
2. Preguntar: *"¿Cómo querés nombrar este archivo? (sugerencia: `TC-HOME-013_ok.png`)"*
3. Si el tester confirma o no responde, usar la sugerencia.
4. Guardar el nombre en `evidencia` y la ruta `evidencia/<nombre>` en `evidenciaSrc` del TC.

> El archivo en sí no se mueve automáticamente. El tester lo debe copiar manualmente a `project/evidencia/` (o Claude Code puede hacerlo si el tester ya lo dejó en `project/uploads/` y pide que se mueva/renombre). El componente detecta `.mp4` para renderizar `<video>` y cualquier otra extensión como imagen clickeable.

**Convención de nombres a sugerir siempre:**

| Caso | Sugerencia |
|------|------------|
| TC pasó (imagen) | `TC-HOME-013_ok.png` |
| TC falló (imagen) | `TC-HOME-013_nok.png` |
| Múltiples capturas | `TC-HOME-013a_ok.png`, `TC-HOME-013b_ok.png` (o sufijo `_ok_a` / `_ok_b`, como ya se usó en TC-HOME-006 y TC-HOME-012) |
| Video | `TC-HOME-013_ok.mp4` |

Cuando hay más de un archivo, `evidencia` es un string separado por comas (para lectura humana) y `evidenciaSrc` es un array de rutas (para que el componente las renderice todas). Ver TC-HOME-006 y TC-HOME-012 en el archivo como referencia exacta de formato.

---

## MAPEO DE VALORES PARA STATUS

| El tester dice | Guardar como |
|----------------|-------------|
| OK / Pasó / Pasa / ✅ / correcto / bien | `"ok"` |
| NO OK / Falla / Falló / ❌ / mal / no pasa / bug | `"nok"` |
| Blocked / Bloqueado / 🔒 / no puedo / sin ambiente | `"blocked"` |
| Pendiente / skip / después / no ejecuté | (no agregar `status` — el default es `"pendiente"`) |

---

## REGLAS DE COMPORTAMIENTO

### Lo que siempre hacer

- **Mostrar la ficha completa del TC** antes de pedir el resultado (PASO A).
- **Confirmar antes de guardar** (PASO C).
- **Editar solo `project/QA HOME Mi DOCTA.dc.html`** — es la única fuente de verdad, no hay tabla paralela que sincronizar.
- **Preservar el contexto de la sesión**: recordar tester y ambiente para no volver a preguntar.
- **Respetar las `alertas`** de cada TC: si un TC tiene `alertas`, mencionarlas antes de que el tester ejecute.
- **Sugerir el siguiente TC** al terminar, priorizando P1 → P2 → P3.

### Lo que nunca hacer

- Modificar campos no mutables de los TCs (`objetivo`, `pasos`, `resultadoEsperado`, `alertas`, `pre`, `tipo`, `prioridad`, `seccion`, `id`, `name`).
- Tocar `support.js` o la lógica de la clase (`static STATUS/TIPO/PRIO/KEY`, métodos) para registrar un resultado.
- Guardar sin confirmación del tester.
- Cambiar el estado de un TC sin que el tester lo indique explícitamente.
- Inventar datos de tester, fecha, ambiente o evidencia si no fueron provistos.
- Inventar contenido de `QA-TESTING-GUIDE.md` o `ENGRAM.md` — no están en este bundle; si se necesitan, pedirlos.

---

## COMANDOS RÁPIDOS

| Comando | Acción |
|---------|--------|
| `resumen` / `estado` | Mostrar el estado de ejecución actual (contadores + últimos 5 ejecutados) |
| `pendientes p1` | Listar todos los TC con prioridad P1 y estado pendiente |
| `pendientes` | Listar todos los TC pendientes, agrupados por sección |
| `bloqueados` | Listar todos los TC con estado blocked |
| `fallidos` | Listar todos los TC con estado nok |
| `tc XXX` / `013` | Mostrar la ficha completa del TC indicado sin ejecutarlo |
| `siguiente` | Sugerir el siguiente TC recomendado (P1 pendiente más cercano) |
| `rollback TC-HOME-013` | Revertir el TC indicado a estado pendiente (pedir confirmación; quitar los campos mutables) |
| `ambiente [nombre]` | Guardar el ambiente (ej. "PRUEBA", "TEST") para usarlo en toda la sesión |
| `tester [nombre]` | Guardar el nombre del tester para toda la sesión |

---

## CONTEXTO DE PROYECTO

- **Cliente:** Municipalidad de Córdoba
- **Plataforma:** Mi DOCTA (web, no mobile)
- **Módulo:** HOME — vista principal del vecino logueado
- **Versión Figma:** v2.2 (base de los TCs)
- **TCs totales:** 70 (TC-HOME-001 a TC-HOME-070)
- **TCs pre-bloqueados:** 050, 051, 052, 053, 054, 061 (pendientes de frames de Figma — pedir a Mati/Diseño)
- **Testers activos en la sesión hasta ahora:** Carla Contreras, Mikael Marovich

### Secciones del módulo HOME

| Sección | TCs | Notas |
|---------|-----|-------|
| 1 · Header y branding | 001–006 | |
| 2 · Sidebar | 007–014 | |
| 3 · Modal 'Descubrí más' | 015–019 | |
| 4 · Buscador de trámites | 020–024 | |
| 5 · Servicios Digitales | 025–026 | ⚠️ Confirmar set de tarjetas con Diseño antes de ejecutar |
| 6 · Tabs (7 tabs) | 027–029 | |
| 7 · Banner 'TU RESUMEN' | 030–035 | |
| 8 · Contribuciones | 036–043 | |
| 9 · Reclamos | 044–049 | |
| 10 · Turnos | 050–052 | 🔒 Blocked — pendiente frame Figma |
| 11 · Comunicaciones | 053–054 | 🔒 Blocked — pendiente frame Figma |
| 12 · Infracciones | 055–060 | |
| 13 · Trámites / Juicios | 061 | 🔒 Blocked — pendiente frame Figma |
| 14 · Footer | 062–064 | |
| 15 · Docta MIA flotante | 065–066 | |
| 16 · Navegación general | 067–070 | |

### Prioridades

| Prioridad | Descripción | Cuándo ejecutar |
|-----------|-------------|-----------------|
| P1 · Crítico | Funcionalidad core que bloquea al vecino si falla | Primero, siempre |
| P2 · Importante | Funcionalidad relevante que impacta la UX | Segundo |
| P3 · Edge / Visual | Casos borde, responsive, texto | Cuando no haya P1 o P2 pendiente |

---

## HALLAZGOS ABIERTOS (consultar antes de ejecutar los TCs relacionados)

| # | Hallazgo | TC | Responsable | Estado |
|---|----------|----|-------------|--------|
| 1 | Set real de tarjetas de Servicios Digitales no confirmado | TC-025 | Diseño | Abierto |
| 2 | "Automotor" (sidebar) vs "Vehículos" (modal): ¿mismo módulo? | TC-013, TC-016 | PO | Abierto |
| 3 | "Tablero de Datos" e "Interacciones": qué hacen | TC-010 | PO | Abierto |
| 4 | Estado "Cumplido" de Reclamos: ¿sigue vigente en v2? | TC-044 | Diseño/Dev | Abierto |
| 5 | Frame Figma Turnos pendiente | TC-050/051/052 | Mati/Diseño | Abierto |
| 6 | Frame Figma Comunicaciones pendiente | TC-053/054 | Mati/Diseño | Abierto |
| 7 | Frame Figma Trámites y Juicios pendiente | TC-061 | Mati/Diseño | Abierto |
| 8 | Copy "Tenés 2 infracción pendiente": ¿typo? | TC-055 | PO/Copy | Abierto |
| 9 | Color naranja tab Mis Infracciones: ¿intencional? | TC-060 | Diseño | Abierto |
| 10 | TC-HOME-018 (ícono PPT) marcado NO OK: servicio no estaba desplegado al momento del testeo, no es necesariamente un bug de UI | TC-018 | Dev | Abierto — re-testear cuando el servicio esté disponible |

---

## EJEMPLO DE SESIÓN COMPLETA

```
[CLAUDE CODE al abrir el proyecto]
──────────────────────────────────────────────────
📋 ESTADO DE EJECUCIÓN — HOME (Web) · Mi DOCTA v2.2
  Total: 70 · ✅ 11 · ❌ 1 · 🔒 6 · ⚪ 52
  Progreso: 26% completado

Próximos P1 pendientes:
  · TC-HOME-013 · Grilla DOCTA MIA del sidebar (8 íconos)
  · TC-HOME-015 · Abrir modal desde 'Ver más' (Servicios Digitales)
  · TC-HOME-016 · Cada ícono del modal navega al módulo correcto
  · TC-HOME-020 · Buscar un trámite válido
  · TC-HOME-024 · Botones de trámite frecuente navegan correctamente

¿Con qué TC empezamos?
──────────────────────────────────────────────────

[TESTER] 013

[CLAUDE CODE]
──────────────────────────────────────────────────
🧪 TC-HOME-013 · Grilla DOCTA MIA del sidebar (8 íconos)
   Tipo: Happy path · Prioridad: P1 · Sección: 2 · Sidebar
──────────────────────────────────────────────────

1️⃣  OBJETIVO
    Verificar que los 8 íconos de la grilla propia del sidebar navegan
    correctamente. DISTINTA al modal 'Descubrí más'.

2️⃣  PRECONDICIÓN
    Usuario: Usuario logueado.
    Estado previo: Sidebar expandido (TC-HOME-008 OK).

3️⃣  PASOS
    1. Click en 'Salud' → módulo Salud.
    ... (8 ítems, ver ficha completa)

4️⃣  RESULTADO ESPERADO
    Cada ícono navega a su módulo. 'Ver más' abre el modal.

⚠️  ALERTA
    'Automotor' (sidebar) vs 'Vehículos' (modal): ¿mismo módulo? Confirmar con PO.

──────────────────────────────────────────────────
Cuando estés listo, pasame el resultado.
──────────────────────────────────────────────────

[TESTER] ok, tester Carla, evidencia TC-HOME-013_ok.png

[CLAUDE CODE]
Voy a guardar esto en TC-HOME-013:

  Estado:    ✅ OK
  Tester:    Carla Contreras
  Fecha:     2026-08-14
  Ambiente:  PRUEBA
  Evidencia: TC-HOME-013_ok.png
  Obs:       (ninguna)

¿Confirmo?

[TESTER] sí

[CLAUDE CODE — edita project/QA HOME Mi DOCTA.dc.html]

✅ TC-HOME-013 guardado como OK.

📊 Progreso: 12/70 (17%) · ✅ 12 · ❌ 1 · 🔒 6 · ⚪ 51

Siguiente recomendado: TC-HOME-015 (P1 · Abrir modal desde 'Ver más')
¿Seguimos con ese o querés ir a otro?
```
