# 📋 TODO - Mejoras Futuras para Mondriart/Step2Prompt

## 🎯 MEJORAS IMPLEMENTADAS RECIENTEMENTE

- ✅ Sistema de enriquecimiento automático de prompts
- ✅ Botón "Generar Prompt Enriquecido" con 3 opciones elegibles
- ✅ Detección inteligente de mood basado en iluminación
- ✅ Templates profesionales por categoría (persona/paisaje/objeto)
- ✅ Composiciones aleatorias variadas

---

## 🚀 PRIORIDAD ALTA - Mejoras Core

### 1. Expandir Variaciones de Composición
**Objetivo**: Aumentar la variedad de composiciones para evitar repetición

**Tareas**:
- [ ] Agregar 5-10 variaciones más por categoría en `promptEnhancers.composition`
- [ ] Incluir términos técnicos adicionales:
  - Para retratos: "Dutch angle", "low angle portrait", "high key composition"
  - Para paisajes: "panoramic vista", "aerial perspective", "foreground interest"
  - Para objetos: "flat lay composition", "diagonal arrangement", "hero shot"

**Beneficio**: Cada generación será más única y variada

---

### 2. Referencias de Artistas Opcionales
**Objetivo**: Permitir agregar estilo de artistas reconocidos

**Tareas**:
- [ ] Crear lista de artistas por categoría:
  - **Fotografía de retrato**: Annie Leibovitz, Peter Lindbergh, Richard Avedon
  - **Fotografía de paisaje**: Ansel Adams, Sebastião Salgado, Galen Rowell
  - **Ilustración digital**: Loish, Ross Tran, Ilya Kuvshinov
  - **Arte conceptual**: Syd Mead, Simon Stålenhag, John Berkey
- [ ] Agregar pregunta opcional 6: "¿Quieres referencia de artista? (opcional)"
- [ ] Integrar en función `generatePromptEnriched()`

**Ejemplo de salida**:
> "...resolución 8K, in the style of Annie Leibovitz."

---

### 3. Sistema de Niveles de Enriquecimiento
**Objetivo**: Ofrecer 3 niveles en lugar de solo 2 opciones

**Tareas**:
- [ ] Modificar botones finales a 4 opciones:
  - "Generar Prompt Básico" (como actual básico)
  - "Generar Prompt Intermedio" (+ composición + calidad)
  - "Generar Prompt Profesional" (+ atmósfera + specs + referencias)
  - "Editar"
- [ ] Crear función `generatePrompt(level)` con parámetro de nivel
- [ ] Ajustar `promptEnhancers` para soportar niveles

**Beneficio**: Mayor control granular para el usuario

---

## 🎨 PRIORIDAD MEDIA - Enriquecimiento Avanzado

### 4. Paletas de Colores Específicas
**Objetivo**: Agregar información de paleta cromática según contexto

**Tareas**:
- [ ] Crear objeto `colorPalettes` con opciones por mood:
  ```javascript
  colorPalettes: {
    warm: "paleta cálida con tonos dorados, naranjas y rojos suaves",
    cool: "paleta fría con azules, verdes y púrpuras",
    monochrome: "paleta monocromática con alto contraste",
    pastel: "paleta pastel suave y delicada",
    vibrant: "colores vibrantes y saturados",
    earth: "tonos tierra naturales y orgánicos"
  }
  ```
- [ ] Detectar keywords de color en las respuestas del usuario
- [ ] Agregar automáticamente si no se especificó paleta

**Ejemplo de salida**:
> "...atmósfera suave y serena, paleta cálida con tonos dorados y naranjas suaves."

---

### 5. Wizard de Mejora Post-Generación
**Objetivo**: Permitir refinar el prompt después de generarlo

**Tareas**:
- [ ] Agregar sección de mejora después del resultado:
  ```html
  <div id="enhancementWizard">
    <h3>💡 Mejora tu prompt</h3>
    <button>🎨 Agregar paleta de colores</button>
    <button>👨‍🎨 Agregar referencia de artista</button>
    <button>🎭 Cambiar atmósfera/mood</button>
    <button>📐 Cambiar composición</button>
    <button>✨ Regenerar variación</button>
  </div>
  ```
- [ ] Crear funciones individuales para cada mejora
- [ ] Permitir aplicar mejoras acumulativas
- [ ] Mantener historial de versiones del prompt

**Beneficio**: Iteración rápida sin rehacer todo el proceso

---

### 6. Soporte Multiidioma Mejorado
**Objetivo**: Generar prompts en inglés y español según plataforma

