# Portfolio Restructure Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Rediseñar el portfolio Jekyll de single-page con sidebar fijo a multi-página full-width con navbar sticky, separando Consultor TI y Líder de Negocio TI, y añadiendo contenido ATS oculto.

**Architecture:** Cuatro tareas secuenciales: (1) Layout base compartido — prerequisito de todo; (2) Landing de bifurcación con ATS + JSON-LD; (3) Página Consultor TI con mapa Leaflet multi-país; (4) Página Líder TI con métricas y timeline. El sitio construye y despliega vía GitHub Actions en cada commit.

**Tech Stack:** Jekyll 4.3, jekyll-theme-minimal, SCSS, HTML5, CSS Grid, Leaflet.js 1.9.4, GitHub Pages.

## Global Constraints

- Jekyll 4.3, ruby 3.2, `jekyll-theme-minimal` como base
- Todo el contenido en **español**
- Wrapper: `max-width: 1200px; width: 95%; margin: 0 auto; padding: 0 20px`
- Navbar: sticky, altura 64px, fondo `rgba(0, 25, 51, 0.97)`
- Paleta Consultor TI: primario `#0066cc`, acento `#00cc66`
- Paleta Líder TI: primario `#003366`, acento `#f59e0b`
- ATS content: CSS clip technique — `position:absolute; width:1px; height:1px; clip:rect(0,0,0,0)` (NO `display:none`)
- Leaflet 1.9.4 carga **solo** en páginas con `use_map: true` en front matter
- Build local NO disponible (Ruby no instalado). Verificación: `git push` → revisar GitHub Actions en https://github.com/script32/portfolio/actions
- URL base del sitio: `https://script32.github.io/portfolio`

---

### Task 1: Layout Base — Navbar Sticky + Full-Width Wrapper

**Files:**
- Modify: `_layouts/default.html` (reescritura completa)
- Modify: `_sass/jekyll-theme-minimal.scss` (agregar overrides al final)

**Interfaces:**
- Produces: clases CSS compartidas: `.wrapper`, `.site-nav`, `main`, `.site-footer`, `.project-card`, `.project-card.featured`, `.project-card.leader`, `.projects-grid`, `.tech-stack`, `.tech-badge`, `.tech-badge.gold`, `.stats-row`, `.stat-card`, `.stat-card.gold`, `.skills-grid`, `.skill-category`, `.skill-list`, `.level-expert`, `.level-advanced`, `.industries-grid`, `.industry-card`, `.timeline`, `.timeline-item`, `.timeline-year`, `.cta-button`, `.cta-button.secondary`, `.cta-button.gold`, `.geo-hero`, `#map`, `.hero-overlay`, `.metrics-hero`, `.metrics-row`, `.metric-item`, `.metric-number`, `.metric-label`, `.landing-hero`, `.role-cards`, `.role-card`, `.role-card.consulting`, `.role-card.leader`, `.mission-statement`, `.ats-resume`
- Produces: variable `page.use_map` — cuando `true` en front matter, carga Leaflet CSS+JS
- Produces: variables `page.title` y `page.description` para SEO por página

- [ ] **Step 1: Reescribir `_layouts/default.html` completo**

Reemplazar el contenido completo del archivo con:

