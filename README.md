# mis-notas

Base de conocimiento personal — un lugar para organizar, estudiar y documentar temas de interés usando inteligencia artificial como asistente.

## Estructura

```
mis-notas/
├── README.md              # Este archivo
├── AGENTS.md              # Instrucciones para asistentes IA
├── .gitignore
├── .module-template/      # Plantilla para nuevos módulos de estudio
│   ├── README.md
│   ├── recursos.md
│   ├── notas.md
│   └── ejercicios.md
├── programacion/
├── filosofia/
├── matematicas/
├── ciencia/
├── historia/
├── productividad/
└── otros/
```

Cada **tema** es una carpeta. Dentro de cada tema hay **módulos de estudio** (subcarpetas), cada uno con la estructura de `.module-template/`.

## Cómo crear un nuevo módulo de estudio

### Con IA (recomendado)

Decile a tu asistente IA algo como:

> "Creá un módulo de estudio sobre [tema] en la carpeta [tema]"

La IA seguirá las instrucciones de `AGENTS.md` para generar:
- `README.md` con objetivos, prerrequisitos y estructura
- `recursos.md` con libros, artículos, videos y cursos curados
- `notas.md` con apuntes y conceptos clave
- `ejercicios.md` con preguntas de reflexión o práctica

### Manual

```bash
cp -r .module-template tema/nombre-del-modulo
```

Luego editá cada archivo.

## Nomenclatura

- **Carpetas de temas**: una palabra, minúsculas, en español (ej. `programacion`, `filosofia`)
- **Módulos**: kebab-case descriptivo (ej. `algoritmos-ordenamiento`, `estoicismo`)
- **Archivos**: minúsculas, con guiones si es necesario

## Convenciones

- Todo el contenido en **español**
- Formato **Markdown**
- Usar `tags` en los frontmatter o al inicio de cada archivo para facilitar búsquedas
- Mantener los recursos con enlaces funcionales y breve descripción de por qué son relevantes
- Fecha de creación y última actualización en cada módulo