**Tareas**:
- [ ] Detectar plataforma objetivo (pregunta adicional o dropdown):
  - Nanobanana/Gemini → Español (narrativo)
  - Midjourney → Inglés (keywords + parámetros)
  - DALL-E 3 → Inglés (narrativo)
  - Stable Diffusion → Inglés (positive + negative)
- [ ] Traducir templates de `promptEnhancers` al inglés
- [ ] Crear función `translateToEnglish()` si es necesario
- [ ] Agregar parámetros específicos de plataforma:
  - Midjourney: `--ar 16:9 --v 6 --style raw`
  - Stable Diffusion: agregar negative prompts

**Ejemplo Midjourney**:
> "portrait of 30yo woman, direct gaze, studio setting, professional photography, soft lighting, Canon EOS R5 85mm f/1.4, photorealistic, 8K --ar 3:4 --v 6 --style raw"

---

## 🔧 PRIORIDAD BAJA - Calidad de Vida

### 7. Historial de Prompts Generados
**Objetivo**: Guardar prompts anteriores para referencia

**Tareas**:
- [ ] Usar `localStorage` para guardar últimos 10 prompts
- [ ] Agregar botón "Ver historial"
- [ ] Permitir recargar y editar prompts anteriores
- [ ] Agregar botón "Exportar historial" (JSON/TXT)

---

### 8. Presets Rápidos por Escenario
**Objetivo**: Plantillas pre-configuradas para casos comunes

**Tareas**:
- [ ] Crear biblioteca de presets:
  - "Retrato profesional LinkedIn"
  - "Foto de producto e-commerce"
  - "Paisaje inspirador para blog"
  - "Avatar estilo ilustración"
  - "Escena cinematográfica"
- [ ] Agregar opción al inicio: "¿Empezar desde preset o desde cero?"
- [ ] Pre-completar respuestas con valores del preset

---

### 9. Modo Comparación Visual
**Objetivo**: Mostrar lado a lado prompt básico vs enriquecido

**Tareas**:
- [ ] Crear botón "Comparar ambos prompts"
- [ ] Mostrar en dos columnas:
  - Izquierda: Prompt básico
  - Derecha: Prompt enriquecido
  - Resaltar diferencias en colores
- [ ] Agregar contador de palabras en cada uno
- [ ] Permitir copiar cualquiera de los dos

---

### 10. Integración con APIs de Generación
**Objetivo**: Generar imagen directamente desde la app

**Tareas**:
- [ ] Agregar botón "Generar imagen ahora"
- [ ] Integrar con APIs:
  - Replicate (Stable Diffusion)
  - OpenAI (DALL-E 3)
  - Fal.ai (múltiples modelos)
- [ ] Mostrar preview de imagen generada
- [ ] Permitir regenerar con ajustes

**Nota**: Requiere backend o API keys del cliente

---

## 🧪 EXPERIMENTALES - Ideas Avanzadas

### 11. IA Adaptativa con Aprendizaje
**Objetivo**: Aprender de los prompts que el usuario prefiere

**Tareas**:
- [ ] Agregar botón "❤️ Me gustó este prompt" después de generar
- [ ] Guardar patrones de prompts favoritos
- [ ] Ajustar pesos de enriquecimiento según preferencias
- [ ] Sugerir estilos similares a los preferidos

---

### 12. Modo Colaborativo
**Objetivo**: Compartir y colaborar en prompts

**Tareas**:
- [ ] Generar URL única para cada prompt
- [ ] Permitir compartir por link
- [ ] Agregar comentarios y sugerencias
- [ ] Galería pública de mejores prompts

---

### 13. Plugin para Navegador
**Objetivo**: Usar desde cualquier plataforma de generación

**Tareas**:
- [ ] Crear extensión de Chrome/Firefox
- [ ] Botón flotante en Midjourney/DALL-E/etc.
- [ ] Auto-rellenar el prompt generado
- [ ] Guardar prompts por plataforma

---

## 📊 MÉTRICAS Y ANALYTICS

### 14. Sistema de Métricas Internas
**Objetivo**: Entender uso y mejorar el producto

**Tareas**:
- [ ] Tracking (anónimo, local) de:
  - Categoría más usada (persona/paisaje/objeto)
  - Botón más usado (básico vs enriquecido)
  - Palabras más comunes en respuestas
  - Tiempo promedio de uso
- [ ] Dashboard local de estadísticas
- [ ] Exportar métricas para análisis

---

## 🐛 BUGS Y MEJORAS TÉCNICAS

### 15. Optimizaciones de Código
**Tareas**:
- [ ] Separar JavaScript en archivo externo `app.js`
- [ ] Separar CSS en archivo externo `styles.css`
- [ ] Modularizar código en funciones más pequeñas
- [ ] Agregar comentarios JSDoc
- [ ] Implementar manejo de errores robusto