```html
<!DOCTYPE html>
<html lang="{{ site.lang | default: "es" }}">
  <head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <title>{{ page.title | default: site.title }}</title>
    <meta name="description" content="{{ page.description | default: 'Cristian Rodriguez - AI Strategy Consultant | Consultor de TI con 20+ años en IA, GIS y transformación digital para Utility, Gobierno, Hoteles, Automotriz y más.' }}">
    <meta name="keywords" content="Cristian Rodriguez, AI Consultant, IT Consultant, Geospatial Solutions, Smart Agent Utility, Python, Machine Learning, Data Science, PostGIS, Azure, IBM Watson, SAP, script32">
    <meta name="author" content="Cristian Rodriguez">
    <meta name="robots" content="index, follow">

    <meta property="og:type" content="website">
    <meta property="og:title" content="{{ page.title | default: site.title }}">
    <meta property="og:description" content="{{ page.description | default: 'Cristian Rodriguez - AI Strategy Consultant' }}">
    <meta property="og:url" content="{{ page.url | absolute_url }}">
    <meta property="og:site_name" content="Cristian Rodriguez Portfolio">
    <meta property="og:image" content="{{ '/images/image.jpg' | absolute_url }}">

    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:title" content="{{ page.title | default: site.title }}">
    <meta name="twitter:description" content="{{ page.description | default: 'Cristian Rodriguez - AI Strategy Consultant' }}">

    <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "Person",
      "name": "Cristian Rodriguez",
      "alternateName": "script32",
      "url": "https://script32.github.io/portfolio/",
      "image": "https://script32.github.io/portfolio/images/image.jpg",
      "jobTitle": "AI Strategy Consultant | IT Business Leader",
      "description": "Consultor de TI con 20+ años de experiencia en IA, análisis geoespacial y transformación digital",
      "sameAs": [
        "https://www.linkedin.com/in/cristian-rodriguez-546a4150/",
        "https://github.com/script32",
        "https://www.kaggle.com/crprpr"
      ],
      "hasOccupation": [
        {
          "@type": "Occupation",
          "name": "Leader of IT Business",
          "occupationLocation": { "@type": "City", "name": "Osorno, Chile" },
          "description": "Gestión de +600 proyectos de $10K a $7M USD. SCADA, STG, Movilidad 1000+ cuadrillas."
        },
        {
          "@type": "Occupation",
          "name": "AI Strategy Consultant",
          "description": "Implementaciones de IA y transformación digital en Chile, España, México, Colombia, Brasil."
        }
      ],
      "alumniOf": [
        { "@type": "EducationalOrganization", "name": "Universitat de Barcelona", "description": "MBA Finance, GPA 8.9, 2021-2022" },
        { "@type": "EducationalOrganization", "name": "Universidad Gabriela Mistral", "description": "Ingeniería en Computación, 2013-2014" },
        { "@type": "EducationalOrganization", "name": "Universidad de Los Lagos", "description": "Técnico en TI, 2001-2004" }
      ],
      "hasCredential": [
        { "@type": "EducationalOccupationalCredential", "name": "Machine Learning", "credentialCategory": "Certificate", "recognizedBy": { "@type": "Organization", "name": "MIT Professional Education" } },
        { "@type": "EducationalOccupationalCredential", "name": "Computer Vision with Watson and OpenCV", "credentialCategory": "Certificate", "recognizedBy": { "@type": "Organization", "name": "Coursera / IBM" } },
        { "@type": "EducationalOccupationalCredential", "name": "Digital Transformation for Leaders", "credentialCategory": "Certificate", "recognizedBy": { "@type": "Organization", "name": "LinkedIn Learning" } }
      ],
      "knowsAbout": [
        "Python", "C#", ".NET", "JavaScript", "TypeScript", "R", "SQL", "ABAP",
        "PostgreSQL", "PostGIS", "SQL Server", "MongoDB", "SAP HANA",
        "Microsoft Azure", "Amazon AWS", "Google Cloud Platform", "IBM Cloud", "IBM Watson",
        "TensorFlow", "PyTorch", "XGBoost", "LightGBM", "BERT", "Transformers", "Scikit-learn", "OpenCV",
        "Machine Learning", "Deep Learning", "Computer Vision", "NLP", "MLOps", "Generative AI",
        "GIS", "Geospatial Analysis", "Leaflet.js", "PostGIS", "Satellite Imagery",
        "React", "SPFx", "Angular", "Node.js", ".NET Core",
        "SAP PM", "SAP PS", "SSIS", "SharePoint", "Synchroteam",
        "Agile", "Scrum", "Digital Transformation", "IT Leadership", "Project Management",
        "Utility Sector", "Smart Grid", "SCADA", "Field Operations", "Smart Agent"
      ]
    }
    </script>

    {% seo %}
    <link rel="stylesheet" href="{{ "/assets/css/style.css?v=" | append: site.github.build_revision | relative_url }}">
    {% if page.use_map %}
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
    {% endif %}
  </head>
  <body>

    <!-- NAVBAR STICKY -->
    <nav class="site-nav">
      <a href="{{ "/" | absolute_url }}" class="nav-brand">
        {% if site.logo %}
        <img src="{{ site.logo | relative_url }}" alt="{{ site.title }}" class="nav-avatar">
        {% endif %}
        <span class="nav-title">Cristian Rodriguez</span>
      </a>
      <input type="checkbox" id="nav-toggle" class="nav-toggle">
      <label for="nav-toggle" class="nav-toggle-label" aria-label="Abrir menú">
        <span></span><span></span><span></span>
      </label>
      <div class="nav-links">
        <a href="{{ "/consulting" | relative_url }}" class="nav-link{% if page.url contains 'consulting' %} active{% endif %}">Consultor TI</a>
        <a href="{{ "/business-leader" | relative_url }}" class="nav-link{% if page.url contains 'business-leader' %} active{% endif %}">Líder TI</a>
        <a href="https://www.linkedin.com/in/cristian-rodriguez-546a4150" target="_blank" rel="noopener" class="nav-link nav-cta">Contacto</a>
      </div>
    </nav>

    <div class="wrapper">
      <main>
        {{ content }}
      </main>
    </div>

    <footer class="site-footer">
      <div class="footer-inner">
        <span>© 2025 Cristian Rodriguez · Script32 Labs · Arquitecto de Soluciones Geoespaciales</span>
        <div class="footer-links">
          <a href="https://www.linkedin.com/in/cristian-rodriguez-546a4150" target="_blank" rel="noopener">LinkedIn</a>
          <a href="https://github.com/script32" target="_blank" rel="noopener">GitHub</a>
          <a href="https://www.kaggle.com/crprpr" target="_blank" rel="noopener">Kaggle</a>
        </div>
      </div>
    </footer>

    <script src="{{ "/assets/js/scale.fix.js" | relative_url }}"></script>

    {% if page.use_map %}
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
    {% endif %}

    <style>
    /* ===== RESET CONFLICTOS DEL TEMA ===== */
    body { padding: 0 !important; }

    /* ===== WRAPPER FULL-WIDTH ===== */
    .wrapper {
      max-width: 1200px !important;
      width: 95% !important;
      margin: 0 auto !important;
      padding: 40px 20px 60px !important;
    }
    main { width: 100%; display: block; }

    /* ===== NAVBAR STICKY ===== */
    .site-nav {
      position: sticky;
      top: 0;
      z-index: 1000;
      background: rgba(0, 25, 51, 0.97);
      backdrop-filter: blur(8px);
      -webkit-backdrop-filter: blur(8px);
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 0 32px;
      height: 64px;
      box-shadow: 0 2px 16px rgba(0, 0, 0, 0.3);
    }
    .nav-brand {
      display: flex;
      align-items: center;
      gap: 12px;
      text-decoration: none;
    }
    .nav-avatar {
      width: 36px;
      height: 36px;
      border-radius: 50%;
      object-fit: cover;
      border: 2px solid #00cc66;
    }
    .nav-title {
      color: white;
      font-weight: 700;
      font-size: 1.05em;
      letter-spacing: 0.3px;
    }
    .nav-toggle { display: none; }
    .nav-toggle-label {
      display: none;
      flex-direction: column;
      gap: 5px;
      cursor: pointer;
      padding: 4px;
    }
    .nav-toggle-label span {
      display: block;
      width: 24px;
      height: 2px;
      background: white;
      border-radius: 2px;
      transition: 0.3s;
    }
    .nav-links {
      display: flex;
      align-items: center;
      gap: 6px;
    }
    .nav-link {
      color: rgba(255, 255, 255, 0.82);
      text-decoration: none;
      padding: 7px 18px;
      border-radius: 20px;
      font-size: 0.9em;
      font-weight: 500;
      transition: all 0.2s ease;
    }
    .nav-link:hover, .nav-link.active {
      color: white;
      background: rgba(255, 255, 255, 0.13);
      font-weight: 500;
    }
    .nav-cta {
      background: linear-gradient(135deg, #00cc66, #009944) !important;
      color: white !important;
      font-weight: 600 !important;
    }
    .nav-cta:hover {
      background: linear-gradient(135deg, #00aa55, #007733) !important;
      transform: translateY(-1px);
      box-shadow: 0 3px 10px rgba(0, 204, 102, 0.3);
    }

    /* ===== FOOTER ===== */
    .site-footer {
      background: #001222;
      color: rgba(255, 255, 255, 0.55);
      padding: 24px 32px;
      margin-top: 40px;
    }
    .footer-inner {
      max-width: 1200px;
      margin: 0 auto;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 12px;
      font-size: 0.82em;
    }
    .footer-links { display: flex; gap: 20px; }
    .footer-links a { color: rgba(255, 255, 255, 0.55); text-decoration: none; transition: color 0.2s; }
    .footer-links a:hover { color: #00cc66; }

    /* ===== PROJECT CARDS ===== */
    .project-card {
      background: linear-gradient(145deg, #ffffff, #f7f7f7);
      border-radius: 12px;
      padding: 28px;
      margin: 20px 0;
      box-shadow: 0 3px 14px rgba(0, 0, 0, 0.07);
      border-left: 4px solid #0066cc;
      transition: transform 0.25s ease, box-shadow 0.25s ease;
    }
    .project-card:hover { transform: translateY(-4px); box-shadow: 0 8px 28px rgba(0, 0, 0, 0.11); }
    .project-card.featured { border-left-color: #00cc66; background: linear-gradient(145deg, #f0fff7, #e6ffed); }
    .project-card.leader { border-left-color: #f59e0b; background: linear-gradient(145deg, #fffbeb, #fef3c7); }
    .project-card h3 { color: #003366; margin-top: 0; }

    /* ===== PROJECTS GRID ===== */
    .projects-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
      gap: 24px;
      margin: 28px 0;
    }
    .projects-grid .project-card { margin: 0; }

    /* ===== TECH BADGES ===== */
    .tech-stack { display: flex; flex-wrap: wrap; gap: 8px; margin: 14px 0; }
    .tech-badge { background: #e8f4fc; color: #0066cc; padding: 4px 13px; border-radius: 15px; font-size: 0.82em; font-weight: 500; }
    .tech-badge.gold { background: #fef3c7; color: #b45309; }

    /* ===== STATS ROW ===== */
    .stats-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(155px, 1fr));
      gap: 16px;
      margin: 24px 0;
    }
    .stat-card { background: white; border-radius: 10px; padding: 20px; text-align: center; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.07); }
    .stat-card .number { font-size: 2.2em; font-weight: 800; color: #0066cc; line-height: 1; }
    .stat-card .label { color: #666; font-size: 0.83em; margin-top: 7px; }
    .stat-card.gold .number { color: #f59e0b; }

    /* ===== SKILLS GRID ===== */
    .skills-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(260px, 1fr)); gap: 20px; margin: 24px 0; }
    .skill-category { background: white; border-radius: 10px; padding: 22px; box-shadow: 0 2px 10px rgba(0, 0, 0, 0.06); }
    .skill-category h4 { color: #003366; border-bottom: 2px solid #0066cc; padding-bottom: 10px; margin-top: 0; margin-bottom: 14px; }
    .skill-list { list-style: none; padding: 0; margin: 0; }
    .skill-list li { display: flex; justify-content: space-between; align-items: center; padding: 7px 0; border-bottom: 1px solid #f0f0f0; }
    .skill-list li:last-child { border-bottom: none; }
    .level-expert { background: #e6f7ed; color: #00994d; padding: 2px 10px; border-radius: 12px; font-size: 0.78em; font-weight: 600; }
    .level-advanced { background: #e6f0fa; color: #0066cc; padding: 2px 10px; border-radius: 12px; font-size: 0.78em; font-weight: 600; }

    /* ===== INDUSTRIES GRID ===== */
    .industries-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(155px, 1fr)); gap: 16px; margin: 24px 0; }
    .industry-card { background: white; border-radius: 10px; padding: 22px 16px; text-align: center; box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06); border-top: 3px solid #0066cc; transition: transform 0.2s ease; }
    .industry-card:hover { transform: translateY(-3px); }
    .industry-icon { font-size: 2.2em; margin-bottom: 10px; }
    .industry-name { font-weight: 600; color: #003366; font-size: 0.9em; }

    /* ===== TIMELINE ===== */
    .timeline { position: relative; padding-left: 36px; margin: 24px 0; }
    .timeline::before { content: ''; position: absolute; left: 0; top: 0; bottom: 0; width: 3px; background: linear-gradient(to bottom, #f59e0b, #003366); border-radius: 3px; }
    .timeline-item { position: relative; margin-bottom: 40px; }
    .timeline-item::before { content: ''; position: absolute; left: -44px; top: 8px; width: 14px; height: 14px; background: #f59e0b; border-radius: 50%; border: 3px solid white; box-shadow: 0 0 0 3px #f59e0b; }
    .timeline-item h4 { color: #003366; margin: 0 0 8px; font-size: 1.1em; }
    .timeline-item p { color: #555; margin: 0; line-height: 1.65; }
    .timeline-year { display: inline-block; background: #003366; color: #f59e0b; padding: 2px 12px; border-radius: 10px; font-size: 0.78em; font-weight: 700; margin-bottom: 10px; letter-spacing: 0.5px; }

    /* ===== CTA BUTTONS ===== */
    .cta-button { display: inline-block; background: linear-gradient(135deg, #0066cc, #004499); color: white !important; padding: 12px 28px; border-radius: 25px; text-decoration: none; font-weight: 600; transition: all 0.3s ease; margin: 5px; font-size: 0.93em; }
    .cta-button:hover { background: linear-gradient(135deg, #004499, #003366); transform: translateY(-2px); box-shadow: 0 4px 16px rgba(0, 102, 204, 0.35); }
    .cta-button.secondary { background: linear-gradient(135deg, #00cc66, #009944); }
    .cta-button.secondary:hover { background: linear-gradient(135deg, #009944, #007733); }
    .cta-button.gold { background: linear-gradient(135deg, #f59e0b, #d97706); }
    .cta-button.gold:hover { background: linear-gradient(135deg, #d97706, #b45309); }

    /* ===== GEO HERO (consulting) ===== */
    .geo-hero { position: relative; width: 100%; height: 320px; border-radius: 14px; overflow: hidden; margin-bottom: 44px; box-shadow: 0 4px 24px rgba(0, 0, 0, 0.16); }
    .geo-hero #map { width: 100%; height: 100%; }
    .hero-overlay { position: absolute; top: 0; left: 0; right: 0; bottom: 0; background: linear-gradient(135deg, rgba(0, 51, 102, 0.82) 0%, rgba(0, 102, 153, 0.62) 100%); display: flex; flex-direction: column; justify-content: center; align-items: center; color: white; text-align: center; padding: 20px; z-index: 1000; pointer-events: none; }
    .hero-overlay h1 { font-size: 2.1em; margin-bottom: 10px; text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.4); color: white; }
    .hero-overlay p { font-size: 1.1em; opacity: 0.93; max-width: 640px; }

    /* ===== METRICS HERO (business-leader) ===== */
    .metrics-hero { background: linear-gradient(135deg, #001833 0%, #003366 100%); border-radius: 14px; padding: 52px 40px; margin-bottom: 44px; color: white; text-align: center; }
    .metrics-hero h1 { color: white; font-size: 2.1em; margin-bottom: 8px; }
    .metrics-hero > p { color: rgba(255, 255, 255, 0.8); font-size: 1.1em; margin-bottom: 36px; }
    .metrics-row { display: grid; grid-template-columns: repeat(auto-fit, minmax(160px, 1fr)); gap: 20px; }
    .metric-item { background: rgba(255, 255, 255, 0.08); border-radius: 12px; padding: 22px 12px; }
    .metric-number { font-size: 2.6em; font-weight: 800; color: #f59e0b; line-height: 1; }
    .metric-label { color: rgba(255, 255, 255, 0.72); font-size: 0.85em; margin-top: 8px; }

    /* ===== LANDING BIFURCATION ===== */
    .landing-hero { text-align: center; padding: 64px 20px 44px; }
    .landing-hero .avatar { width: 116px; height: 116px; border-radius: 50%; object-fit: cover; border: 4px solid #0066cc; box-shadow: 0 4px 20px rgba(0, 102, 204, 0.22); margin-bottom: 22px; }
    .landing-hero h1 { font-size: 2.5em; color: #001833; margin-bottom: 8px; }
    .landing-hero .tagline { color: #0066cc; font-size: 1.12em; font-weight: 500; margin-bottom: 32px; }
    .landing-stats { display: flex; justify-content: center; gap: 48px; margin-bottom: 52px; flex-wrap: wrap; }
    .landing-stat .number { font-size: 2.1em; font-weight: 800; color: #003366; line-height: 1; }
    .landing-stat .label { color: #888; font-size: 0.8em; margin-top: 5px; }
    .role-cards { display: grid; grid-template-columns: 1fr 1fr; gap: 28px; max-width: 920px; margin: 0 auto 64px; }
    .role-card { border-radius: 16px; padding: 38px 32px; text-decoration: none; display: block; transition: transform 0.28s ease, box-shadow 0.28s ease; }
    .role-card:hover { transform: translateY(-7px); box-shadow: 0 14px 40px rgba(0, 0, 0, 0.14); }
    .role-card.consulting { background: linear-gradient(145deg, #eef6ff, #d6eaff); border: 2px solid #0066cc; }
    .role-card.leader { background: linear-gradient(145deg, #fef9ee, #fde9a0); border: 2px solid #f59e0b; }
    .role-icon { font-size: 2.8em; margin-bottom: 16px; }
    .role-card h2 { margin: 0 0 8px; font-size: 1.45em; }
    .role-card.consulting h2 { color: #003366; }
    .role-card.leader h2 { color: #92400e; }
    .role-meta { font-size: 0.88em; color: #555; margin-bottom: 16px; }
    .role-card p { color: #444; line-height: 1.62; font-size: 0.93em; margin-bottom: 22px; }
    .role-cta { display: inline-block; padding: 10px 22px; border-radius: 20px; font-weight: 600; font-size: 0.88em; }
    .role-card.consulting .role-cta { background: #0066cc; color: white; }
    .role-card.leader .role-cta { background: #f59e0b; color: white; }

    /* ===== MISSION STATEMENT ===== */
    .mission-statement { background: linear-gradient(135deg, #003366, #0066cc); color: white; padding: 28px 36px; border-radius: 12px; font-style: italic; font-size: 1.08em; text-align: center; margin: 28px 0; border-left: none; }

    /* ===== ATS HIDDEN CONTENT ===== */
    .ats-resume { position: absolute; width: 1px; height: 1px; padding: 0; margin: -1px; overflow: hidden; clip: rect(0, 0, 0, 0); white-space: nowrap; border: 0; }

    /* ===== GITHUB STATS (existente) ===== */
    .github-stats { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 15px; margin: 20px 0; }

    /* ===== RESPONSIVE ===== */
    @media (max-width: 768px) {
      .nav-toggle-label { display: flex; }
      .nav-links {
        display: none;
        position: absolute;
        top: 64px;
        left: 0;
        right: 0;
        background: rgba(0, 25, 51, 0.98);
        flex-direction: column;
        padding: 16px 20px;
        gap: 4px;
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.3);
      }
      .nav-toggle:checked ~ .nav-links { display: flex; }
      .nav-link { padding: 10px 16px; border-radius: 8px; }
      .role-cards { grid-template-columns: 1fr; max-width: 100%; }
      .projects-grid { grid-template-columns: 1fr; }
      .metrics-hero { padding: 36px 20px; }
      .metrics-row { grid-template-columns: repeat(2, 1fr); }
      .landing-hero { padding: 40px 16px 32px; }
      .landing-hero h1 { font-size: 1.9em; }
      .landing-stats { gap: 28px; }
      .geo-hero { height: 260px; }
      .hero-overlay h1 { font-size: 1.5em; }
      .wrapper { padding: 20px 14px 40px !important; }
      .site-nav { padding: 0 20px; }
      .footer-inner { flex-direction: column; text-align: center; gap: 16px; }
      .skills-grid { grid-template-columns: 1fr; }
    }

    @media (max-width: 480px) {
      .metrics-row { grid-template-columns: repeat(2, 1fr); }
      .industries-grid { grid-template-columns: repeat(2, 1fr); }
      .stats-row { grid-template-columns: repeat(2, 1fr); }
    }
    </style>

    {% if site.google_analytics %}
    <script>
      (function(i,s,o,g,r,a,m){i['GoogleAnalyticsObject']=r;i[r]=i[r]||function(){
      (i[r].q=i[r].q||[]).push(arguments)},i[r].l=1*new Date();a=s.createElement(o),
      m=s.getElementsByTagName(o)[0];a.async=1;a.src=g;m.parentNode.insertBefore(a,m)
      })(window,document,'script','https://www.google-analytics.com/analytics.js','ga');
      ga('create', '{{ site.google_analytics }}', 'auto');
      ga('send', 'pageview');
    </script>
    {% endif %}
  </body>
</html>
```

