# AGENTS.md — Instrucciones para asistentes IA

Este archivo define cómo debe comportarse un asistente de IA al trabajar en este repositorio (`mis-notas`), una base de conocimiento personal para organizar temas de estudio.

## Propósito del repositorio

Este es un espacio para que el usuario pueda leer, estudiar y documentar temas de su interés. El asistente IA actúa como un **curador y generador de módulos de estudio** de alta calidad.

## Idioma

**Todo el contenido debe generarse en español**, independientemente del idioma en que se haga la petición.

## Estructura de un módulo de estudio

Cada módulo es una carpeta dentro del tema correspondiente. Un módulo **siempre** debe contener estos 4 archivos:

```
tema/nombre-del-modulo/
├── README.md        # Metadatos, objetivos de aprendizaje, prerrequisitos, índice
├── recursos.md      # Lista curada de materiales de estudio (libros, artículos, videos, cursos)
├── notas.md         # Conceptos clave, resúmenes, diagramas conceptuales
└── ejercicios.md    # Preguntas de reflexión, ejercicios prácticos, proyectos sugeridos
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

## Objetivos de aprendizaje
- Objetivo 1
- Objetivo 2

## Prerrequisitos
- Conocimiento previo necesario

## Roadmap
1. Tema 1
2. Tema 2
3. Tema 3
```

### recursos.md

Cada recurso debe seguir este formato:

```markdown
## Categoría (Libros / Artículos / Videos / Cursos / Herramientas)

### Título del recurso
- **Tipo**: libro | artículo | video | curso | herramienta
- **Autor**: Nombre
- **Enlace**: URL
- **Descripción**: 1-2 frases sobre por qué es relevante y qué aporta
```

### notas.md

Formato libre pero organizado por secciones con `##` encabezados. Incluir:
- Conceptos clave con definiciones claras
- Diagramas en ASCII o referencias a herramientas externas
- Relaciones entre conceptos
- Analogías útiles

### ejercicios.md

- Preguntas de reflexión (¿por qué?, ¿cómo?, ¿cuándo?)
- Ejercicios prácticos (si aplica)
- Mini-proyectos sugeridos
- Autoevaluación

## Flujo para crear un nuevo módulo

Cuando el usuario pida crear un módulo de estudio:

1. **Identificar el tema** (carpeta padre). Si la carpeta del tema no existe, crearla.
2. **Crear la carpeta del módulo** con nombre en kebab-case dentro del tema.
3. **Copiar la estructura base** de `.module-template/` o crear los 4 archivos manualmente.
4. **Investigar** (si es posible) para generar contenido relevante y actualizado.
5. **Generar contenido sustancial** — no placeholders vacíos. Cada archivo debe tener valor real.
6. **Usar los emojis** solo donde aporten claridad visual (tablas de metadatos, categorías de recursos). No abusar.

## Comandos útiles

| Comando | Uso |
|---------|-----|
| `find . -name "*.md"` | Listar todos los módulos y notas |
| `grep -r "tag" --include="*.md"` | Buscar por tag |
| `cp -r .module-template tema/nuevo-modulo` | Crear módulo desde plantilla |

## Reglas

- **No modificar `AGENTS.md` ni `README.md` (raíz)** a menos que el usuario lo pida explícitamente.
- **No borrar archivos** sin confirmación.
- **Usar `grep` y `glob`** para buscar contenido existente antes de crear algo nuevo.
- **Añadir tags consistentes** para facilitar búsquedas futuras.
- **Ser conciso pero completo** — máximo 4 líneas de explicación por recurso, notas claras y directas.
- **No inventar URLs** — si no conocés un recurso real, indicá el tipo de recurso que se necesita buscar en lugar de generar enlaces falsos.

## Categorías de tags sugeridas

- `#fundamentos` — conceptos base
- `#avanzado` — profundización
- `#practico` — ejercicios y proyectos
- `#teoria` — contenido teórico
- `#referencia` — material de consulta
- `#pendiente` — por revisar o completar
