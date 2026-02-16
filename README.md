# 🚀 NEXUS - Plataforma Educativa de Programación

![Versión](https://img.shields.io/badge/versión-7.0-00f5ff?style=for-the-badge)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)

---

## 📋 Tabla de Contenidos
- [Descripción General](#-descripción-general)
- [Arquitectura](#-arquitectura)
- [Funcionalidades Principales](#-funcionalidades-principales)
- [Sistema de IA](#-sistema-de-ia)
- [Sistema de Gamificación](#-sistema-de-gamificación)
- [Tecnologías y Dependencias](#-tecnologías-y-dependencias)
- [Instalación y Uso](#-instalación-y-uso)
- [Estructura del Código](#-estructura-del-código)
- [Decisiones de Diseño](#-decisiones-de-diseño)
- [Limitaciones Conocidas](#-limitaciones-conocidas)
- [Autor](#-autor)

---

## 🎯 Descripción General

**NEXUS** es una plataforma educativa interactiva de **código abierto** diseñada para el aprendizaje de programación. Implementa un entorno completo de aprendizaje con gamificación, asistente IA simulado, laboratorio de código en vivo y sistema de logros, todo en una **aplicación de página única (SPA)** construida con tecnologías web estándar.

### Propósito Principal
Proporcionar un espacio de aprendizaje interactivo donde los usuarios puedan:
- Aprender conceptos de programación mediante un asistente conversacional
- Practicar con ejercicios prácticos (quizzes)
- Experimentar con código en un entorno seguro (labs)
- Seguir su progreso mediante un sistema de XP y logros

---

## 🏗️ Arquitectura

### Diseño Técnico
```
┌─────────────────────────────────────────────────────────────┐
│                    Navegador Web                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │    HTML5     │  │    CSS3      │  │  JavaScript  │      │
│  │  Estructura  │  │   Estilos    │  │   Lógica     │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              localStorage (Persistencia)              │  │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐ │  │
│  │  │ Usuarios │ │  Perfil  │ │   XP     │ │  Chat    │ │  │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘ │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Patrón de Diseño
La aplicación utiliza el **patrón Módulo** con un objeto central `appState` que actúa como:
- **Store**: Contiene todo el estado de la aplicación (usuarios, datos, configuraciones)
- **Controlador**: Maneja la lógica de negocio y las interacciones
- **Renderizador**: Actualiza la interfaz según el estado

---

## ⚡ Funcionalidades Principales

### 1. **Sistema de Autenticación**
| Característica | Implementación |
|----------------|----------------|
| Registro local | Almacena usuario con email y contraseña (codificada en base64) |
| Login social simulado | 6 proveedores (Google, GitHub, Facebook, Discord, Microsoft, Apple) |
| Persistencia de sesión | Guarda usuario actual en `localStorage` |
| Cuentas vinculadas | Sistema de vinculación de múltiples proveedores |

### 2. **Perfil de Usuario Personalizable**
- **Avatar**: Emoji personalizable (máx. 2 caracteres)
- **Información**: Nombre, email, biografía
- **Estadísticas**: Nivel, XP total, racha de días
- **Configuración**: Notificaciones, sonidos, tema oscuro, idioma
- **Temas de color**: 5 temas rápidos + selector personalizado de colores

### 3. **Asistente IA**
- **Motor de respuestas**: Sistema basado en reglas y detección de patrones
- **Detección de intenciones**: Saludos, despedidas, temas técnicos
- **Persistencia**: Historial de conversaciones guardado por usuario
- **Feedback**: Sistema de votos (útil/no útil) para mejorar respuestas

### 4. **Recursos Educativos**
- **10 categorías**: HTML, CSS, JavaScript, TypeScript, React, Node.js, PHP, Python, Java, C#
- **3 recursos por categoría**: Enlaces a documentación oficial y tutoriales

### 5. **Quizzes Interactivos**
- **10 lenguajes × 3 niveles**: 30 quizzes diferentes
- **Sistema de puntuación**: Porcentaje basado en respuestas correctas
- **Recompensas XP**: Variable según rendimiento (0-100% del XP ofrecido)
- **Modal interactivo**: Interfaz limpia con feedback inmediato

### 6. **Laboratorio de Código (Labs)**
- **Editor en vivo**: Escribe y ejecuta código HTML/CSS/JS
- **Vista previa instantánea**: iframe que renderiza el resultado
- **Auto-guardado**: Guarda el código cada segundo después de escribir
- **Ejemplos predefinidos** por categorías:
  - **Básicos**: HTML, CSS, JavaScript, Tailwind
  - **Frameworks**: React, Vue, Svelte
  - **Visualización**: Three.js (3D), D3.js (gráficos)
  - **Juegos**: Juego interactivo con canvas

### 7. **Sistema de Logros**
- **9 logros** basados en:
  - Acumulación de XP (5 niveles)
  - Preguntas realizadas
  - Rachas de actividad
  - Votos útiles recibidos
- **Indicadores visuales**: Opacidad y color según completado
- **Estadísticas en tiempo real**: XP, nivel, racha actualizados

---

## 🤖 Sistema de IA

### Arquitectura del Motor de IA

```javascript
appState.iaKnowledge = {
  // Detección por temas
  javascript: { keywords: [...], responses: [...] },
  react: { keywords: [...], responses: [...] },
  python: { keywords: [...], responses: [...] },
  // ... más temas
  
  // Detección de intenciones
  intentions: {
    greeting: {...},
    farewell: {...}
  },
  
  // Respuestas genéricas por defecto
  defaultResponses: [...]
}
```

### Componentes del Sistema IA

#### 1. **Detector de Intenciones**
```javascript
// Prioridad 1: Detectar saludos y despedidas
const intentions = {
  greeting: {
    keywords: ['hola', 'buenos días', 'hey', 'hi'],
    responses: ["¡Hola! 👋 ¿En qué puedo ayudarte?"]
  },
  farewell: {
    keywords: ['adiós', 'hasta luego', 'bye'],
    responses: ["¡Hasta luego! Sigue practicando 🚀"]
  }
}
```

#### 2. **Clasificador de Temas**
```javascript
// Prioridad 2: Detectar temas técnicos
for (const [key, topic] of Object.entries(this.iaKnowledge)) {
  if (topic.keywords.some(keyword => message.includes(keyword))) {
    detectedTopic = topic;
    break;
  }
}
```

#### 3. **Motor de Respuestas Basado en Patrones**
```javascript
// Cada tema tiene respuestas específicas con patrones regex
responses: [
  {
    pattern: /(empezar|comenzar|aprender)/i,
    response: "Para empezar con JavaScript..."
  },
  {
    pattern: /(que es|definición)/i,
    response: "JavaScript es un lenguaje..."
  }
]
```

#### 4. **Sistema de Feedback**
- Botones "Útil/No útil" en cada respuesta
- Contador de respuestas útiles (`usefulCount`)
- Los datos de feedback se guardan para futuras mejoras

### Flujo de Procesamiento de Mensajes

```
1. Usuario escribe mensaje
2. appState.sendMessage() valida input
3. Se renderiza mensaje del usuario
4. Se muestra indicador "pensando..."
5. appState.getIAResponse() procesa:
   a. ¿Coincide con intención? (saludo/despedida) → respuesta inmediata
   b. ¿Coincide con tema? (JavaScript/React/etc.) → busca patrón
   c. ¿Coincide con patrón específico? → respuesta personalizada
   d. Por defecto → respuesta genérica aleatoria
6. Se renderiza respuesta con botones de feedback
7. Se guarda en historial (this.data.messages)
8. Se actualiza XP (+5 por mensaje)
```

---

## 🎮 Sistema de Gamificación

### Cálculo de XP y Niveles
```javascript
const XP_PER_LEVEL = 500;
this.data.level = Math.floor(this.data.xp / XP_PER_LEVEL) + 1;
```

### Fuentes de XP
| Acción | XP | Límite |
|--------|----|--------|
| Mensaje en chat | +5 | Ilimitado |
| Quiz (según rendimiento) | Variable (0-100% del quiz.xp) | Por quiz |
| Logros | No otorgan XP directo | - |

### Sistema de Rachas
```javascript
const dayInMs = 24 * 60 * 60 * 1000;
if (now - lastActivity < dayInMs) {
  // Actividad diaria → incrementa racha
  streak++;
} else if (now - lastActivity > dayInMs * 2) {
  // Más de 2 días sin actividad → reinicia racha
  streak = 1;
}
```

---

## 💻 Tecnologías y Dependencias

### Tecnologías Core
| Tecnología | Uso | Versión |
|------------|-----|---------|
| HTML5 | Estructura semántica | Estándar |
| CSS3 | Estilos, animaciones, responsive | Estándar |
| JavaScript (ES6+) | Lógica de negocio, interactividad | Estándar |

### APIs del Navegador
- **localStorage**: Persistencia de datos
- **IntersectionObserver**: No utilizado (scroll nativo)
- **requestAnimationFrame**: Animaciones (Three.js, juego)
- **Canvas API**: Renderizado de juegos y gráficos

### Dependencias Externas (CDN)
```html
<!-- Fuentes -->
<link href="https://fonts.googleapis.com/css2?family=Orbitron...">

<!-- Librerías para ejemplos en Labs -->
<script src="https://unpkg.com/react@18/umd/react.development.js">
<script src="https://unpkg.com/vue@3/dist/vue.global.js">
<script src="https://cdn.tailwindcss.com">
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js">
<script src="https://d3js.org/d3.v7.min.js">
```

### Estructura de Datos en localStorage
```javascript
// nexus-users: Todos los usuarios registrados
{
  "usuario@email.com": {
    username: "usuario",
    email: "usuario@email.com",
    password: "base64(password)",
    provider: "local",
    linkedAccounts: [],
    data: { xp, level, streak, profile },
    settings: { notifications, sound, darkMode, language, colors }
  }
}

// nexus-current-user: Usuario activo
{
  email: "usuario@email.com",
  username: "usuario"
}
```
---

## 📦 Instalación y Uso

### Requisitos Mínimos
- Navegador web moderno (Chrome 90+, Firefox 88+, Edge 90+, Safari 14+)
- JavaScript habilitado
- Conexión a internet (para cargar fuentes y librerías CDN)

### Instalación
```bash
# 1. Clona el repositorio
git clone https://github.com/tuusuario/nexus-platform.git

# 2. Navega al directorio
cd nexus-platform

# 3. ¡No necesita instalación! Solo abre el archivo
# Abre nexus-ultimate.html en tu navegador
```

### Configuración Inicial
1. **Abre el archivo** `nexus-ultimate.html`
2. **Regístrate** con email y contraseña (o login social simulado)
3. **Personaliza tu perfil** (opcional, pero recomendado)
4. **Explora las secciones** desde el menú principal

### Guía de Uso Rápida

#### Para Aprender:
1. Ve a **IA Asistente** y pregunta conceptos que no entiendas
2. Practica con **Quizzes** para consolidar conocimientos
3. Consulta **Recursos** para documentación oficial

#### Para Experimentar:
1. Ve a **Labs**
2. Elige una categoría de ejemplo
3. Modifica el código y haz clic en "EJECUTAR"
4. ¡Prueba el juego interactivo!

#### Para Seguir tu Progreso:
1. Haz clic en el avatar flotante (esquina inferior derecha)
2. Revisa tus estadísticas en la pestaña "ESTADÍSTICAS"
3. Ve a **Logros** para ver qué has desbloqueado

---

## 📁 Estructura del Código

### Organización del Archivo Único
```html
nexus-ultimate.html
├── <head>
│   ├── Metadatos y viewport
│   ├── Fuentes de Google
│   └── Estilos CSS (3000+ líneas)
│       ├── Variables CSS
│       ├── Componentes (loader, navbar, cards)
│       ├── Animaciones keyframes
│       └── Media queries responsive
│
├── <body>
│   ├── Pantalla de login
│   ├── Loader animado
│   ├── Navegación flotante
│   ├── Perfil flotante
│   ├── Modal de perfil
│   └── MAIN con 6 secciones:
│       ├── Hero (inicio)
│       ├── Tecnologías
│       ├── IA
│       ├── Recursos
│       ├── Quizzes
│       ├── Labs
│       └── Logros
│
└── <script>
    ├── Constantes globales
    ├── Objeto appState (2500+ líneas)
    │   ├── Datos de usuario
    │   ├── Sistema de autenticación
    │   ├── Motor de IA
    │   ├── Lógica de quizzes
    │   ├── Funciones de Labs
    │   └── Utilidades (notificaciones, sincronización)
    └── Inicialización y event listeners
```

### Patrones de Diseño Implementados

#### 1. **Módulo Revelador (Revealing Module Pattern)**
```javascript
const appState = {
  // Datos públicos
  users: {},
  currentUser: null,
  
  // Métodos públicos
  loginUser() { ... },
  sendMessage() { ... },
  
  // Métodos privados (por convención _)
  _validateInput() { ... }
}
```

#### 2. **Inmutabilidad Controlada**
```javascript
// Copias profundas para evitar mutaciones accidentales
this.data = JSON.parse(JSON.stringify(user.data));
```

#### 3. **Observador (Observer Pattern)**
```javascript
// syncUI() actúa como método de actualización
// Notifica a todos los componentes visuales
syncUI() {
  this.updateXPBadge();
  this.updateLevel();
  this.updateProgressBar();
  // ...
}
```

---

## 🎨 Decisiones de Diseño

### 1. **Aplicación de Página Única (SPA) sin Frameworks**
**Razón**: 
- Simplicidad para un proyecto inicial
- Control total sobre el código
- Sin dependencias pesadas
- Ideal para demostrar habilidades vanilla JS

**Implementación**:
- Secciones ocultas con clase `.hidden`
- Función `show(sectionId)` que maneja la navegación

### 2. **localStorage como Backend**
**Razón**:
- No requiere servidor
- Persistencia entre sesiones
- Fácil implementación para prototipo
- Suficiente para demostración educativa

**Trade-offs**:
- Límite de 5-10MB
- No sincronización entre dispositivos
- Datos accesibles en cliente

### 3. **IA Basada en Reglas vs. APIs Externas**
**Razón**:
- Funciona sin conexión a internet (excepto ejemplos)
- Control total sobre respuestas
- Sin costos de API
- Enfocado en demostrar lógica de procesamiento

**Limitación**: No aprende ni mejora con el tiempo

### 4. **Diseño Cyberpunk**
**Razón**:
- Identidad visual fuerte y memorable
- Facilita separación de componentes
- Temática "futurista" adecuada para tecnología
- Colores neón contrastan bien con fondo oscuro

**Implementación**:
- Variables CSS para cambio de tema
- Sombras y brillos para efecto "glow"
- Animaciones sutiles para feedback

### 5. **Gamificación como Motivador**
**Razón**:
- Aumenta engagement del usuario
- Proporciona objetivos claros
- Feedback de progreso constante
- Sentido de logro y recompensa

---

## ⚠️ Limitaciones Conocidas

### Técnicas
1. **No hay backend real**: Los datos solo persisten en localStorage
2. **IA no es inteligente**: Es un sistema de reglas, no ML real
3. **Login social es simulado**: No hay OAuth real implementado
4. **Sin base de datos**: No escalable a múltiples usuarios simultáneos
5. **Sin autenticación segura**: Contraseñas codificadas en base64 (no encriptadas)

### Funcionales
1. **Quizzes con preguntas fijas**: No se pueden añadir nuevas sin editar código
2. **Recursos limitados**: Solo 3 por lenguaje
3. **Sin editor colaborativo**: Labs es individual
4. **Sin modo multijugador**: No hay competencia en tiempo real

### De Experiencia de Usuario
1. **Menú flotante puede superponerse** en resoluciones muy pequeñas
2. **Sin atajos de teclado** para navegación avanzada
3. **Sin soporte PWA** (no se puede instalar como app)
4. **Sin modo offline completo**: Algunos ejemplos requieren CDN

---

## 🔮 Próximas Mejoras (Ideas)

Si se continuara el desarrollo:

1. **Backend real** con Node.js/Express y MongoDB
2. **Autenticación OAuth** real con Passport.js
3. **IA con API de OpenAI** (GPT) para respuestas reales
4. **Editor colaborativo** con WebSockets
5. **Desafíos semanales** con recompensas especiales
6. **Perfiles públicos** para compartir logros
7. **Modo oscuro/claro** mejorado (ya implementado)
8. **Exportar/importar** datos en JSON
9. **Sonidos** para feedback de acciones
10. **Soporte PWA** para instalación en dispositivos

---

## 👨‍💻 Autor

**Tu Nombre**
- 🎓 **Desarrollador Web Principiante**
- 📅 **Primer Proyecto Completo:** Febrero 2026
- 🎯 **Objetivo:** Demostrar dominio de HTML, CSS y JavaScript vanilla
- 💡 **Habilidades demostradas:**
  - Diseño responsive y mobile-first
  - Animaciones CSS avanzadas
  - Manipulación del DOM
  - Persistencia con localStorage
  - Patrones de diseño en JavaScript
  - Sistema de rutas SPA
  - Gamificación y experiencia de usuario

### 📞 Contacto
- **GitHub:** [@visintinitech](https://github.com/visintinitech
- **Email:** tu@email.com
- **LinkedIn:** [Tu Perfil](https://linkedin.com/in/sofia-amalia-visintini-34383a3ab)

---

## 📄 Licencia

Este proyecto es de **código abierto** bajo la licencia MIT. Puedes:
- ✅ Usarlo con fines educativos
- ✅ Modificarlo y adaptarlo
- ✅ Compartirlo con otros
- ✅ Incluirlo en tu portafolio

**Condición**: Por favor, da crédito al autor original si utilizas partes significativas del código.

---

## 🙏 Agradecimientos

- A la comunidad de **MDN Web Docs** por la documentación invaluable
- A **Google Fonts** por las tipografías espectaculares
- A los creadores de **Tailwind**, **Three.js**, **D3.js**, **React**, **Vue** y **Svelte** por inspirar los ejemplos
- A **VS Code** por ser el editor donde nació este proyecto
- A ti, **lector**, por tomarte el tiempo de explorar mi trabajo

---

## 🏁 Palabras Finales

Este README documenta **más de 90 funcionalidades implementadas** en NEXUS v7.0. Como primer proyecto completo, representa:

- 3000+ líneas de CSS
- 2500+ líneas de JavaScript
- 1000+ líneas de HTML
- 3 meses de desarrollo y aprendizaje

**¿El resultado?** Una plataforma educativa funcional, atractiva y completamente interactiva, construida desde cero con tecnologías web estándar.

---

*"El mejor momento para aprender a programar fue ayer. El segundo mejor momento es ahora."*

**¡Gracias por visitar NEXUS!** 🚀