- [ ] **Step 2: Añadir overrides al FINAL de `_sass/jekyll-theme-minimal.scss`**

Agregar estas líneas al final del archivo (después de todos los `@media` existentes):

```scss
/* ===== PORTFOLIO OVERRIDES - neutralizar sidebar fijo del tema ===== */
body { padding: 0 !important; }
header { display: none !important; }
footer { display: none !important; }
section {
  width: 100% !important;
  float: none !important;
  padding-bottom: 0 !important;
  border: none !important;
}
.wrapper {
  width: 100% !important;
  max-width: none !important;
  margin: 0 !important;
}
```

- [ ] **Step 3: Commit y verificar build en GitHub Pages**

```bash
git add _layouts/default.html _sass/jekyll-theme-minimal.scss
git commit -m "feat: replace fixed sidebar with sticky navbar and full-width 1200px layout"
git push origin master
```

Abrir https://github.com/script32/portfolio/actions y esperar que el workflow "pages build and deployment" termine con ✅.

Luego abrir https://script32.github.io/portfolio/ y verificar:
- Navbar oscura sticky visible en la parte superior con logo + "Consultor TI | Líder TI | Contacto"
- Contenido ocupa el ancho completo (no 500px centrados)
- En mobile (DevTools → responsive): navbar colapsa, hamburger visible

