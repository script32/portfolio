---
layout: default
title: Consultor de TI - Cristian Rodriguez
description: "18+ años implementando IA, soluciones geoespaciales y transformación digital en Chile, España, México, Colombia y Brasil. Utility, Gobierno, Hoteles, Automotriz y más."
use_map: true
---

<!-- HERO CON MAPA MULTI-PAÍS -->
<div class="geo-hero">
  <div id="map"></div>
  <div class="hero-overlay">
    <h1>Consultor de TI Internacional</h1>
    <p>18+ años transformando industrias con IA y tecnología &middot; 5 países &middot; 6+ industrias</p>
  </div>
</div>

<script>
document.addEventListener('DOMContentLoaded', function() {
  var map = L.map('map', {
    zoomControl: false,
    attributionControl: false,
    dragging: false,
    scrollWheelZoom: false,
    doubleClickZoom: false
  }).setView([10, -55], 3);

  L.tileLayer('https://{s}.basemaps.cartocdn.com/dark_all/{z}/{x}/{y}{r}.png', {
    maxZoom: 10
  }).addTo(map);

  var locations = [
    { coords: [-33.45, -70.65], label: 'Chile',    color: '#00cc66', r: 10 },
    { coords: [40.41,  -3.70],  label: 'España',   color: '#00aaff', r: 8 },
    { coords: [19.43, -99.13],  label: 'México',   color: '#00aaff', r: 8 },
    { coords: [4.71,  -74.07],  label: 'Colombia', color: '#00aaff', r: 8 },
    { coords: [-23.55,-46.63],  label: 'Brasil',   color: '#00aaff', r: 8 }
  ];

  locations.forEach(function(loc) {
    L.circleMarker(loc.coords, {
      radius: loc.r,
      fillColor: loc.color,
      color: '#fff',
      weight: 2,
      opacity: 1,
      fillOpacity: 0.92
    }).bindTooltip(loc.label, {
      permanent: true,
      direction: 'top',
      offset: [0, -12],
      className: 'country-tooltip'
    }).addTo(map);
  });
});
</script>

<style>
.leaflet-tooltip.country-tooltip {
  background: rgba(0, 25, 51, 0.92);
  color: white;
  border: 1px solid #00cc66;
  font-weight: 600;
  font-size: 0.78em;
  padding: 3px 9px;
  border-radius: 4px;
  box-shadow: none;
}
.leaflet-tooltip.country-tooltip::before { display: none; }
</style>

---

## Industrias Servidas

<div class="industries-grid">
  <div class="industry-card">
    <div class="industry-icon">⚡</div>
    <div class="industry-name">Utility &amp; Energía</div>
  </div>
  <div class="industry-card">
    <div class="industry-icon">🏨</div>
    <div class="industry-name">Hoteles &amp; Turismo</div>
  </div>
  <div class="industry-card">
    <div class="industry-icon">🚗</div>
    <div class="industry-name">Automotriz</div>
  </div>
  <div class="industry-card">
    <div class="industry-icon">🏛️</div>
    <div class="industry-name">Gobierno</div>
  </div>
  <div class="industry-card">
    <div class="industry-icon">🌾</div>
    <div class="industry-name">Alimentos</div>
  </div>
  <div class="industry-card">
    <div class="industry-icon">🏥</div>
    <div class="industry-name">Salud &amp; Otros</div>
  </div>
</div>

---

## Arquitectura de Sistemas

<div style="text-align: justify">
Mi enfoque va más allá de escribir scripts; diseño <strong>sistemas completos</strong> que integran múltiples capas de tecnología. La siguiente arquitectura muestra cómo los componentes interactúan en proyectos para el sector Utility:
</div>

<center><img src="{{ '/images/arquitectura.jpg' | relative_url }}" alt="Arquitectura de Sistemas" style="max-width: 100%; border-radius: 10px; margin-top: 16px;"/></center>

---

## Proyecto Estrella: Smart Agent Utility

<div class="project-card featured">

<h3>El Desafío de las Operaciones de Campo</h3>

