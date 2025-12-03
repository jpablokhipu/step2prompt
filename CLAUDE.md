# CLAUDE.md

Este archivo proporciona orientación a Claude Code (claude.ai/code) al trabajar con código en este repositorio.

## Descripción General del Proyecto

Este repositorio contiene **Step2Prompt (anteriormente Mondriart)**, un generador conversacional de prompts para creación de imágenes con IA, junto con el sistema **BMAD (Better Methodology for Agentic Development)** - un framework para construir y orquestar agentes de IA especializados.

### Dos Componentes Principales

1. **Step2Prompt** - Una aplicación web de una sola página (index.html) que guía a los usuarios a través de la creación de prompts profesionales para generación de imágenes
2. **BMAD Framework** - Un sistema modular de agentes con workflows, tareas y comandos slash para Claude Code

## Comandos de Desarrollo

### Ejecutar la Aplicación

```bash
# Abrir la aplicación web en el navegador
open index.html
```

La aplicación es 100% del lado del cliente (JavaScript vanilla, no requiere proceso de compilación).

### Flujo de Trabajo Git

Este repositorio usa prácticas convencionales de Git:

```bash
# Ver estado
git status

# Hacer commit de cambios (ver GIT-CHEATSHEET.md para comandos detallados)
git add .
git commit -m "Descripción"

# Subir al remoto
git push origin <nombre-rama>
```

**Rama actual**: `step2prompt`
**Repositorio remoto**: `https://github.com/jpablokhipu/step2prompt.git`
**Rama principal**: No está explícitamente configurada (probablemente `main` o `master`, confirmar antes de crear PRs)

## Arquitectura de Alto Nivel

### Aplicación Web Step2Prompt (index.html)

**Propósito**: Interfaz conversacional para crear prompts de imágenes IA optimizados para modelos Nanobanana/Gemini.

**Componentes Clave**:
- **Sistema de Flujo Dialógico**: Interfaz conversacional de 6 pasos (Sujeto, Acción, Entorno, Estilo, Iluminación, Detalles)
- **Detección de Categoría**: Detecta automáticamente si el sujeto es una persona, paisaje u objeto
- **Generación Inteligente de Prompts**: Dos modos:
  - Básico: Solo entradas del usuario, formato narrativo
  - Enriquecido: Añade presets profesionales (composición, especificaciones de cámara, calidad técnica, atmósfera)
- **Diseño Mobile-First**: Interfaz de chat estilo WhatsApp con diseños responsivos

**Stack Técnico**:
- HTML5 + CSS3 + JavaScript Vanilla puro
- Sin dependencias externas
- Todo el procesamiento ocurre del lado del cliente
- Estado gestionado mediante variables globales (`accumulatedContent`, `userTexts`, `currentStep`, `subjectCategory`)

**Funciones Clave**:
- `detectSubjectCategory()`: Categoriza el sujeto como persona/paisaje/objeto
- `generatePrompt()`: Crea prompt narrativo básico
- `generatePromptEnriched()`: Añade mejoras técnicas profesionales
- `editPrompt()`: Permite editar todos los pasos antes de generar

### BMAD Framework (bmad/)

**Propósito**: Sistema modular para crear agentes de IA especializados con workflows y tareas reutilizables.

**Estructura de Directorios**:
```
bmad/
├── _cfg/              # Configuración y plantillas
│   ├── agent-manifest.csv        # Registro de todos los agentes
│   ├── workflow-manifest.csv     # Registro de todos los workflows
│   ├── task-manifest.csv         # Registro de todas las tareas
│   ├── prompt-template.md        # Plantilla para nuevos agentes
│   ├── image-prompt-template.md  # Plantilla para agentes de prompts de imagen
│   ├── writing-prompt-template.md # Plantilla para agentes de prompts de escritura
│   ├── nanobanana-character-templates.md # Plantillas de personajes para Nanobanana
│   ├── PROMPT-CREATION-GUIDE.md  # Guía para crear prompts
│   └── create-prompt.sh          # Script para generar nuevos agentes
├── core/              # Módulo core (BMAD v6.0.0-alpha.4)
│   ├── config.yaml               # Usuario: Jp, Idioma: Español
│   ├── agents/                   # Agentes core (bmad-master, focus-coach, prompt-artist)
│   ├── workflows/                # Workflows core (brainstorming, party-mode)
│   ├── tasks/                    # Tareas reutilizables (index-docs, workflow.xml)
│   └── tools/                    # Herramientas (shard-doc)
└── cis/               # Módulo Creative Innovation Suite
    ├── agents/                   # Agentes CIS (brainstorming-coach, storyteller, etc.)
    └── workflows/                # Workflows CIS (design-thinking, problem-solving, etc.)
```