---

### Task 2: Landing Page — Bifurcación de Roles + ATS + JSON-LD

**Files:**
- Modify: `index.md` (reescritura completa)

**Interfaces:**
- Consumes: `.landing-hero`, `.avatar`, `.tagline`, `.landing-stats`, `.landing-stat`, `.role-cards`, `.role-card.consulting`, `.role-card.leader`, `.role-icon`, `.role-meta`, `.role-cta`, `.mission-statement`, `.ats-resume` (todos definidos en Task 1)
- Consumes: `/images/image.jpg` (foto de perfil existente)
- Produces: landing page en `/` con bifurcación hacia `/consulting` y `/business-leader`

- [ ] **Step 1: Reescribir `index.md` completo**

```markdown
---
layout: default
title: Cristian Rodriguez - AI Strategy Consultant
description: "Consultor de TI con 20+ años implementando IA, soluciones geoespaciales y transformación digital en 5 países y 6+ industrias. Líder de Negocio TI con 600+ proyectos ejecutados."
---

<!-- HERO LANDING -->
<div class="landing-hero">
  <img src="{{ '/images/image.jpg' | relative_url }}" alt="Cristian Rodriguez" class="avatar">
  <h1>Cristian Rodriguez</h1>
  <p class="tagline">AI Strategy Consultant &middot; Arquitecto de Soluciones Geoespaciales</p>
  <div class="landing-stats">
    <div class="landing-stat">
      <div class="number">20+</div>
      <div class="label">Años de experiencia</div>
    </div>
    <div class="landing-stat">
      <div class="number">600+</div>
      <div class="label">Proyectos ejecutados</div>
    </div>
    <div class="landing-stat">
      <div class="number">5</div>
      <div class="label">Países</div>
    </div>
    <div class="landing-stat">
      <div class="number">6+</div>
      <div class="label">Industrias</div>
    </div>
  </div>
</div>

<!-- BIFURCACIÓN DE ROLES -->
<div class="role-cards">
  <a href="{{ '/consulting' | relative_url }}" class="role-card consulting">
    <div class="role-icon">🌐</div>
    <h2>Consultor de TI</h2>
    <p class="role-meta">2007 &ndash; presente &middot; Chile · España · México · Colombia · Brasil</p>
    <p>Implementaciones de IA, soluciones geoespaciales y transformación digital para clientes en Utility, Hoteles, Gobierno, Automotriz, Alimentos y más. Proyectos con ROI de hasta $9M USD.</p>
    <span class="role-cta">Ver experiencia de consultoría &rarr;</span>
  </a>
  <a href="{{ '/business-leader' | relative_url }}" class="role-card leader">
    <div class="role-icon">📊</div>
    <h2>Líder de Negocio TI</h2>
    <p class="role-meta">2017 &ndash; presente &middot; Grupo Saesa &middot; Sector Eléctrico</p>
    <p>Gestión de equipos, presupuesto y roadmap tecnológico. Más de 600 proyectos de $10K a $7M USD integrando tecnología en operaciones eléctricas de generación, transmisión y distribución.</p>
    <span class="role-cta">Ver liderazgo TI &rarr;</span>
  </a>
</div>

<blockquote class="mission-statement">
  "En un mundo donde la infraestructura crítica genera terabytes de datos espaciales, mi misión es transformar esa información en acción operativa."
</blockquote>

<!-- ATS HIDDEN: visible para bots de reclutamiento, invisible para usuarios -->
<div class="ats-resume" aria-hidden="false">
CRISTIAN RODRIGUEZ
AI Strategy Consultant | IT Business Leader | Geospatial Solutions Architect
Osorno, Los Lagos, Chile
LinkedIn: linkedin.com/in/cristian-rodriguez-546a4150
GitHub: github.com/script32 | Kaggle: kaggle.com/crprpr

EXPERIENCIA LABORAL

Grupo Saesa — Leader of IT Business
Febrero 2017 – Presente (9+ años) | Osorno, Chile
Responsabilidades: Liderazgo de transformación digital con IA, Data Science y tecnologías geoespaciales. Gestión de más de 600 proyectos con presupuestos entre USD 10.000 y USD 7.000.000. Gestión de equipos TI, presupuesto anual, roadmap tecnológico y relación con el negocio. Construcción de casos de negocio para inversiones tecnológicas. Proyectos emblematicos: SCADA Distribuidora, Sistema Técnico Geográfico (STG) para distribuidora y transmisora eléctrica, Movilidad y Digitalización para más de 1.000 cuadrillas de terreno. Implementación de modelos predictivos XGBoost en IBM Watson (más de 12 millones de predicciones generadas, 85% de precisión). Análisis de vegetación satelital en 11.500 km de líneas eléctricas (ROI $9 millones USD). Inspección de activos con drones Optelos/Neurala/Azure (más de 300.000 imágenes procesadas). Gestión de fuerza de trabajo con Synchroteam para más de 300 brigadas.

Grupo Saesa — Service Quality Engineer
Enero 2011 – Enero 2017 (6 años) | Osorno, Chile
Integración de tecnologías geográficas y móviles en operaciones de campo. Implementación de sistemas GIS para distribución eléctrica. Gestión de calidad de servicio eléctrico.

CLAS — Head of IT Department
Enero 2007 – Diciembre 2010 (4 años) | Chile
Administración de ERP, gestión de hardware y desarrollo de aplicaciones móviles. Implementación de sistemas de guía vehicular para Parque Arauco y Clínica las Condes.

CONSULTORÍA INTERNACIONAL DE TI — 2007 – Presente
Países: Chile, España, México, Colombia, Brasil
Industrias: Utility eléctrico (distribución, transmisión, generación), Hoteles y Turismo, Automotriz, Gobierno, Alimentos, Salud
Actividades: Implementaciones técnicas y arquitectura de soluciones para clientes empresariales. Implementación de Inteligencia Artificial y Machine Learning en procesos de negocio. Desarrollo de soluciones geoespaciales y análisis de datos a escala. Consultoría en transformación digital. Implementación de sistemas SAP PM, SAP PS, SSIS, SharePoint, ERP.

EDUCACIÓN

MBA Finance — Universitat de Barcelona
2021 – 2022 | GPA: 8.9 sobre 10

Ingeniería en Computación — Universidad Gabriela Mistral
2013 – 2014

Técnico en Tecnologías de Información — Universidad de Los Lagos
2001 – 2004

CERTIFICACIONES Y CURSOS

Machine Learning — MIT Professional Education (Diciembre 2019)
Computer Vision with Watson and OpenCV — Coursera / IBM (Julio 2019)
Digital Transformation for Leaders — LinkedIn Learning (Septiembre 2020)
Data Science Foundations Level 1 — IBM (Junio 2018)
IBM Bluemix Essentials — IBM
Watson Conversation Service — IBM
Node-RED de básico a intermedio — IBM
CS224n: Natural Language Processing with Deep Learning — Stanford University (Auditoría)

HABILIDADES TÉCNICAS

Lenguajes de Programación: Python, C#, .NET, JavaScript, TypeScript, R, SQL, ABAP
Bases de Datos: PostgreSQL, PostGIS, SQL Server, MySQL, MongoDB, SAP HANA
Plataformas Cloud: Microsoft Azure, Amazon AWS, Google Cloud Platform, IBM Cloud, IBM Watson
Inteligencia Artificial y Machine Learning: TensorFlow, PyTorch, XGBoost, LightGBM, BERT, Transformers, Scikit-learn, OpenCV, NLTK, Keras, Deep Learning, Computer Vision, NLP, MLOps, Generative AI, LLM
GIS y Geoespacial: PostGIS, Leaflet.js, QGIS, Sistemas de Información Geográfica, Análisis Satelital, Drones, Optelos, Neurala AI
Frameworks Web: React, SPFx SharePoint, Angular, Node.js, .NET Core, FastAPI, Jekyll
DevOps: Docker, GitHub Actions, CI/CD, Azure DevOps, IBM DevOps
Sistemas Empresariales: SAP PM, SAP PS, SAP HANA, SSIS, Node-RED, Synchroteam, SharePoint
Metodologías: Agile, Scrum, Gestión de Proyectos TI, Casos de Negocio, Transformación Digital, DevSecOps

ROLES PROFESIONALES: AI Strategy Consultant, IT Consultant, IT Business Leader, Data Scientist, Solutions Architect, Geospatial Engineer, MLOps Engineer, Product Manager AI, Digital Transformation Lead, FullStack Senior Developer, Python Developer, Machine Learning Engineer, Business Intelligence Analyst, ERP Consultant, SAP Consultant, Field Operations Specialist
</div>
```

