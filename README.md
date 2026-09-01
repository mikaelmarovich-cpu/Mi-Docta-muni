# QA Mi DOCTA · Municipalidad de Córdoba

Proyecto de ejecución de QA de la plataforma Mi DOCTA. Un único tablero
interactivo, con un selector de módulo arriba a la izquierda — hoy cubre
**HOME (Web)** (70 casos), **Autenticación e Ingreso (Web)** (33 casos),
**Contribuciones Municipales (Web)** (35 casos), **Inmuebles (Web)** (13 casos),
**Automotor (Web)** (15 casos), **Estacionamiento (Web)** (15 casos) —
cierra el Clúster B — y **Perfil del Usuario (Web)** (18 casos), primer
módulo del Clúster A.
Cada módulo nuevo se agrega como una entrada más del selector, reusando
el mismo componente — no se crea un repo ni un archivo nuevo por módulo.

## 🔗 Ver el tablero de ejecución (HTML interactivo)

> ⚠️ Requiere que **GitHub Pages** esté activado en este repositorio (Settings → Pages → Source: branch `main`, carpeta `/root`).

**[Abrir QA Mi DOCTA (vista interactiva)](https://mikaelmarovich-cpu.github.io/Mi-Docta-muni/qa-home-mi-docta-municipalidad-de-c-rdoba-versi-n-del-docume/project/QA%20HOME%20Mi%20DOCTA.dc.html)**

Desde ahí se puede elegir el módulo (selector superior), filtrar por
estado/prioridad/sección, ver el detalle de cada caso de prueba, y
descargar un informe en PDF con el botón "DESCARGAR PDF". El progreso de
cada módulo se guarda por separado (no se mezclan los resultados de HOME
con los de Autenticación).

## Estructura

```
qa-home-mi-docta-municipalidad-de-c-rdoba-versi-n-del-docume/
├── README.md                         ← nota de handoff de Claude Design
└── project/
    ├── QA HOME Mi DOCTA.dc.html      ← fuente de verdad: los 70 TCs y sus resultados
    ├── support.js                    ← motor de render del componente
    ├── evidencia/                    ← capturas/videos de evidencia por TC
    └── uploads/                      ← adjuntos sueltos sin clasificar
```

Ver `CLAUDE.md` para el detalle del workflow de ejecución de QA de este proyecto.