**Conceptos Clave**:

1. **Agentes**: Personas de IA especializadas con roles definidos, estilos de comunicación y menús de comandos
   - Definidos en markdown tipo XML con frontmatter YAML
   - Activados vía comandos slash en `.claude/commands/bmad/`
   - Cargan config desde el `config.yaml` del módulo al iniciar

2. **Workflows**: Procesos multi-paso definidos en YAML
   - Orquestados por `bmad/core/tasks/workflow.xml` (el "OS" de BMAD)
   - Se pueden activar desde menús de agentes usando el atributo `workflow="ruta/al/workflow.yaml"`
   - Soportan plantillas, recolección de datos y lógica condicional

3. **Manifiestos**: Registros CSV que rastrean todos los agentes, workflows y tareas
   - Ubicados en `bmad/_cfg/`
   - Deben actualizarse al agregar nuevos componentes

4. **Módulos**: Unidades organizacionales (core, cis, custom)
   - Cada uno tiene su propio `config.yaml` con preferencias de usuario
   - Configuración actual: Usuario es "Jp", idioma es "Español"

**Patrón de Activación de Agentes**:
```xml
<activation>
  <step n="1">Cargar persona desde el archivo del agente actual</step>
  <step n="2">Cargar {project-root}/bmad/core/config.yaml y almacenar variables</step>
  <step n="3">Recordar que user_name es {user_name}</step>
  <step n="4-6">Configurar idioma de comunicación y contexto</step>
  <step n="7">Mostrar saludo y menú numerado</step>
  <step n="8">Esperar entrada del usuario</step>
  <step n="9-10">Ejecutar comandos basados en la entrada</step>
</activation>
```

**Manejadores de Menú**:
- `action="#id"` - Ejecutar prompt con id coincidente en el archivo actual
- `action="text"` - Ejecutar texto como instrucción en línea
- `workflow="path"` - Cargar y ejecutar workflow vía workflow.xml

### Comandos Slash BMAD Disponibles

Agentes y workflows core accesibles vía `/`:
- `/bmad:core:agents:bmad-master` - Orquestador maestro y custodio del conocimiento
- `/bmad:core:agents:focus-coach` - Coach élite de enfoque y productividad
- `/bmad:core:agents:prompt-artist` - Creador élite de prompts de imágenes IA (Nanobanana, Midjourney, DALL-E, etc.)
- `/bmad:core:workflows:brainstorming` - Sesiones interactivas de lluvia de ideas
- `/bmad:core:workflows:party-mode` - Discusiones grupales multi-agente
- `/bmad:core:tasks:index-docs` - Generar index.md para directorios de documentos
- `/bmad:core:tools:shard-doc` - Dividir archivos markdown grandes en secciones

CIS (Creative Innovation Suite):
- `/bmad:cis:workflows:design-thinking` - Procesos de diseño centrado en el humano
- `/bmad:cis:workflows:storytelling` - Desarrollo de narrativas
- `/bmad:cis:workflows:problem-solving` - Resolución sistemática de problemas
- `/bmad:cis:workflows:innovation-strategy` - Innovación de modelos de negocio
- `/bmad:cis:agents:brainstorming-coach` - Especialista élite en lluvia de ideas
- `/bmad:cis:agents:storyteller` - Maestro narrador
- `/bmad:cis:agents:creative-problem-solver` - Experto en resolución de problemas
- `/bmad:cis:agents:design-thinking-coach` - Maestro de design thinking
- `/bmad:cis:agents:innovation-strategist` - Oráculo de innovación disruptiva

## Crear Nuevos Componentes BMAD

### Inicio Rápido: Usar el Script de Creación

```bash
cd bmad/_cfg
./create-prompt.sh

# O con parámetros:
./create-prompt.sh <módulo> <categoría> <id>
# Ejemplo: ./create-prompt.sh core agents mi-agente
```

### Pasos de Creación Manual