- [ ] **Step 2: Commit y verificar**

```bash
git add index.md
git commit -m "feat: rewrite landing page with role bifurcation and ATS hidden content"
git push origin master
```

Verificar en https://script32.github.io/portfolio/:
- Foto de perfil circular con borde azul
- Nombre + tagline en el centro
- 4 stats: 20+, 600+, 5, 6+
- Dos cards grandes lado a lado (azul izquierda, dorado derecha)
- Mission statement en gradiente azul al fondo
- En DevTools → Elements: buscar `class="ats-resume"` y verificar que el texto completo está en el DOM

---

### Task 3: Consulting Page — Mapa Multi-País + Todos los Proyectos

**Files:**
- Create: `consulting.md`

**Interfaces:**
- Consumes: `use_map: true` en front matter (activa Leaflet desde Task 1)
- Consumes: `.geo-hero`, `#map`, `.hero-overlay`, `.industries-grid`, `.industry-card`, `.project-card`, `.project-card.featured`, `.projects-grid`, `.tech-badge`, `.tech-stack`, `.skills-grid`, `.skill-list`, `.level-expert`, `.level-advanced`, `.stats-row`, `.stat-card`, `.cta-button`, `.cta-button.secondary`, `.mission-statement` (definidos en Task 1)
- Consumes: todas las imágenes en `/images/` (existentes: Arbol.jpeg, xgboost.jpg, Optelos.jpeg, sync.jpg, SSIS sources-final.png, sharepoint.png, sap.jpg, face.png, nlp.png, BERT-classification.png, ML.jpg, eventoinv.jpg, vision.png, arquitectura.jpg)
- Produces: página en `/consulting` con todo el contenido de consultoría

