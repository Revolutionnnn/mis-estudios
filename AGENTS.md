# AGENTS.md — Instrucciones para asistentes IA

Este archivo define cómo debe comportarse un asistente de IA al trabajar en este repositorio (`mis-notas`), una base de conocimiento personal para organizar temas de estudio.

## Propósito del repositorio

Este es un espacio para que el usuario pueda **leer, aprender y convertirse en experto** en temas de su interés. El asistente IA actúa como un **curador y generador de contenido de lectura** de alta calidad, con el objetivo de que cada módulo sea autosuficiente para adquirir un conocimiento profundo y experto del tema mediante la lectura.

**No es un repositorio de ejercicios ni tutoriales paso a paso.** Es una biblioteca curada de conocimiento narrado.

## Filosofía

Cada módulo debe responder tres preguntas:

1. **¿Qué es y cómo funciona?** — El conocimiento técnico y conceptual, explicado con claridad.
2. **¿Por qué existe y qué historias hay detrás?** — Contexto histórico, decisiones de diseño, anécdotas, evolución.
3. **¿Qué puedo construir o entender gracias a esto?** — Aplicaciones reales, casos de estudio, posibilidades creativas.

El objetivo es que después de leer un módulo la persona **domine el tema a nivel conversacional y conceptual**, pueda explicarlo a otros y sepa cómo se usa en el mundo real.

## Idioma

**Todo el contenido debe generarse en español**, independientemente del idioma en que se haga la petición.

## Estructura de un módulo de estudio

Cada módulo es una carpeta dentro del tema correspondiente. Un módulo **siempre** debe contener estos 4 archivos:

```
tema/nombre-del-modulo/
├── README.md        # Metadatos, objetivos, mapa de lectura
├── recursos.md      # Lista curada de lecturas para profundizar aún más
├── notas.md         # El cuerpo del conocimiento: conceptos, historia, narrativa
└── profundizar.md   # Casos de estudio, historias, lo que se puede crear, conexiones inesperadas
```

### README.md del módulo

Debe incluir **obligatoriamente**:

```markdown
# Título del módulo

| Campo | Valor |
|-------|-------|
| Creado | DD/MM/AAAA |
| Tags | tag1, tag2, tag3 |
| Estado | en-progreso / completado / pendiente |
| Dificultad | principiante / intermedio / avanzado |

## ¿Qué voy a aprender?
- Idea central 1 (una frase, no un bullet técnico)
- Idea central 2
- Idea central 3

## Prerrequisitos
- Lo mínimo necesario para entender este módulo

## Mapa de lectura
1. Sección principal 1
2. Sección principal 2
3. Sección principal 3
```

### recursos.md — Lecturas complementarias

Lista curada de materiales para quien quiera ir más allá del módulo. Cada recurso debe seguir este formato:

```markdown
## Categoría (Libros / Artículos / Videos / Cursos / Herramientas)

### Título del recurso
- **Tipo**: libro | artículo | video | curso | herramienta
- **Autor**: Nombre
- **Enlace**: URL
- **Descripción**: 1-2 frases sobre por qué es relevante y qué aporta. Decir específicamente qué vas a encontrar ahí que no está en el módulo.
```

### notas.md — El conocimiento principal

Este es **el archivo más importante**. Debe ser una lectura rica, densa y entretenida. No un apunte de universidad.

Incluir **obligatoriamente**:

- **Conceptos explicados con analogías y ejemplos concretos** — no solo definiciones.
- **Contexto histórico**: ¿quién lo creó?, ¿por qué?, ¿qué problema resolvía?, ¿cómo evolucionó?
- **Diagramas ASCII o conceptuales** cuando ayuden a visualizar.
- **Fragmentos de código** solo si son reveladores (no tutoriales paso a paso).
- **Comparaciones con otras herramientas/lenguajes/enfoques** (¿esto se parece a qué? ¿en qué se diferencia?).
- **Curiosidades y hechos poco conocidos** que sorprendan al lector.

El tono debe ser el de un libro de divulgación de calidad, no el de un manual técnico. Se busca **entender profundamente**, no memorizar.

### profundizar.md — Más allá del módulo

Contenido para expandir horizontes una vez dominado lo básico:

- **Casos de estudio reales**: ¿quién usó esto y qué construyó? ¿qué salió bien? ¿qué salió mal?
- **Historias y anécdotas**: momentos clave, guerras de tecnología, decisiones polémicas.
- **Qué se puede crear**: proyectos, sistemas, aplicaciones — ideas concretas que despierten la imaginación.
- **Conexiones con otros temas**: este tema, ¿con qué otros temas inesperados se relaciona?
- **Debates abiertos y controversias**: ¿qué discute la comunidad? ¿qué no está resuelto?
- **Futuro y tendencias**: ¿hacia dónde va esto?

## Flujo para crear un nuevo módulo

Cuando el usuario pida crear un módulo de estudio:

1. **Identificar el tema** (carpeta padre). Si la carpeta del tema no existe, crearla.
2. **Crear la carpeta del módulo** con nombre en kebab-case dentro del tema.
3. **Copiar la estructura base** de `.module-template/` o crear los 4 archivos manualmente.
4. **Investigar** (si es posible) para generar contenido relevante y actualizado.
5. **Generar contenido sustancial** — no placeholders vacíos. Cada archivo debe ser una lectura valiosa por sí mismo.
6. **Escribir en tono de divulgación experta**: claro, entretenido y profundo. Como un ensayo o un buen libro de no-ficción técnica.
7. **No incluir ejercicios, quizzes ni autoevaluaciones**. El aprendizaje ocurre mediante la lectura y la reflexión.

## Reglas

- **No modificar `AGENTS.md` ni `README.md` (raíz)** a menos que el usuario lo pida explícitamente.
- **No borrar archivos** sin confirmación.
- **Usar `grep` y `glob`** para buscar contenido existente antes de crear algo nuevo.
- **Añadir tags consistentes** para facilitar búsquedas futuras.
- **Ser conciso pero rico en contenido** — cada párrafo debe aportar valor. Sin relleno.
- **No inventar URLs** — si no conocés un recurso real, indicá el tipo de recurso que se necesita buscar en lugar de generar enlaces falsos.

## Categorías de tags sugeridas

- `#fundamentos` — conceptos base
- `#avanzado` — profundización experta
- `#historico` — contexto histórico y evolución
- `#lectura` — contenido pensado para ser leído, no ejecutado
- `#crear` — ideas de proyectos y aplicaciones
- `#conexiones` — relaciones con otros temas
- `#referencia` — material de consulta
- `#pendiente` — por revisar o completar