<div style="text-align: justify">
Los sistemas actuales en empresas de servicios públicos operan en <strong>silos de datos</strong>. Los trabajadores de campo carecen de inteligencia en tiempo real, dependiendo de información desactualizada que llega con horas o días de retraso. Esto resulta en tiempos de respuesta lentos ante emergencias, rutas subóptimas y decisiones basadas en datos incompletos.
</div>

<h3>La Solución: Agentes Autónomos Inteligentes</h3>

<div style="text-align: justify">
<strong>Smart Agent Utility</strong> es un sistema de agentes autónomos que utiliza la <em>ubicación del trabajador en tiempo real</em> para enviar información contextual proactiva. El agente analiza múltiples fuentes de datos y toma decisiones para empoderar a los equipos de campo.
</div>

<div class="tech-stack">
  <span class="tech-badge">Python</span>
  <span class="tech-badge">PostGIS</span>
  <span class="tech-badge">Machine Learning</span>
  <span class="tech-badge">Real-time APIs</span>
  <span class="tech-badge">Cloud Native</span>
</div>

<a href="https://github.com/script32/smart_agent_utility" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Smart_Agent_Utility-00cc66?logo=github&logoColor=white&style=for-the-badge" alt="Ver en GitHub"/></a>

</div>

---

## Proyectos de Alto Impacto

<div class="projects-grid">

<div class="project-card">
<h3>Análisis de Vegetación con IA &mdash; ROI: $9M USD</h3>
<div style="text-align: justify">
Inspección mediante <strong>imágenes satelitales</strong> de 11.500 kilómetros de líneas eléctricas. Detecta vegetación de riesgo y programa trabajos preventivos, generando <strong>$9 millones USD</strong> en prevención de fallas.
</div>
<div class="tech-stack">
  <span class="tech-badge">Imágenes Satelitales</span>
  <span class="tech-badge">Computer Vision</span>
  <span class="tech-badge">Python</span>
  <span class="tech-badge">GIS</span>
</div>
<center><img src="{{ '/images/Arbol.jpeg' | relative_url }}" alt="Análisis de Vegetación" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

<div class="project-card">
<h3>Modelo Predictivo XGBoost &mdash; 12M+ Predicciones</h3>
<div style="text-align: justify">
Modelo <strong>XGBoost</strong> desplegado en IBM Watson que predice el tiempo de reposición del servicio eléctrico en los primeros 15 minutos de una llamada de cliente. Integra datos de clima, ubicación geográfica y tiempos históricos.
</div>
<div class="stats-row" style="margin: 16px 0;">
  <div class="stat-card"><div class="number">12M+</div><div class="label">Predicciones</div></div>
  <div class="stat-card"><div class="number">85%</div><div class="label">Precisión</div></div>
  <div class="stat-card"><div class="number">15 min</div><div class="label">Tiempo respuesta</div></div>
</div>
<div class="tech-stack">
  <span class="tech-badge">XGBoost</span>
  <span class="tech-badge">IBM Watson</span>
  <span class="tech-badge">Node-RED</span>
  <span class="tech-badge">Weather API</span>
</div>
<center><img src="{{ '/images/xgboost.jpg' | relative_url }}" alt="XGBoost Model" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

<div class="project-card">
<h3>Análisis de Activos con Drones &mdash; 300K+ Imágenes</h3>
<div style="text-align: justify">
Implementación de <strong>Optelos</strong> con cerebros de IA entrenados para inspección de activos eléctricos. Integra tecnología Neurala con Microsoft Azure, gestionando más de <strong>300.000 imágenes</strong> de inspecciones.
</div>
<div class="tech-stack">
  <span class="tech-badge">Drones</span>
  <span class="tech-badge">Neurala AI</span>
  <span class="tech-badge">Azure</span>
  <span class="tech-badge">Computer Vision</span>
</div>
<center><img src="{{ '/images/Optelos.jpeg' | relative_url }}" alt="Optelos Platform" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