1. Copiar `bmad/_cfg/prompt-template.md` a la ubicación destino
2. Reemplazar todos los valores `[PLACEHOLDER]`
3. Definir persona (role, identity, communication_style, principles)
4. Diseñar menú con comandos
5. Agregar entrada al CSV de manifiesto apropiado en `bmad/_cfg/`
6. Crear enlace simbólico de comando slash en `.claude/commands/bmad/`
7. Probar activación y todos los comandos del menú

**Importante**: Siempre cargar el `config.yaml` del módulo al inicio del agente (paso 2 de activación).

## Notas de Desarrollo de Step2Prompt

### Versión Actual
**v3.1** - "Fase 1.5" con detección inteligente de lagunas y generación de prompts enriquecidos

### Relación con el Agente prompt-artist de BMAD
Step2Prompt (index.html) sirve como aplicación web independiente para crear prompts de imágenes IA, mientras que el agente prompt-artist de BMAD (`/bmad:core:agents:prompt-artist`) proporciona un asistente conversacional IA para propósitos similares dentro de Claude Code. Ambos comparten la misma base de conocimiento pero sirven casos de uso diferentes:
- **Step2Prompt**: Basado en web, flujo guiado de 6 pasos, optimizado para Nanobanana/Gemini, interfaz en español
- **agente prompt-artist**: Basado en CLI, conversación flexible, soporta todas las plataformas, bilingüe (Español/Inglés)

### Características Clave a Mantener
- Flujo conversacional de 5 preguntas
- Detección automática de categoría del sujeto (persona/paisaje/objeto)
- Ejemplos contextuales que se adaptan a la categoría detectada
- Dos modos de generación de prompts (básico y enriquecido)
- Diseño responsivo mobile-first (interfaz estilo WhatsApp)
- Procesamiento 100% del lado del cliente (enfocado en privacidad)
- Interfaz en español con ortografía correcta

### Patrones Técnicos

**Gestión de Estado**:
```javascript
let accumulatedContent = [];  // Almacena texto de visualización formateado
let userTexts = [];           // Almacena entradas crudas del usuario
let currentStep = 0;          // Pregunta actual (0-5)
let subjectCategory = 'general';  // Categoría detectada
```

**Detección de Categoría** (paso 0):
```javascript
function detectSubjectCategory(sujetoText) {
  // Verifica palabras clave: persona, paisaje, objeto
  // Retorna uno de: 'persona', 'paisaje', 'objeto', 'general'
}
```

**Sistema de Mejora de Prompts**:
```javascript
const promptEnhancers = {
  persona: { composition, technicalQuality, cameraSpecs, atmosphere, details },
  paisaje: { /* estructura similar */ },
  objeto: { /* estructura similar */ }
};
```

### Consideraciones Móviles
- Botón de envío visible en móvil (se muestra en pantallas < 480px)
- Ícono Enter visible solo en escritorio
- Tooltips usan posicionamiento fijo con overlay en móvil
- Tamaños de botón optimizados para touch (mínimo 44px de altura)
- Tamaños de fuente responsivos que se reducen en pantallas pequeñas
- El layout cambia a dirección de columna en móvil

### Mejoras Futuras (TODO.md)
El archivo TODO.md contiene un roadmap extenso que incluye:
- Integración de referencias de artistas
- Sistema de enriquecimiento multi-nivel
- Presets de paletas de colores
- Asistente de refinamiento post-generación
- Soporte multi-idioma (prompts en inglés para Midjourney/DALL-E)
- Historial de prompts con localStorage
- Presets rápidos para escenarios comunes

Consultar TODO.md para detalles completos sobre características planificadas.

## Organización de Archivos

```
.
├── index.html              # Aplicación web Step2Prompt (entregable principal)
├── avatar.jpg              # Imagen de avatar del asistente
├── README.md               # Resumen del proyecto y características
├── TODO.md                 # Roadmap detallado y mejoras futuras
├── GIT-CHEATSHEET.md      # Referencia de comandos Git
├── CLAUDE.md              # Este archivo
├── .gitignore             # Reglas de ignorar de Git
├── bmad/                  # Framework BMAD (ver estructura arriba)
│   ├── _cfg/              # Config, manifiestos, plantillas, scripts de creación
│   ├── core/              # Módulo core (agentes, workflows, tareas)
│   ├── cis/               # Módulo Creative Innovation Suite
│   └── docs/              # Documentación BMAD
└── .claude/               # Configuración de Claude Code
    └── commands/          # Enlaces simbólicos de comandos slash
        └── bmad/          # Estructura de comandos BMAD
```

