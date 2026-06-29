# Portfolio Restructure Design Spec
**Date:** 2026-06-29
**Author:** Cristian Rodriguez
**Status:** Approved

---

## Objetivo

Rediseñar el portfolio de `script32.github.io/portfolio` para:
1. Eliminar el espacio desperdiciado del layout actual (section 500px fija)
2. Separar claramente dos roles profesionales distintos
3. Optimizar para lectura por ATS/IA de reclutamiento
4. Enfocar la propuesta de valor hacia **clientes de consultoría** en todas las industrias

---

## Arquitectura de Páginas

```
script32.github.io/portfolio/
├── index.md              → Landing (bifurcación entre roles)
├── consulting.md         → Consultor de TI (2007–presente)
├── business-leader.md    → Líder de Negocio TI (2017–presente)
└── _layouts/default.html → Layout compartido, navbar sticky
```

Se eliminan las páginas de proyectos individuales (`/projects/*.html`) del flujo principal; el contenido relevante se integra en las páginas de rol.

---

## Layout

### Problema actual
`jekyll-theme-minimal` impone:
- `header { width: 270px; float: left; position: fixed; }`
- `section { width: 500px; float: right; }`
- Resultado: solo 500px de contenido en pantallas de 1280px+

### Solución
Sobrescribir el layout en `_layouts/default.html`:

```css
/* Wrapper amplio */
.wrapper {
  max-width: 1200px;
  width: 95%;
  margin: 0 auto;
  padding: 0 20px;
}

/* Navbar sticky reemplaza sidebar fijo */
.site-nav {
  position: sticky;
  top: 0;
  z-index: 1000;
  background: rgba(0, 30, 60, 0.95);
  backdrop-filter: blur(8px);
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 12px 24px;
}

/* Contenido a full width */
section {
  width: 100%;
  float: none;
  padding: 40px 0;
}

header {
  width: auto;
  float: none;
  position: static;
}
```

### Navbar
Contenido: `[Logo/Nombre] ......... [Consultor TI] [Líder TI] [Contacto]`

Los links "Consultor TI" y "Líder TI" navegan a sus páginas respectivas. En mobile colapsa a hamburger menu.

### Grid de contenido
Cards de proyectos usan CSS Grid:
```css
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
  gap: 24px;
}
```
Resultado: 3 columnas en desktop, 2 en tablet, 1 en mobile.

---

## Páginas

### Landing (`index.md`)

**Hero compacto:**
- Foto de perfil circular
- Nombre + tagline: "AI Strategy Consultant | 20+ años transformando industrias"
- 2 stats destacados: `600+ proyectos | $7M proyectos gestionados`

**Bifurcación (2 cards side-by-side):**

| Card Izquierda | Card Derecha |
|---|---|
| Consultor de TI | Líder de Negocio TI |
| Azul #0066cc + Verde #00cc66 | Azul oscuro #003366 + Dorado #f59e0b |
| 2007–hoy · 5 países · 6+ industrias | 2017–hoy · Grupo Saesa · 600+ proyectos |
| CTA: "Ver experiencia de consultoría" | CTA: "Ver liderazgo TI" |

**Footer mínimo:** LinkedIn · GitHub · Kaggle

---

### Consultor de TI (`consulting.md`)

**Paleta:** Azul #0066cc + Verde #00cc66 (identidad actual)

**Secciones:**

1. **Hero** — Mapa Leaflet mostrando Chile, España, México, Colombia, Brasil con markers en ciudades clave. Overlay: "Transformando industrias con IA y tecnología · 5 países"

2. **Industrias servidas** — Grid de 6 cards:
   - Utility (eléctrico, distribución, transmisión)
   - Hoteles
   - Automotriz
   - Gobierno
   - Alimentos
   - Otras