<div class="project-card">
<h3>Gestión de Fuerza de Trabajo &mdash; 300+ Brigadas</h3>
<div style="text-align: justify">
Implementación de <strong>Synchroteam</strong> para administrar la gestión de fuerza de trabajo en terreno. Trazabilidad completa de activos, recursos y materiales para más de 300 brigadas operativas.
</div>
<div class="tech-stack">
  <span class="tech-badge">Geolocalización</span>
  <span class="tech-badge">Mobile</span>
  <span class="tech-badge">Real-time Sync</span>
  <span class="tech-badge">Analytics</span>
</div>
<center><img src="{{ '/images/sync.jpg' | relative_url }}" alt="Synchroteam" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

</div>

---

## Integraciones Empresariales

<div class="projects-grid">

<div class="project-card">
<h3>Librería SSIS SAP CO &mdash; ETL Avanzado</h3>
<div style="text-align: justify">
Librería desarrollada en <strong>C#</strong> para simplificar la interacción con tablas SAP desde SSIS. Permite explorar tablas, generar consultas optimizadas y realizar JOINs complejos sin código extenso.
</div>
<div class="tech-stack">
  <span class="tech-badge">C#</span>
  <span class="tech-badge">SSIS</span>
  <span class="tech-badge">SAP RFC</span>
  <span class="tech-badge">SQL Server</span>
</div>
<center><img src="{{ '/images/SSIS sources-final.png' | relative_url }}" alt="SSIS SAP Integration" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

<div class="project-card">
<h3>SharePoint &mdash; Gestión del Conocimiento con React SPFx</h3>
<div style="text-align: justify">
Plataforma de gestión del conocimiento para empresa multinacional utilizando <strong>React</strong> y <strong>SPFx</strong>. Centraliza el acceso a información con seguridad basada en roles y búsqueda avanzada mediante Graph API.
</div>
<div class="tech-stack">
  <span class="tech-badge">React</span>
  <span class="tech-badge">SPFx</span>
  <span class="tech-badge">Graph API</span>
  <span class="tech-badge">Role-based Security</span>
</div>
<center><img src="{{ '/images/sharepoint.png' | relative_url }}" alt="SharePoint SPFx" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

<div class="project-card">
<h3>Proyectos SAP PM/PS</h3>
<div style="text-align: justify">
Liderazgo en implementaciones de módulos <strong>SAP PM</strong> (Plant Maintenance) y <strong>SAP PS</strong> (Project Systems) con integraciones avanzadas: liquidación automática de activos, valoración de presupuestos, planes de mantenimiento, integración C4C y OMS.
</div>
<div class="tech-stack">
  <span class="tech-badge">SAP PM</span>
  <span class="tech-badge">SAP PS</span>
  <span class="tech-badge">ABAP</span>
  <span class="tech-badge">Integration</span>
</div>
<center><img src="{{ '/images/sap.jpg' | relative_url }}" alt="SAP Projects" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

</div>

---

## Data Science &amp; Machine Learning

<div class="projects-grid">

<div class="project-card">
<h3>COVID-19 Research Challenge &mdash; Kaggle</h3>
<a href="https://www.kaggle.com/crprpr/vaccine-data-filter" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Kaggle-Ver_Notebook-20BEFF?logo=kaggle&style=for-the-badge" alt="Ver Notebook"/></a>
<div style="text-align:justify;margin-top:12px;">Notebook para búsqueda de metadatos en publicaciones médicas utilizando <strong>NLP</strong> para encontrar relaciones en condiciones afectadas por el virus.</div>
<div class="tech-stack"><span class="tech-badge">Python</span><span class="tech-badge">NLTK</span><span class="tech-badge">NLP</span></div>
</div>

