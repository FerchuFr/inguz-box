# Directivas y Memoria para Agentes — Inguz Box

Instrucciones de arquitectura, diseño y despliegue para cualquier agente o asistente de IA trabajando en este repositorio.

---

## 1. Contexto del Proyecto y Negocio
* **Cliente / Marca:** Inguz Box (Espacio de Entrenamiento, Salud, Estética y Bienestar).
* **Sede física:** Gral. Machado 944, 1° Piso, Morón Centro, Buenos Aires (a 2 cuadras de la estación de Morón).
* **WhatsApp Oficial para Turnos:** `+54 11 3571-3432` (`https://wa.me/5491135713432`).
* **Objetivo del repositorio:** Landing pages individuales de alta conversión para cada vertical de servicios, optimizadas para mobile-first y contacto directo por WhatsApp.

---

## 2. Sistema de Diseño e Identidad Visual (Manual Oficial)
Cualquier landing o componente generado **DEBE** respetar estrictamente esta paleta y tipografías:

### Paleta de Colores
* **Verde Corporativo (Acento y CTAs principales):** `#78B74F` (Hover: `#8cd15e`, Glow: `rgba(120, 183, 79, 0.35)`).
* **Negro Base / Dark Mode:** `#000000`, `#0a0d0b`, `#121713`.
* **Superficies Secundarias / Bordes:** `rgba(120, 183, 79, 0.2)` o `rgba(255, 255, 255, 0.06)`.
* **Fondo Claro (Contraste para catálogos y FAQs):** `#FFFEF8` (Off-white).
* **Textos:**
  - Títulos oscuros / claros: `#FFFEF8` (en dark) / `#000000` (en light).
  - Texto secundario: `#abb8c3` (en dark) / `#58585A` (en light).

### Tipografías
* **Web (Google Fonts):**
  - **Títulos y cuerpo:** `Inter` (weights: 400, 600, 700, 800, 900).
  - **Logos y badges destacados:** `Oswald` (weights: 600, 700).

### Reglas de Copy y Estilo Visual
* **Botones limpios:** NO incluir emojis invasivos como `📲` o `⚡` en botones de navegación ni en llamadas a la acción principales.
* **Fotos reales:** Priorizar siempre fotos reales del consultorio/box (disponibles en sus respectivas carpetas) por sobre placeholders o ilustraciones genéricas.

---

## 3. Estructura Estándar de las Landing Pages
Cada vertical sigue una arquitectura de 8 bloques probada en conversión:
1. **Top Bar:** Barra superior negra con ubicación y claim del espacio.
2. **Navbar fija:** Logo con badge de la vertical, anclas internas y botón de WhatsApp destacado.
3. **Hero Section (Dark):** Badge pill verde, H1 de impacto con acento verde, subtítulo con el profesional a cargo, checklist de beneficios, botón principal de WhatsApp y tarjeta flotante del profesional con foto real.
4. **Showcase Multimedia:** Grilla con fotos o videos reales en gabinete (videos en loop muted).
5. **Catálogo de Servicios/Tratamientos (Light):** Fondo `#FFFEF8` con tarjetas de bordes superiores verdes e íconos corporativos específicos.
6. **El Método en 3 Pasos (Dark):** 1. Diagnóstico/Evaluación, 2. Sesión Personalizada, 3. Alivio/Seguimiento.
7. **Preguntas Frecuentes:** Acordeón interactivo en fondo claro con respuestas directas a objeciones comunes.
8. **Banner Final de Conversión:** Tarjeta oscura con borde verde, claim motivador y botón directo de turnos.
9. **Botón Flotante de WhatsApp:** Fijado en la esquina inferior derecha (`z-index: 999`).

---

## 4. Entregables por Vertical
Para cada vertical (Nutrición, Masajes, Depilación, etc.) se generan **dos formatos idénticos**:
1. **Archivo HTML estático (`<vertical>.html`):** Autocontenido, responsivo, con Vanilla CSS y JavaScript interactivo (para el acordeón).
2. **Plantilla Elementor JSON (`plantilla_elementor_<vertical>.json`):** Formato JSON v0.4 listo para importar en WordPress/Elementor.

---

## 5. Infraestructura y Despliegue en Servidor

### Datos del Servidor Hostinger
* **Host IP:** `147.93.39.187`
* **Puerto SSH:** `65002`
* **Usuario SSH:** `u524469798`
* **Web Root:** `/home/u524469798/domains/inguzbox.com/public_html/`

### Subdominios y Rutas
| Vertical | Subdominio Oficial | Carpeta en Servidor | Estado |
|---|---|---|---|
| **Nutrición** | `https://nutricion.inguzbox.com` | `public_html/nutricion/` | **En Producción** |
| **Masajes** | `https://masajes.inguzbox.com` | `public_html/masajes/` | **En Producción** |
| **Depilación** | `https://depilacion.inguzbox.com` | `public_html/depilacion/` | **Pendiente de material** |

### Reglas Críticas de Despliegue
* En cada carpeta de servicio en el servidor, subir el HTML copiado como `index.html`.
* Eliminar o renombrar `default.php` a `default.php.bk` para que el placeholder de Hostinger no pise el sitio.
* Asegurar permisos: directorios `755`, archivos `644`.
* Configurar en `.htaccess`:
  ```apache
  <IfModule mod_headers.c>
  Header set Cache-Control "no-cache, no-store, must-revalidate"
  Header set Pragma "no-cache"
  Header set Expires 0
  </IfModule>
  DirectoryIndex index.html index.php
  ```
* Si la carpeta contiene WordPress, asegurarse de que el tema activo (`hello-elementor`) tenga un `front-page.php` que haga `readfile(ABSPATH . 'index.html'); exit;` para evitar que WordPress cargue archivos vacíos de blog.

---

## 6. Convenciones de Desarrollo y Git
* **Commits:** Usar exclusivamente *Conventional Commits* (ej: `feat: ...`, `fix: ...`).
* **Sin atribución:** NUNCA agregar "Co-Authored-By" ni firmas automáticas de IA en los mensajes de commit.
* **Archivos privados:** Todo archivo con credenciales, tokens o notas internas debe guardarse en la carpeta `local/` (ignorada en `.gitignore`).
