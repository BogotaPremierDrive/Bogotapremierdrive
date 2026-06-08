# 🏎 BPD — Bogotá Premier Drive
## Terminal Titan 4.0 · Vitrina Pública PWA

> *"El Asfalto es Opcional. La Adrenalina No."*

[![PWA Ready](https://img.shields.io/badge/PWA-Ready-E10600?style=flat-square&logo=googlechrome&logoColor=white)](https://web.dev/progressive-web-apps/)
[![Three.js](https://img.shields.io/badge/Three.js-r128-C9A84C?style=flat-square)](https://threejs.org/)
[![Licencia](https://img.shields.io/badge/Licencia-Privado-3A3A52?style=flat-square)]()
[![Versión](https://img.shields.io/badge/Versión-4.0.0-EDEBE2?style=flat-square)]()

---

## Índice

1. [Descripción del Proyecto](#1-descripción-del-proyecto)
2. [Arquitectura de Archivos](#2-arquitectura-de-archivos)
3. [Stack Tecnológico](#3-stack-tecnológico)
4. [Sistema de Diseño](#4-sistema-de-diseño)
5. [Módulos y Funcionalidades](#5-módulos-y-funcionalidades)
6. [PWA — manifest.json y Service Worker](#6-pwa--manifestjson-y-service-worker)
7. [Instalación y Desarrollo Local](#7-instalación-y-desarrollo-local)
8. [Despliegue en Producción](#8-despliegue-en-producción)
9. [Datos e Inventario](#9-datos-e-inventario)
10. [Responsividad y Mobile](#10-responsividad-y-mobile)
11. [Referencias de Diseño](#11-referencias-de-diseño)
12. [Roadmap](#12-roadmap)
13. [Créditos](#13-créditos)

---

## 1. Descripción del Proyecto

**Bogotá Premier Drive (BPD)** es una vitrina pública premium de vehículos usados de alto rendimiento, orientada al segmento de lujo en Bogotá, Colombia. La interfaz **Terminal Titan 4.0** es la cara pública del ecosistema BPD: una Progressive Web App (PWA) de una sola página que combina estética cinemática de marcas como Rolls-Royce y Aston Martin con la energía visual de videojuegos como *Need for Speed*, *Forza Horizon* y *Gran Turismo*.

### Propuesta de Valor

| Problema del mercado | Solución BPD |
|---|---|
| 8 meses de espera en importación | Entrega inmediata con stock activo |
| Opacidad en historia del vehículo | Validación RUNT en 0.8 segundos |
| Fotografía de baja calidad en clasificados | Render 4K con Nano Banana Pro |
| Valoración subjetiva | Algoritmo de precios en tiempo real |
| Experiencia de compra genérica | Protocolo Titan 4.0 — 200 puntos de peritaje |

---

## 2. Arquitectura de Archivos

```
bpd/
│
├── vitrina.html              # ← Punto de entrada principal (este archivo)
│
├── manifest.json             # PWA manifest (generado dinámicamente en dev,
│                             #   archivo estático en producción)
│
├── sw.js                     # Service Worker (extraer de SW_CODE en prod)
│
├── css/
│   └── vitrina.css           # Estilos responsivos, breakpoints, efectos táctiles
│
├── js/
│   ├── vitrina.js            # Lógica de interactividad (menú, tap events, RPM)
│   └── data.js               # Inventario de vehículos (estado: PUBLISHED / IN_INSPECTION)
│
├── assets/
│   ├── icons/                # Iconos PWA (72px → 512px), generados por Canvas API
│   ├── screenshots/          # Screenshots para el instalador PWA (wide / narrow)
│   └── video/                # (Opcional) Videos locales para Racing Films section
│
└── README.md                 # Este archivo
```

> **Nota de arquitectura:** En la versión de artefacto único (`vitrina.html`), el `manifest.json` y el `sw.js` se inyectan como Blob URLs en runtime. Para producción, deben ser archivos estáticos servidos desde la raíz del dominio.

---

## 3. Stack Tecnológico

### Core
| Tecnología | Versión | Uso |
|---|---|---|
| HTML5 | — | Estructura semántica de la vitrina |
| CSS3 | — | Animaciones, Grid, Custom Properties, Glassmorphism |
| JavaScript (ES2020) | — | Interactividad, Three.js, PWA, Blob URLs |
| Three.js | r128 | Hero 3D — partículas, tori, cubo scanner |

### Tipografía (Google Fonts)
| Familia | Pesos | Rol |
|---|---|---|
| **Bebas Neue** | 400 | Títulos principales, números grandes |
| **Orbitron** | 400 · 700 · 900 | UI gaming, etiquetas HUD, menú móvil |
| **Barlow Condensed** | 200 · 300 · 400 · 600 · 700 | Cuerpo de texto, subtítulos |
| **JetBrains Mono** | 300 · 400 · 700 | Telemetría, datos técnicos, monoespaciado |

### PWA
| API | Uso |
|---|---|
| `ServiceWorker` | Cache-First / Network-First / Offline fallback |
| `CacheStorage` | Precache de assets estáticos |
| `Push API` | Notificaciones de nuevas unidades |
| `Background Sync` | Cola de formularios de contacto offline |
| `BeforeInstallPrompt` | Banner de instalación (A2HS) |
| `Canvas API` | Generación dinámica de iconos PWA |

---

## 4. Sistema de Diseño

### Paleta de Colores

```css
/* ── Fondos ── */
--ob:     #07070A;   /* Obsidian · fondo principal */
--gr:     #10101A;   /* Graphite · secciones alternas */
--gr2:    #18182A;   /* Graphite 2 · tarjetas */
--gr3:    #1E1E2E;   /* Graphite 3 · specs inner */

/* ── Acentos ── */
--gold:   #C9A84C;   /* Champagne Gold · acento principal */
--gold-l: #E4C060;   /* Gold Light · glitch layer */
--red:    #E10600;   /* GT Red · acento racing (Gran Turismo) */

/* ── Texto ── */
--txt:    #EDEBE2;   /* Warm White · texto principal */
--muted:  #7A7A92;   /* Slate · texto secundario */
--dim:    #3A3A52;   /* Dim · etiquetas, separadores */
--ok:     #3DE87A;   /* Racing Green · estados OK / RUNT sync */
```

### Tokens de Fuente

```css
--fd: 'Bebas Neue', sans-serif;       /* Display */
--fo: 'Orbitron', sans-serif;          /* Gaming / HUD */
--fb: 'Barlow Condensed', sans-serif;  /* Body */
--fm: 'JetBrains Mono', monospace;     /* Monospace / telemetría */
```

### Escala Tipográfica Hero

```css
/* Título principal — clamp responsive */
font-size: clamp(64px, 9vw, 136px);

/* Secciones */
font-size: clamp(44px, 5.8vw, 78px);

/* Número de mercado */
font-size: clamp(100px, 17vw, 200px);
```

---

## 5. Módulos y Funcionalidades

### 5.1 Hero — Terminal Titan
- **Fondo triple capa:** imagen Unsplash (brightness: 18%) + Three.js particles + scanlines CSS
- **Three.js scene:** 2,400 partículas doradas + 600 partículas GT Red + tori flotantes + grid plano
- **Parallax:** el texto fantasma `.hero-ghost` responde al scroll
- **Mouse parallax:** la cámara Three.js sigue al cursor suavemente
- **Indicador LIVE:** punto rojo pulsante con `box-shadow` animado
- **Streak lines:** 3 líneas verticales con degradados rojo/dorado

### 5.2 Glitch Effect *(vitrina.css — NFS/Cyberpunk)*
Efecto glitch sobre el título principal vía pseudoelementos `::before` (GT Red, clip-path superior) y `::after` (Gold Light, clip-path inferior), activado con animaciones keyframe de `skewX` y `translateX` desincronizadas.

```css
.glitch::before { color: var(--red); animation: ga 5s infinite; }
.glitch::after  { color: var(--gold-l); animation: gb 5s 1.5s infinite; }
```

### 5.3 Gallery Masonry
Grid CSS de 12 columnas con 8 slots de layout fijo (`l1`→`l8`). Hover revela:
- Gradiente oscuro desde abajo
- Borde GT Red con `inset box-shadow`
- Label con marca y badge de certificación

### 5.4 Racing Films
3 tarjetas de video con imagen de portada, overlay de scanlines, botón play con `border-radius: 50%`, runtime badge y borde neon GT Red en hover. Click abre búsqueda YouTube de la marca correspondiente.

### 5.5 Tarjetas de Inventario *(vitrina.js)*

**Forza Horizon Popup**
Al hacer hover (desktop) o tap (mobile), aparece un badge con animación `cubic-bezier(.34,1.56,.64,1)` (efecto "overshoot"):
- BMW M2: `⚡ PERFECT SHIFT!`
- Porsche 911: `🏆 ULTIMATE SPEED`
- Mercedes AMG: `🔥 CLEAN RACING`

**RPM Bar** *(Gran Turismo HUD)*
Barra de 3px con gradiente `#E10600 → #ff4040`, `box-shadow` rojo y transición de 1.8s. Se llena al 100% en hover o tap. Incluye 10 muescas, las primeras 4 en GT Red.

**Speed Sweep**
Animación `@keyframes mb` — destello diagonal que cruza la imagen del vehículo al activar la tarjeta.

**Touch / Tap (Mobile)**
Clase `.tapped` aplicada por 2 segundos via `addEventListener('touchstart')` con bloqueo anti-doble-tap.

### 5.6 Sello Titán *(Gran Turismo telemetry HUD)*
4 anillos concéntricos con velocidades de rotación diferentes (22s, 13s, 9s, 40s), puntos rojos luminosos como marcadores cardinales, y score central `200` con indicador `PASS`. Panel lateral con 6 métricas de peritaje y barras animadas.

### 5.7 Scanner IDP — 3D Cube
Three.js secundario en canvas independiente: cubo exterior dorado + cubo interior GT Red + partículas + anillo de escaneo pulsante. Overlay con tabla de telemetría RUNT y barra de progreso roja animada.

### 5.8 Menú Gaming Pause Screen *(mobile)*
En `< 1024px`:
- Botón hamburguesa 44×44px (tamaño táctil óptimo) con animación a "✕"
- Drawer deslizante desde la derecha con `border-left: 2px solid #E10600`
- Links numerados (`01` → `06`) con efecto de `padding-left` en hover/tap
- Overlay semitransparente que cierra el menú al tocarlo
- Indicador de estado RUNT con punto verde pulsante

---

## 6. PWA — manifest.json y Service Worker

### manifest.json

```json
{
  "name": "BPD — Bogotá Premier Drive",
  "short_name": "BPD Premier",
  "display": "standalone",
  "orientation": "portrait-primary",
  "background_color": "#07070A",
  "theme_color": "#E10600",
  "lang": "es",
  "start_url": "./",
  "shortcuts": [
    { "name": "Iniciar Protocolo", "url": "./#protocolo" },
    { "name": "Ver Inventario",    "url": "./#inventario" },
    { "name": "Scanner IDP",       "url": "./#scanner" }
  ]
}
```

### Service Worker — Estrategias de Cache

| Tipo de Request | Estrategia | Cache Name |
|---|---|---|
| Navegación (HTML) | Network-First → Cache → Offline | `bpd-titan-v4.0` |
| Imágenes Unsplash | Cache-First → Network | `bpd-titan-v4.0` |
| JS / CSS / Fuentes | Cache-First → Network | `bpd-titan-v4.0` |
| Push Notifications | `push` event → `showNotification` | — |
| Sync pendientes | Background Sync `bpd-contact` | — |

### Página Offline
HTML embebido en el SW con animación de barra GT Red pulsante, título `SIN SEÑAL` en Champagne Gold y mensaje de reconexión.

### Extracción para Producción

```bash
# 1. Copiar el contenido de SW_CODE al archivo:
cp SW_CODE_content /public/sw.js

# 2. Guardar el objeto MANIFEST como:
cp MANIFEST_object /public/manifest.json

# 3. Actualizar el registro en vitrina.html:
# navigator.serviceWorker.register('/sw.js')
# <link rel="manifest" href="/manifest.json">
```

---

## 7. Instalación y Desarrollo Local

### Requisitos
- Navegador moderno (Chrome 90+ / Edge 90+ / Firefox 88+ / Safari 15+)
- Servidor HTTP local (requerido para Service Workers — no funciona con `file://`)

### Opción A — Python (sin dependencias)

```bash
cd C:\Users\PC\.claude\bpd
python -m http.server 8080
# → http://127.0.0.1:8080/vitrina.html
```

### Opción B — Node.js / npx

```bash
cd C:\Users\PC\.claude\bpd
npx serve .
# → http://localhost:3000/vitrina.html
```

### Opción C — VS Code Live Server
1. Instalar extensión **Live Server** (Ritwick Dey)
2. Click derecho en `vitrina.html` → **Open with Live Server**
3. Automáticamente abre `http://127.0.0.1:5500/vitrina.html`

### Verificar PWA en DevTools

```
Chrome DevTools → Application
  ├── Manifest       ← Verificar iconos y shortcuts
  ├── Service Workers ← Estado: activated and running
  └── Cache Storage  ← bpd-titan-v4.0: assets precacheados
```

---

## 8. Despliegue en Producción

### Wix (despliegue actual)
1. Importar `vitrina.html` como página custom en Wix Editor
2. Subir `sw.js` y `manifest.json` a Wix Media Manager o CDN propio
3. Configurar las rutas en el `<head>` de Wix:
   ```html
   <link rel="manifest" href="https://cdn.tudominio.com/manifest.json">
   ```
4. Registrar SW desde Wix Custom Code:
   ```js
   navigator.serviceWorker.register('https://cdn.tudominio.com/sw.js')
   ```

### Servidor propio / VPS

```bash
# Nginx — configuración recomendada
location /sw.js {
    add_header Cache-Control "no-cache";
    proxy_cache_bypass $http_pragma;
}

location /manifest.json {
    add_header Content-Type "application/manifest+json";
}
```

### Checklist de Producción

- [ ] HTTPS habilitado (requerido para Service Workers)
- [ ] `manifest.json` accesible en `/manifest.json`
- [ ] `sw.js` accesible en `/sw.js`
- [ ] Iconos PNG en `/assets/icons/`
- [ ] `theme-color` meta tag en `<head>`
- [ ] Lighthouse PWA score ≥ 90
- [ ] `robots.txt` configurado
- [ ] OG tags para compartir en redes sociales

---

## 9. Datos e Inventario

El inventario se alimenta del array `INV` en `js/data.js`. Solo se renderizan vehículos con estado `PUBLISHED` o `IN_INSPECTION`.

### Estructura de un vehículo

```javascript
{
  make:    'BMW',                    // Marca
  model:   'M2 CS',                 // Modelo
  year:    '2022',                  // Año
  badge:   'DESTACADO',             // Etiqueta (DESTACADO | VIP | NUEVO | EXCLUSIVO)
  hp:      '444',                   // Potencia (HP)
  acc:     '4.0S',                  // 0-100 km/h
  km:      '18,500',                // Kilometraje
  price:   '$285M',                 // Precio COP
  fin:     '$3.9M',                 // Cuota mensual estimada
  img:     'https://...',           // URL de imagen principal
  certs:   ['RUNT Limpio', ...],    // Certificaciones (máx. 3)
  forza:   '⚡ PERFECT SHIFT!',     // Texto popup Forza Horizon
  status:  'PUBLISHED'              // PUBLISHED | IN_INSPECTION | DRAFT
}
```

---

## 10. Responsividad y Mobile

| Breakpoint | Layout | Cambios |
|---|---|---|
| `> 1024px` | Desktop full | Menú horizontal, cursor custom, hover states |
| `901px – 1024px` | Tablet | Hamburger activo, menú horizontal oculto |
| `601px – 900px` | Mobile L | Stats 2 col, Gallery 2 col, Scanner 1 col |
| `< 600px` | Mobile S | Todo 1 col, tipografía clamp mínimo |

### Optimizaciones táctiles
- Botón hamburguesa: `44×44px` (WCAG 2.5.5 — tamaño mínimo de toque)
- Tap en tarjeta activa clase `.tapped` por 1.8s (simula hover de desktop)
- Specs de vehículo **siempre visibles** en mobile (sin depender de hover)
- Cursor custom desactivado en mobile (`cursor: auto`)

---

## 11. Referencias de Diseño

| Inspiración | Elementos adoptados |
|---|---|
| **Rolls-Royce / Aston Martin** | Hero 100vh full-bleed, espacio negativo, transiciones lentas |
| **Need for Speed Heat** | Glitch effect en título, scanlines, botones con glow neon |
| **Forza Horizon** | Popups de habilidad (PERFECT SHIFT, ULTIMATE SPEED) |
| **Gran Turismo** | GT Red `#E10600`, RPM bar, Sello Titán HUD telemétrico |
| **Lamborghini** | Fondo `#07070A`, hover states de alto contraste |
| **Porsche** | Grid de datos técnicos, JetBrains Mono para telemetría |

---

## 12. Roadmap

### v4.1 — Próximas mejoras
- [ ] Integración real con RUNT API (validación live)
- [ ] Módulo de valoración algorítmica (inputs del usuario)
- [ ] Galería 360° con Three.js OrbitControls por vehículo
- [ ] Chat WhatsApp flotante con pre-mensaje por modelo
- [ ] Formulario de contacto con Background Sync offline

### v4.2 — IA & Automatización
- [ ] AI Photo Studio — Nano Banana Pro para renders 4K
- [ ] IDP Document Scanner — OCR de tarjeta de propiedad
- [ ] Omnichannel Copy Engine — copy para Instagram/TikTok/OLX
- [ ] Pricing Intelligence — scraping de precios del mercado

### v5.0 — Plataforma completa
- [ ] Panel de administración BPD (CRUD de inventario)
- [ ] Dashboard de analytics (visitas, conversiones, WhatsApp clicks)
- [ ] App nativa via PWA + Capacitor (iOS / Android)

---

## 13. Créditos

**Diseño & Desarrollo**
BPD · Bogotá Premier Drive — Equipo Terminal Titan

**Assets visuales**
- Fotografías: [Unsplash](https://unsplash.com) — licencia libre
- Tipografías: [Google Fonts](https://fonts.google.com) — SIL OFL 1.1
- Motor 3D: [Three.js](https://threejs.org) — MIT License

**Estándares PWA**
- [web.dev/progressive-web-apps](https://web.dev/progressive-web-apps/)
- [MDN Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
- [W3C Web App Manifest](https://www.w3.org/TR/appmanifest/)

---

```
© 2026 BPD — Bogotá Premier Drive
Terminal Titan 4.0 · Todos los derechos reservados
El Asfalto es Opcional. La Adrenalina No.
```