- [ ] **Step 1: Crear `consulting.md`**

```markdown
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
    { coords: [-33.45, -70.65], label: 'Chile', color: '#00cc66', primary: true },
    { coords: [40.41, -3.70],   label: 'España', color: '#00aaff', primary: false },
    { coords: [19.43, -99.13],  label: 'México', color: '#00aaff', primary: false },
    { coords: [4.71, -74.07],   label: 'Colombia', color: '#00aaff', primary: false },
    { coords: [-23.55, -46.63], label: 'Brasil', color: '#00aaff', primary: false }
  ];

  locations.forEach(function(loc) {
    L.circleMarker(loc.coords, {
      radius: loc.primary ? 10 : 8,
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
Los sistemas actuales en empresas de servicios públicos operan en <strong>silos de datos</strong>. Los trabajadores de campo carecen de inteligencia en tiempo real, dependiendo de información desactualizada que llega con horas o días de retraso.
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
<h3>Análisis de Vegetación con IA — ROI: $9M USD</h3>
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
<h3>Modelo Predictivo XGBoost — 12M+ Predicciones</h3>
<div style="text-align: justify">
Modelo <strong>XGBoost</strong> desplegado en IBM Watson que predice el tiempo de reposición del servicio eléctrico en los primeros 15 minutos de una llamada. Integra datos de clima, ubicación y tiempos históricos.
</div>
<div class="stats-row" style="margin:16px 0;">
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
<h3>Análisis de Activos con Drones — 300K+ Imágenes</h3>
<div style="text-align: justify">
Implementación de <strong>Optelos</strong> con cerebros de IA entrenados para inspección de activos eléctricos. Integra tecnología Neurala con Microsoft Azure, gestionando más de <strong>300.000 imágenes</strong>.
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
<h3>Gestión de Fuerza de Trabajo — 300+ Brigadas</h3>
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
<h3>Librería SSIS SAP CO — ETL Avanzado</h3>
<div style="text-align: justify">
Librería en <strong>C#</strong> para interacción con tablas SAP desde SSIS. Exploración de tablas, generación de consultas optimizadas y JOINs complejos sin código extenso.
</div>
<div class="tech-stack">
  <span class="tech-badge">C#</span>
  <span class="tech-badge">SSIS</span>
  <span class="tech-badge">SAP RFC</span>
  <span class="tech-badge">SQL Server</span>
</div>
<center><img src="{{ '/images/SSIS sources-final.png' | relative_url }}" alt="SSIS SAP" style="max-width:100%;border-radius:8px;margin-top:14px;"/></center>
</div>

<div class="project-card">
<h3>SharePoint — Gestión del Conocimiento con React SPFx</h3>
<div style="text-align: justify">
Plataforma de gestión del conocimiento para empresa multinacional usando <strong>React</strong> y <strong>SPFx</strong>. Seguridad basada en roles y búsqueda avanzada con Graph API.
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
Liderazgo en implementaciones de <strong>SAP PM</strong> (Plant Maintenance) y <strong>SAP PS</strong> (Project Systems) con integraciones avanzadas: liquidación de activos, valoración de presupuestos, planes de mantenimiento, integración C4C y OMS.
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

## Data Science & Machine Learning

<div class="projects-grid">

<div class="project-card">
<h3>COVID-19 Research Challenge — Kaggle</h3>
<a href="https://www.kaggle.com/crprpr/vaccine-data-filter" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Kaggle-Ver_Notebook-20BEFF?logo=kaggle&style=for-the-badge" alt="Ver Notebook"/></a>
<div style="text-align:justify;margin-top:12px;">Búsqueda de metadatos en publicaciones médicas usando <strong>NLP</strong> para encontrar relaciones en condiciones afectadas por el virus.</div>
<div class="tech-stack"><span class="tech-badge">Python</span><span class="tech-badge">NLTK</span><span class="tech-badge">NLP</span></div>
</div>

<div class="project-card">
<h3>Predicción de Demanda Energética</h3>
<a href="https://colab.research.google.com/drive/1gVBN1qg3ajEjxJPkd_YwXb9yLDBMQAR0" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Colab-Ejecutar_Notebook-F9AB00?logo=googlecolab&style=for-the-badge" alt="Colab"/></a>
<div style="text-align:justify;margin-top:12px;">Modelo <strong>LightGBM</strong> para predecir rendimiento energético según tipo de instalación y datos climáticos.</div>
<div class="tech-stack"><span class="tech-badge">LightGBM</span><span class="tech-badge">Weather Data</span><span class="tech-badge">Regression</span></div>
</div>

<div class="project-card">
<h3>Reconocimiento Facial — API + Web Platform</h3>
<a href="https://github.com/script32/face" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Ver_Repositorio-181717?logo=github&logoColor=white&style=for-the-badge" alt="GitHub"/></a>
<div style="text-align:justify;margin-top:12px;">Sistema completo con API REST y plataforma web. Usa <strong>PostgreSQL + CUDA</strong> para embeddings faciales de alto rendimiento.</div>
<div class="tech-stack"><span class="tech-badge">Python</span><span class="tech-badge">PostgreSQL</span><span class="tech-badge">CUDA</span><span class="tech-badge">REST API</span></div>
<center><img src="{{ '/images/face.png' | relative_url }}" alt="Face Recognition" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>CS224n: NLP con Deep Learning — Stanford</h3>
<a href="https://github.com/script32/CS224n-NLP" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Ver_Repositorio-181717?logo=github&logoColor=white&style=for-the-badge" alt="GitHub"/></a>
<div style="text-align:justify;margin-top:12px;">Implementación del curso de Stanford. <strong>NMT</strong> español-inglés con encoder LSTM bidireccional y decoder con atención multiplicativa.</div>
<div class="tech-stack"><span class="tech-badge">Deep Learning</span><span class="tech-badge">NLP</span><span class="tech-badge">LSTM</span><span class="tech-badge">Attention</span></div>
<center><img src="{{ '/images/nlp.png' | relative_url }}" alt="NLP Stanford" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>Análisis de Sentimiento — Detección de Toxicidad</h3>
<a href="https://www.kaggle.com/crprpr/clasificaci-n-de-texto-tensorflow-transformers" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Kaggle-Ver_Notebook-20BEFF?logo=kaggle&style=for-the-badge" alt="Kaggle"/></a>
<div style="text-align:justify;margin-top:12px;">Modelos de ML para identificar toxicidad en conversaciones online usando <strong>BERT</strong> y Transformers.</div>
<div class="tech-stack"><span class="tech-badge">BERT</span><span class="tech-badge">TensorFlow</span><span class="tech-badge">Transformers</span></div>
<center><img src="{{ '/images/BERT-classification.png' | relative_url }}" alt="BERT" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>Biblioteca de Modelos ML — Python &amp; R</h3>
<a href="https://github.com/script32/Modelos-ML" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/GitHub-Ver_Repositorio-181717?logo=github&logoColor=white&style=for-the-badge" alt="GitHub"/></a>
<div style="text-align:justify;margin-top:12px;">Repositorio con ejemplos de todos los tipos de modelos de Machine Learning en <strong>Python</strong> y <strong>R</strong> con datasets de ejemplo.</div>
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
<h3>Workshop Invierno 2020 — Universidad Austral de Chile</h3>
<a href="https://youtu.be/XnX3HJDSIgU?t=8905" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/YouTube-Ver_Presentación-FF0000?logo=youtube&style=for-the-badge" alt="YouTube"/></a>
<div style="text-align:justify;margin-top:12px;"><em>"Cómo las empresas del Sector Utility deben adoptar la Inteligencia Artificial"</em> — Presentación sobre el viaje de transformación digital.</div>
<center><img src="{{ '/images/eventoinv.jpg' | relative_url }}" alt="Workshop UACH" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

<div class="project-card">
<h3>Workshop Primavera 2020 — Computación Visual</h3>
<a href="https://youtu.be/4eFyjwE3bCM?t=5576" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/YouTube-Ver_Presentación-FF0000?logo=youtube&style=for-the-badge" alt="YouTube"/></a>
<a href="https://colab.research.google.com/drive/1l4UfnvcbBgybtc16pJcIGuh41ETbXsF3" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/Colab-Ejecutar_Demo-F9AB00?logo=googlecolab&style=for-the-badge" alt="Colab"/></a>
<div style="text-align:justify;margin-top:12px;">Demostración práctica de computación visual implementada en menos de 20 líneas de código.</div>
<center><img src="{{ '/images/vision.png' | relative_url }}" alt="Computer Vision Workshop" style="max-width:100%;border-radius:8px;margin-top:12px;"/></center>
</div>

</div>

---

<div style="text-align:center;margin:44px 0 20px;">
  <a href="https://www.linkedin.com/in/cristian-rodriguez-546a4150" class="cta-button" target="_blank" rel="noopener">Conectar en LinkedIn</a>
  <a href="https://github.com/script32" class="cta-button secondary" target="_blank" rel="noopener">Explorar Código</a>
  <a href="https://www.kaggle.com/crprpr" class="cta-button" target="_blank" rel="noopener" style="background:linear-gradient(135deg,#20BEFF,#0088cc);">Ver en Kaggle</a>
</div>
```