## Notas Importantes

### Al Trabajar con Step2Prompt (index.html)
- La aplicación completa está en un solo archivo HTML - sin archivos JS/CSS separados
- Todo el estado es global - tener cuidado al modificar variables de estado
- La app detecta categoría del sujeto en el paso 0 (primera entrada del usuario) lo que afecta ejemplos subsecuentes
- `generatePrompt()` crea prompts básicos, `generatePromptEnriched()` añade presets técnicos
- El comportamiento móvil difiere significativamente - probar breakpoints responsivos
- Todo el texto está en español - mantener idioma y ortografía consistentes

### Al Trabajar con BMAD
- Siempre actualizar manifiestos al agregar/remover agentes o workflows
- La activación del agente DEBE cargar config.yaml en el paso 2 (antes de cualquier salida)
- El idioma de comunicación del config es "Español" - los agentes deben usar español
- El nombre de usuario del config es "Jp" - los agentes deben usar esto en saludos
- Los disparadores de menú usan asteriscos (*) no viñetas de markdown
- Los workflows son ejecutados por `bmad/core/tasks/workflow.xml` - nunca omitir esto
- Cargar recursos en tiempo de ejecución, nunca pre-cargar (principio BMAD)

### Flujo de Trabajo Git
- Rama de trabajo actual: `step2prompt`
- Repositorio remoto: `https://github.com/jpablokhipu/step2prompt.git`
- Rama principal no está explícitamente configurada - confirmar antes de crear PRs
- Ver GIT-CHEATSHEET.md para referencia detallada de comandos
- Los commits deben ser descriptivos y seguir estilo de commit convencional

## Procedimientos de Testing

### Testing de Step2Prompt
1. Abrir index.html en el navegador
2. Completar las 6 preguntas con varias entradas
3. Probar detección de categoría (intentar sujetos persona, paisaje, objeto)
4. Generar prompts básicos y enriquecidos
5. Probar funcionalidad de edición
6. Probar copiar al portapapeles
7. Probar comportamiento responsivo en viewport móvil
8. Verificar que el texto en español sea gramaticalmente correcto

### Testing de Agentes BMAD
1. Activar agente vía comando slash
2. Verificar que config.yaml se cargue exitosamente
3. Verificar que el saludo use el nombre de usuario correcto (Jp)
4. Verificar que la comunicación esté en español
5. Probar cada ítem del menú individualmente
6. Para workflows: verificar que todos los pasos se completen y la salida se guarde
7. Probar manejo de errores para comandos inválidos
8. Verificar que el comando de salida funcione apropiadamente

## Recursos

- **Step2Prompt**: Todo el código en index.html (autocontenido)
- **Documentación BMAD**:
  - `bmad/_cfg/PROMPT-CREATION-GUIDE.md` - Guía completa para crear agentes
  - `bmad/_cfg/README.md` - Resumen del directorio de configuración
  - `bmad/docs/claude-code-instructions.md` - Cómo usar BMAD en Claude Code
- **Referencia Git**: `GIT-CHEATSHEET.md`
- **Roadmap de Características**: `TODO.md`

## Tareas Comunes

### Agregar un nuevo agente BMAD
```bash
./bmad/_cfg/create-prompt.sh core agents nombre-del-nuevo-agente
# Editar archivo generado
# Agregar a bmad/_cfg/agent-manifest.csv
# Probar activación
```

### Modificar comportamiento de Step2Prompt
1. Abrir index.html en editor
2. Encontrar la sección relevante (usualmente en la etiqueta `<script>`)
3. Modificar funciones JavaScript
4. Probar en el navegador
5. Verificar responsividad móvil

### Agregar nuevos presets de enriquecimiento
1. Localizar el objeto `promptEnhancers` en index.html (alrededor de la línea 1362)
2. Agregar nuevas propiedades a las secciones persona/paisaje/objeto
3. Actualizar `generatePromptEnriched()` para usar las nuevas propiedades
4. Probar con varias entradas

### Crear un nuevo workflow BMAD
```bash
./bmad/_cfg/create-prompt.sh <módulo> workflows nombre-workflow
# Editar workflow.yaml con los pasos
# Editar instructions.md con la lógica del agente
# Editar template.md para el formato de salida
# Agregar a bmad/_cfg/workflow-manifest.csv
# Probar vía menú del agente
```
