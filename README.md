# mis-notas

Una biblioteca personal de conocimiento — diseñada para **leer, aprender y convertirse en experto** en temas de interés. Cada módulo es una lectura autosuficiente que combina explicaciones, historia, casos reales e inspiración para crear.

**No es un repositorio de ejercicios.** Es una colección de conocimiento narrado para leer, disfrutar y entender profundamente.

## Filosofía

Cada módulo responde tres preguntas:
1. **¿Qué es y cómo funciona?** — Conceptos explicados con claridad y analogías.
2. **¿Por qué existe y qué historias hay detrás?** — Origen, evolución, decisiones de diseño.
3. **¿Qué puedo crear o entender gracias a esto?** — Aplicaciones reales, ideas, conexiones.

## Estructura

```
mis-notas/
├── README.md              # Este archivo
├── AGENTS.md              # Instrucciones para asistentes IA
├── .gitignore
├── .module-template/      # Plantilla para nuevos módulos
│   ├── README.md          #   Metadatos, ¿qué voy a aprender?, mapa de lectura
│   ├── recursos.md        #   Lecturas complementarias curadas
│   ├── notas.md           #   El conocimiento: conceptos, historia, narrativa
│   └── profundizar.md     #   Casos de estudio, historias, ideas para crear
├── programacion/
│   └── lua/               # Módulo de ejemplo
├── filosofia/
├── matematicas/
├── ciencia/
├── historia/
├── productividad/
└── otros/
```

Cada **tema** es una carpeta. Dentro hay **módulos** (subcarpetas) con 4 archivos.

## Cómo crear un nuevo módulo

### Con IA (recomendado)

Decile a tu asistente IA algo como:

> "Creá un módulo de estudio sobre [tema] en la carpeta [tema]"

La IA lee `AGENTS.md` y genera los 4 archivos con contenido de lectura enriquecido.

### Manual

```bash
cp -r .module-template tema/nombre-del-modulo
```

## Nomenclatura

- **Temas**: una palabra, minúsculas, en español (`programacion`, `filosofia`)
- **Módulos**: kebab-case descriptivo (`algoritmos-ordenamiento`, `estoicismo`)
- **Archivos**: minúsculas, con guiones si es necesario

## Convenciones

- Todo el contenido en **español**
- Formato **Markdown** (compatible con Obsidian y cualquier editor)
- **Sin ejercicios ni autoevaluaciones** — aprendizaje mediante lectura y reflexión
- Tags para facilitar búsquedas (`#fundamentos`, `#historico`, `#crear`, `#lectura`)
- Recursos con enlaces funcionales y descripción de qué aporta cada uno