- [ ] **Step 2: Commit y verificar**

```bash
git add consulting.md
git commit -m "feat: add consulting page with multi-country Leaflet map and all projects"
git push origin master
```

Verificar en https://script32.github.io/portfolio/consulting:
- Mapa interactivo oscuro con 5 markers (Chile verde, otros azules) y tooltips permanentes
- Grid de 6 industrias
- Proyectos en grid 2-3 columnas (no lista vertical)
- Skills grid 4 columnas
- Navbar muestra "Consultor TI" en estado activo

---

### Task 4: Business Leader Page — Métricas + Timeline de Proyectos

**Files:**
- Create: `business-leader.md`

**Interfaces:**
- Consumes: `.metrics-hero`, `.metrics-row`, `.metric-item`, `.metric-number`, `.metric-label`, `.project-card.leader`, `.timeline`, `.timeline-item`, `.timeline-year`, `.tech-badge.gold`, `.cta-button.gold` (definidos en Task 1)
- Produces: página en `/business-leader` con liderazgo TI de Grupo Saesa

- [ ] **Step 1: Crear `business-leader.md`**

```markdown
---
layout: default
title: Líder de Negocio TI - Cristian Rodriguez
description: "9+ años como Leader of IT Business en Grupo Saesa. Más de 600 proyectos de $10K a $7M USD. SCADA, Sistema Técnico Geográfico, Movilidad 1000+ cuadrillas."
---

<!-- HERO DE MÉTRICAS -->
<div class="metrics-hero">
  <h1>Líder de Negocio TI</h1>
  <p>Integrando tecnología y negocio en el sector eléctrico desde 2017 · Grupo Saesa</p>
  <div class="metrics-row">
    <div class="metric-item">
      <div class="metric-number">600+</div>
      <div class="metric-label">Proyectos ejecutados</div>
    </div>
    <div class="metric-item">
      <div class="metric-number">$7M</div>
      <div class="metric-label">Proyecto más grande (USD)</div>
    </div>
    <div class="metric-item">
      <div class="metric-number">1.000+</div>
      <div class="metric-label">Cuadrillas digitalizadas</div>
    </div>
    <div class="metric-item">
      <div class="metric-number">9+</div>
      <div class="metric-label">Años en el rol</div>
    </div>
  </div>
</div>

---

## El Rol: Leader of IT Business

<div class="project-card leader">

<h3>Grupo Saesa · Febrero 2017 – Presente</h3>

<div style="text-align: justify">
Como <strong>Leader of IT Business</strong> en el Grupo Saesa —empresa eléctrica con operaciones de generación, transmisión, distribución y comercialización— lidero la convergencia entre tecnología y negocio. Mi rol abarca la gestión integral del portafolio TI de la empresa.
</div>

**Responsabilidades clave:**
- Definición del **roadmap tecnológico** alineado a la estrategia corporativa
- Gestión de **equipos multidisciplinarios** de TI y proveedores
- Construcción de **casos de negocio** para inversiones de $10K a $7M USD
- Relación directa con el negocio: operaciones de campo, distribución, transmisión, comercial
- Liderazgo de **transformación digital** con IA, Data Science y tecnologías geoespaciales
- Supervisión de más de **600 proyectos** ejecutados entre 2017 y la actualidad

<div class="tech-stack" style="margin-top:20px;">
  <span class="tech-badge gold">SAP PM / SAP PS</span>
  <span class="tech-badge gold">Azure DevOps</span>
  <span class="tech-badge gold">Agile / Scrum</span>
  <span class="tech-badge gold">Power BI</span>
  <span class="tech-badge gold">Microsoft Azure</span>
  <span class="tech-badge gold">IBM Watson</span>
</div>

</div>

---

## Proyectos Emblemáticos

<div class="timeline">

<div class="timeline-item">
  <span class="timeline-year">2018 – 2020</span>
  <h4>SCADA Distribuidora</h4>
  <p>Implementación del sistema de <strong>Supervisory Control and Data Acquisition</strong> para la red de distribución eléctrica. El sistema integra sensores en terreno, centros de control y sistemas de gestión de red, permitiendo supervisión y control en tiempo real de la infraestructura eléctrica. Proyecto de alta criticidad para la continuidad del servicio.</p>
</div>

<div class="timeline-item">
  <span class="timeline-year">2019 – 2022</span>
  <h4>Sistema Técnico Geográfico (STG) — Distribuidora y Transmisora</h4>
  <p>Plataforma <strong>GIS corporativa</strong> que centraliza toda la información geoespacial de los activos de distribución y transmisión eléctrica. Integra información de líneas, transformadores, subestaciones y equipos con sus coordenadas precisas. Habilitador clave para proyectos de IA sobre la red, incluyendo el análisis de vegetación satelital y la planificación de mantenimiento preventivo.</p>
</div>

<div class="timeline-item">
  <span class="timeline-year">2020 – 2023</span>
  <h4>Movilidad y Digitalización — 1.000+ Cuadrillas de Terreno</h4>
  <p>Proyecto de <strong>transformación digital de las operaciones de campo</strong> para más de 1.000 cuadrillas de trabajo en terreno. Implementación de aplicaciones móviles, digitalización de formularios y procesos de trabajo, integración con sistemas GIS y SAP PM, y gestión de la fuerza de trabajo en tiempo real. Eliminó los procesos en papel y redujo significativamente los tiempos de reporte y coordinación.</p>
</div>

</div>

---

## Escala del Portafolio

<div class="stats-row">
  <div class="stat-card gold">
    <div class="number">$10K</div>
    <div class="label">Proyecto mínimo</div>
  </div>
  <div class="stat-card gold">
    <div class="number">$7M</div>
    <div class="label">Proyecto máximo (USD)</div>
  </div>
  <div class="stat-card">
    <div class="number">600+</div>
    <div class="label">Proyectos totales</div>
  </div>
  <div class="stat-card">
    <div class="number">15+</div>
    <div class="label">Años en Grupo Saesa</div>
  </div>
</div>

<div style="text-align: justify; margin: 24px 0;">
Cada proyecto requiere la construcción de un <strong>caso de negocio</strong> que justifique la inversión tecnológica en términos de impacto operacional, reducción de costos o mejora de la calidad del servicio. Esta práctica ha forjado una visión única que combina profundidad técnica con comprensión del negocio eléctrico.
</div>

---

## Industria: Sector Eléctrico

<div class="industries-grid" style="grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));">
  <div class="industry-card" style="border-top-color:#f59e0b;">
    <div class="industry-icon">⚡</div>
    <div class="industry-name">Distribución Eléctrica</div>
  </div>
  <div class="industry-card" style="border-top-color:#f59e0b;">
    <div class="industry-icon">🔌</div>
    <div class="industry-name">Transmisión</div>
  </div>
  <div class="industry-card" style="border-top-color:#f59e0b;">
    <div class="industry-icon">🏭</div>
    <div class="industry-name">Generación</div>
  </div>
  <div class="industry-card" style="border-top-color:#f59e0b;">
    <div class="industry-icon">🛒</div>
    <div class="industry-name">Comercialización</div>
  </div>
</div>

---

<div style="text-align:center;margin:44px 0 20px;">
  <a href="https://www.linkedin.com/in/cristian-rodriguez-546a4150" class="cta-button gold" target="_blank" rel="noopener">Conectar en LinkedIn</a>
  <a href="{{ '/consulting' | relative_url }}" class="cta-button secondary">Ver experiencia de consultoría</a>
</div>
```

- [ ] **Step 2: Commit y verificar**

```bash
git add business-leader.md
git commit -m "feat: add business-leader page with metrics hero and emblematic projects timeline"
git push origin master
```

Verificar en https://script32.github.io/portfolio/business-leader:
- Hero oscuro con gradiente azul marino y 4 métricas en dorado
- Card del rol con borde dorado
- Timeline de 3 proyectos con marcadores y años
- Stats row con colores dorados
- Navbar muestra "Líder TI" en estado activo
- CTA buttons en dorado y verde