<div class="project-card">
<h3>Predicción de Demanda Energética</h3>
<a href="https://colab.research.google.com/drive/1gVBN1qg3ajEjxJPkd_YwXb9yLDBMQAR0" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Colab-Ejecutar_Notebook-F9AB00?logo=googlecolab&style=for-the-badge" alt="Ejecutar en Colab"/></a>
<div style="text-align:justify;margin-top:12px;">Modelo <strong>LightGBM</strong> para predecir rendimiento energético según tipo de instalación y datos climáticos.</div>
<div class="tech-stack"><span class="tech-badge">LightGBM</span><span class="tech-badge">Weather Data</span><span class="tech-badge">Regression</span></div>
</div>

<div class="project-card">
<h3>Reconocimiento Facial &mdash; API + Web Platform</h3>
<a href="https://github.com/script32/face" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Ver_Repositorio-181717?logo=github&logoColor=white&style=for-the-badge" alt="Ver en GitHub"/></a>
<div style="text-align:justify;margin-top:12px;">Sistema completo de reconocimiento facial con API REST y plataforma web. Utiliza <strong>PostgreSQL + CUDA</strong> para embeddings faciales de alto rendimiento.</div>
<div class="tech-stack"><span class="tech-badge">Python</span><span class="tech-badge">PostgreSQL</span><span class="tech-badge">CUDA</span><span class="tech-badge">REST API</span></div>
<center><img src="{{ '/images/face.png' | relative_url }}" alt="Face Recognition" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>CS224n: NLP con Deep Learning &mdash; Stanford</h3>
<a href="https://github.com/script32/CS224n-NLP" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Ver_Repositorio-181717?logo=github&logoColor=white&style=for-the-badge" alt="Ver en GitHub"/></a>
<div style="text-align:justify;margin-top:12px;">Implementación completa del curso de Stanford. Incluye sistema de <strong>Traducción Automática Neuronal</strong> español-inglés con encoder LSTM bidireccional y decoder con atención multiplicativa.</div>
<div class="tech-stack"><span class="tech-badge">Deep Learning</span><span class="tech-badge">NLP</span><span class="tech-badge">LSTM</span><span class="tech-badge">Attention</span></div>
<center><img src="{{ '/images/nlp.png' | relative_url }}" alt="NLP Stanford" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>Análisis de Sentimiento &mdash; Detección de Toxicidad</h3>
<a href="https://www.kaggle.com/crprpr/clasificaci-n-de-texto-tensorflow-transformers" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Kaggle-Ver_Notebook-20BEFF?logo=kaggle&style=for-the-badge" alt="Ver en Kaggle"/></a>
<div style="text-align:justify;margin-top:12px;">Modelos de ML para identificar toxicidad en conversaciones online utilizando <strong>BERT</strong> y Transformers.</div>
<div class="tech-stack"><span class="tech-badge">BERT</span><span class="tech-badge">TensorFlow</span><span class="tech-badge">Transformers</span></div>
<center><img src="{{ '/images/BERT-classification.png' | relative_url }}" alt="BERT Classification" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>Biblioteca de Modelos ML &mdash; Python &amp; R</h3>
<a href="https://github.com/script32/Modelos-ML" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Ver_Repositorio-181717?logo=github&logoColor=white&style=for-the-badge" alt="Ver en GitHub"/></a>
<div style="text-align:justify;margin-top:12px;">Repositorio completo con ejemplos de todos los tipos de modelos de Machine Learning en <strong>Python</strong> y <strong>R</strong> con datasets de ejemplo.</div>
<div class="tech-stack"><span class="tech-badge">Python</span><span class="tech-badge">R</span><span class="tech-badge">Scikit-learn</span><span class="tech-badge">XGBoost</span></div>
<center><img src="{{ '/images/ML.jpg' | relative_url }}" alt="ML Library" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

</div>

---

## Stack Tecnológico

<div class="skills-grid">
<div class="skill-category">
<h4>Lenguajes de Programación</h4>
<ul class="skill-list">
  <li><strong>Python</strong> <span class="level-expert">Experto</span></li>
  <li><strong>C# / .NET</strong> <span class="level-expert">Experto</span></li>
  <li><strong>JavaScript / TypeScript</strong> <span class="level-expert">Experto</span></li>
  <li><strong>R</strong> <span class="level-advanced">Avanzado</span></li>
  <li><strong>SQL</strong> <span class="level-expert">Experto</span></li>