3. **Proyectos por impacto** — Reutilizar los project-cards actuales. Los proyectos de mayor ROI primero:
   - Análisis de Vegetación con IA ($9M USD)
   - Modelo XGBoost IBM Watson (12M+ predicciones)
   - Smart Agent Utility (proyecto estrella)
   - Análisis de Activos con Drones (300K+ imágenes)
   - Workforce Management (300+ brigadas)
   - Data Science: Kaggle, Stanford NLP, BERT, LightGBM, etc.
   - Integraciones empresariales: SSIS/SAP, SharePoint SPFx, SAP PM/PS

4. **Stack técnico** — Grid actual de 4 categorías (mantener)

5. **Charlas y publicaciones** — Sección actual (mantener)

6. **CTA de contacto** — LinkedIn + GitHub + Kaggle

---

### Líder de Negocio TI (`business-leader.md`)

**Paleta:** Azul oscuro #003366 + Dorado/Amber #f59e0b

**Secciones:**

1. **Hero** — Sin mapa. Métricas grandes en cards:
   ```
   [600+ Proyectos]  [$7M Max]  [1000+ Cuadrillas]  [9 años liderando]
   ```

2. **Empresa actual** — Grupo Saesa, Leader of IT Business (Feb 2017–presente). Descripción del rol: gestión de equipos, presupuesto, roadmap, relación negocio-TI, transformación digital.

3. **Proyectos emblematicos** — Timeline vertical con los 3 grandes:
   - **SCADA Distribuidora** — Sistema de supervisión y control de red eléctrica
   - **Sistema Técnico Geográfico (STG)** — GIS para distribuidora y transmisora eléctrica
   - **Movilidad y Digitalización Cuadrillas** — App móvil + digitalización para 1000+ brigadas de terreno

4. **Escala de proyectos** — Rango $10K–$7M, casos de negocio, 600+ proyectos ejecutados

5. **Herramientas de gestión** — SAP PM/PS, Azure DevOps, metodologías ágiles, etc.

6. **CTA** — LinkedIn

---

## Identidad Visual por Rol

| Elemento | Consultor TI | Líder TI |
|---|---|---|
| Color primario | #0066cc | #003366 |
| Color acento | #00cc66 | #f59e0b |
| Hero | Mapa Leaflet interactivo | Métricas de escala |
| Tono | Innovación, agilidad, tecnología | Liderazgo, escala, estrategia |
| Cards borde | Verde #00cc66 | Dorado #f59e0b |

La landing usa ambas paletas solo en las cards de bifurcación.

---

## Contenido ATS/IA (Oculto Visualmente)

### Estrategia dual

**Capa 1 — JSON-LD Schema.org expandido** (en `<head>` de cada página)

Tipo `Person` con:
- `hasOccupation` con cada posición laboral
- `alumniOf` con todas las instituciones educativas
- `hasCredential` con certificaciones
- `knowsAbout` con 40+ keywords de skills

**Capa 2 — Bloque HTML semántico oculto** (al final del `<body>` en `index.md`)

```css
.ats-resume {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}
```

Contenido del bloque `.ats-resume`:

