# QA HOME · Mi DOCTA · Municipalidad de Córdoba (v2.2)

Proyecto de ejecución de QA para el módulo HOME (web) de la plataforma Mi DOCTA.

## 🔗 Ver el tablero de ejecución (HTML interactivo)

> ⚠️ Requiere que **GitHub Pages** esté activado en este repositorio (Settings → Pages → Source: branch `main`, carpeta `/root`).

**[Abrir QA HOME Mi DOCTA (vista interactiva)](https://mikaelmarovich-cpu.github.io/Mi-Docta-muni/qa-home-mi-docta-municipalidad-de-c-rdoba-versi-n-del-docume/project/QA%20HOME%20Mi%20DOCTA.dc.html)**

Desde ahí se puede filtrar por estado/prioridad/sección, ver el detalle de cada uno de los 70 casos de prueba, y descargar un informe en PDF con el botón "DESCARGAR PDF".

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