</ul>
</div>
<div class="skill-category">
<h4>Bases de Datos &amp; GIS</h4>
<ul class="skill-list">
  <li><strong>PostgreSQL / PostGIS</strong> <span class="level-expert">Experto</span></li>
  <li><strong>SQL Server</strong> <span class="level-expert">Experto</span></li>
  <li><strong>MySQL</strong> <span class="level-advanced">Avanzado</span></li>
  <li><strong>MongoDB</strong> <span class="level-advanced">Avanzado</span></li>
  <li><strong>SAP HANA</strong> <span class="level-advanced">Avanzado</span></li>
</ul>
</div>
<div class="skill-category">
<h4>Cloud &amp; DevOps</h4>
<ul class="skill-list">
  <li><strong>Azure</strong> <span class="level-expert">Experto</span></li>
  <li><strong>IBM Cloud / Watson</strong> <span class="level-expert">Experto</span></li>
  <li><strong>AWS</strong> <span class="level-advanced">Avanzado</span></li>
  <li><strong>Google Cloud</strong> <span class="level-advanced">Avanzado</span></li>
  <li><strong>Docker</strong> <span class="level-advanced">Avanzado</span></li>
</ul>
</div>
<div class="skill-category">
<h4>Frameworks &amp; IA</h4>
<ul class="skill-list">
  <li><strong>React / SPFx</strong> <span class="level-expert">Experto</span></li>
  <li><strong>Node.js / .NET Core</strong> <span class="level-expert">Experto</span></li>
  <li><strong>TensorFlow / PyTorch</strong> <span class="level-advanced">Avanzado</span></li>
  <li><strong>XGBoost / LightGBM</strong> <span class="level-expert">Experto</span></li>
  <li><strong>BERT / Transformers</strong> <span class="level-advanced">Avanzado</span></li>
</ul>
</div>
</div>

---

## Charlas y Eventos

<div class="projects-grid">

<div class="project-card">
<h3>Workshop Invierno 2020 &mdash; Universidad Austral de Chile</h3>
<a href="https://youtu.be/XnX3HJDSIgU?t=8905" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/YouTube-Ver_Presentación-FF0000?logo=youtube&style=for-the-badge" alt="Ver en YouTube"/></a>
<div style="text-align:justify;margin-top:12px;">Presentación sobre el viaje de transformación digital: <em>"Cómo las empresas del Sector Utility deben adoptar la Inteligencia Artificial"</em></div>
<center><img src="{{ '/images/eventoinv.jpg' | relative_url }}" alt="Workshop UACH" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>Workshop Primavera 2020 &mdash; Computación Visual</h3>
<a href="https://youtu.be/4eFyjwE3bCM?t=5576" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/YouTube-Ver_Presentación-FF0000?logo=youtube&style=for-the-badge" alt="Ver en YouTube"/></a>
<a href="https://colab.research.google.com/drive/1l4UfnvcbBgybtc16pJcIGuh41ETbXsF3" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Colab-Ejecutar_Demo-F9AB00?logo=googlecolab&style=for-the-badge" alt="Ejecutar en Colab"/></a>
<div style="text-align:justify;margin-top:12px;">Demostración práctica de computación visual con implementación en menos de 20 líneas de código.</div>
<center><img src="{{ '/images/vision.png' | relative_url }}" alt="Computer Vision Workshop" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

</div>

---

<div style="text-align:center;margin:44px 0 20px;">
  <a href="https://www.linkedin.com/in/cristian-rodriguez-546a4150" class="cta-button" target="_blank" rel="noopener">Conectar en LinkedIn</a>
  <a href="https://github.com/script32" class="cta-button secondary" target="_blank" rel="noopener">Explorar Código</a>
  <a href="https://www.kaggle.com/crprpr" class="cta-button" target="_blank" rel="noopener" style="background:linear-gradient(135deg,#20BEFF,#0088cc);">Ver en Kaggle</a>
</div>