```
CRISTIAN RODRIGUEZ
AI Strategy Consultant | IT Business Leader | Geospatial Solutions Architect

EXPERIENCIA LABORAL

Grupo Saesa — Leader of IT Business
Febrero 2017 – Presente | Osorno, Chile
- Liderazgo de transformación digital con IA, Data Science y tecnologías geoespaciales
- Gestión de más de 600 proyectos con presupuestos entre USD 10,000 y USD 7,000,000
- Gestión de equipos TI, presupuesto, roadmap tecnológico y relación con el negocio
- Proyectos emblematicos: SCADA Distribuidora, Sistema Técnico Geográfico, Movilidad 1000+ cuadrillas
- Implementación de modelos predictivos XGBoost en IBM Watson (12M+ predicciones, 85% accuracy)
- Análisis de vegetación satelital en 11,500 km de líneas eléctricas (ROI $9M USD)
- Inspección de activos con drones Optelos/Neurala/Azure (300,000+ imágenes)
- Gestión de fuerza de trabajo con Synchroteam para 300+ brigadas

Grupo Saesa — Service Quality Engineer
Enero 2011 – Enero 2017 | Osorno, Chile
- Integración de tecnologías geográficas y móviles en operaciones de campo
- Implementación de sistemas GIS para distribución eléctrica

CLAS — Head of IT Department
Enero 2007 – Diciembre 2010 | Chile
- Administración de ERP, hardware y desarrollo de aplicaciones móviles
- Implementación de sistemas de guía vehicular para Parque Arauco y Clínica las Condes

CONSULTORÍA INTERNACIONAL DE TI — 2007 – Presente
Países: Chile, España, México, Colombia, Brasil
Industrias: Utility (eléctrico), Hoteles, Automotriz, Gobierno, Alimentos
- Implementaciones técnicas y arquitectura de soluciones para clientes empresariales
- Implementación de Inteligencia Artificial en procesos de negocio
- Desarrollo de soluciones geoespaciales y análisis de datos

EDUCACIÓN

MBA Finance — Universitat de Barcelona (2021–2022) | GPA: 8.9
Computer Science Engineering — Universidad Gabriela Mistral (2013–2014)
IT Technical Degree — Universidad de Los Lagos (2001–2004)

CERTIFICACIONES

Machine Learning — MIT Professional Education (Diciembre 2019)
Computer Vision with Watson and OpenCV — Coursera/IBM (Julio 2019)
Digital Transformation for Leaders — LinkedIn Learning (Septiembre 2020)
Data Science Foundations Level 1 — IBM (Junio 2018)
IBM Bluemix Essentials — IBM
Watson Conversation Service — IBM
Node-RED Basics — IBM

HABILIDADES TÉCNICAS

Lenguajes: Python, C#, .NET, JavaScript, TypeScript, R, SQL, ABAP
Bases de datos: PostgreSQL, PostGIS, SQL Server, MySQL, MongoDB, SAP HANA
Cloud: Azure, AWS, Google Cloud Platform, IBM Cloud, IBM Watson
IA/ML: TensorFlow, PyTorch, XGBoost, LightGBM, BERT, Transformers, Scikit-learn, OpenCV
GIS/Geoespacial: PostGIS, Leaflet.js, QGIS, Sistemas de Información Geográfica
Frameworks: React, SPFx, Angular, Node.js, .NET Core, FastAPI
DevOps: Docker, GitHub Actions, CI/CD, Azure DevOps
Empresarial: SAP PM, SAP PS, SSIS, Node-RED, Synchroteam, SharePoint
Metodologías: Agile, Scrum, casos de negocio, gestión de proyectos

ROLES: AI Consultant, IT Business Leader, Data Scientist, Solutions Architect,
Geospatial Engineer, MLOps Engineer, Product Manager, Digital Transformation Lead,
Software Developer, FullStack Developer, Python Developer, Machine Learning Engineer
```

---

## Archivos a Crear/Modificar

| Archivo | Acción |
|---|---|
| `_layouts/default.html` | Modificar: navbar sticky, wrapper 1200px, section full-width |
| `index.md` | Reescribir: landing bifurcación |
| `consulting.md` | Crear: contenido consultoría |
| `business-leader.md` | Crear: contenido liderazgo TI |
| `_sass/jekyll-theme-minimal.scss` | Modificar: sobrescribir section/header/wrapper |

---

## Consideraciones Técnicas

- **Leaflet.js**: Solo se carga en `consulting.md`, no en todas las páginas
- **SEO**: Cada página tiene su propio `<title>`, description y JSON-LD específico
- **Performance**: El bloque ATS usa CSS clip (no `display:none`) para que sea indexable pero no afecte layout
- **Mobile**: Navbar colapsa con CSS puro (checkbox hack) sin JS adicional
- **Jekyll**: Páginas nuevas usan `layout: default` en el front matter