### 16. Accesibilidad
**Tareas**:
- [ ] Agregar ARIA labels a todos los botones
- [ ] Soporte completo de teclado (Tab, Enter, Esc)
- [ ] Modo alto contraste
- [ ] Lector de pantalla compatible
- [ ] Modo oscuro / claro

### 17. Responsive Design
**Tareas**:
- [ ] Optimizar para móviles (< 768px)
- [ ] Optimizar para tablets (768-1024px)
- [ ] Touch gestures para móviles
- [ ] Probar en iOS Safari y Android Chrome

---

## 📚 DOCUMENTACIÓN

### 18. Documentación de Usuario
**Tareas**:
- [ ] Crear guía de uso en `GUIA-DE-USO.md`
- [ ] Video tutorial corto (< 2 min)
- [ ] FAQ con preguntas comunes
- [ ] Ejemplos de mejores prácticas

### 19. Documentación de Desarrollador
**Tareas**:
- [ ] Documentar arquitectura en `ARCHITECTURE.md`
- [ ] Guía de contribución `CONTRIBUTING.md`
- [ ] Documentar API interna de funciones
- [ ] Agregar tests unitarios (Jest)

---

## 🎓 EDUCACIÓN Y CONTENIDO

### 20. Modo Tutorial Interactivo
**Tareas**:
- [ ] Tutorial paso a paso al primer uso
- [ ] Tooltips explicativos en cada pregunta
- [ ] "¿Por qué es importante esto?" en cada paso
- [ ] Mostrar prompts de ejemplo profesionales

### 21. Biblioteca de Conocimiento Integrada
**Tareas**:
- [ ] Botón "💡 Aprende más" en cada paso
- [ ] Mostrar tips de composición cuando se pregunta por composición
- [ ] Tips de iluminación con ejemplos visuales
- [ ] Glosario de términos técnicos

---

## ⚙️ CONFIGURACIÓN

### 22. Panel de Preferencias de Usuario
**Tareas**:
- [ ] Botón "⚙️ Configuración"
- [ ] Opciones configurables:
  - Idioma de salida (ES/EN)
  - Plataforma favorita (Nanobanana/Midjourney/DALL-E/SD)
  - Nivel de enriquecimiento por defecto
  - Ocultar/mostrar ejemplos
  - Sugerencias automáticas on/off
- [ ] Guardar preferencias en localStorage

---

## 🌟 VERSIÓN PREMIUM (FUTURO)

### 23. Funcionalidades Premium Potenciales
**Ideas para monetización futura**:
- [ ] Generación ilimitada de variaciones
- [ ] Acceso a biblioteca de 1000+ presets profesionales
- [ ] Integración directa con APIs de generación (créditos incluidos)
- [ ] Asesoría personalizada de prompts
- [ ] Análisis de prompts existentes con sugerencias de mejora
- [ ] Exportar a múltiples formatos (JSON, CSV, PDF)
- [ ] Modo equipo con colaboración en tiempo real

---

## 📅 ROADMAP SUGERIDO

### Q1 2025 (Enero - Marzo)
- Mejoras 1, 2, 4 (Variaciones, Artistas, Paletas)
- Mejoras 15, 16, 17 (Optimizaciones técnicas)

### Q2 2025 (Abril - Junio)
- Mejoras 3, 5 (Niveles, Wizard post-gen)
- Mejora 6 (Multiidioma mejorado)
- Mejora 18, 19 (Documentación)

### Q3 2025 (Julio - Septiembre)
- Mejoras 7, 8, 9 (Historial, Presets, Comparación)
- Mejora 20, 21 (Tutorial, Biblioteca conocimiento)

### Q4 2025 (Octubre - Diciembre)
- Mejoras experimentales 10, 11, 12, 13
- Evaluación de versión premium

---

## 🤝 CONTRIBUCIONES

Si quieres implementar alguna de estas mejoras:
1. Marca la tarea como en progreso agregando `[WIP]` al lado
2. Crea un branch con el nombre de la mejora
3. Implementa y prueba
4. Actualiza este TODO con el estado

---

## 📝 NOTAS

- Prioridad basada en impacto vs esfuerzo
- Mantener siempre la simplicidad del UX actual
- Todas las mejoras deben ser opcionales/no-invasivas
- Testing en múltiples navegadores antes de release

---

**Última actualización**: 2025-12-03
**Versión actual**: v3.1 (Mondriart) con sistema de enriquecimiento
**Próxima versión planeada**: v3.2 (Variaciones expandidas + Referencias de artistas)
