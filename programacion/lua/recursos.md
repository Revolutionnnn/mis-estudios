# Recursos — Lua

## Libros

### Programming in Lua (PiL) — 4ta edición
- **Tipo**: libro
- **Autor**: Roberto Ierusalimschy (creador de Lua)
- **Enlace**: https://www.lua.org/pil/ (1ra edición gratuita online)
- **Descripción**: La referencia escrita por el creador. La primera edición gratuita cubre todo Lua 5.0 y sigue siendo relevante para entender la filosofía detrás de las decisiones de diseño. La cuarta edición añade enteros, utf-8 y el nuevo GC generacional.

### Lua Programming Gems
- **Tipo**: libro
- **Autor**: Varios (editado por Figueiredo, Celes e Ierusalimschy)
- **Enlace**: https://www.lua.org/gems/
- **Descripción**: No es un tutorial. Son artículos de la comunidad sobre técnicas avanzadas: cómo BalckBerry usó Lua en sus dispositivos, cómo implementar un depurador, patrones de optimización. Ideal para entender el "arte" de programar en Lua.

### Game Development with Lua (Serie de libros)
- **Tipo**: libro
- **Autor**: Varios (buscar en editoriales como CRC Press, Apress)
- **Enlace**: Buscar "game development lua book" en Amazon o sitios técnicos
- **Descripción**: Libros que explican cómo estudios reales integran Lua en sus motores de juego. Incluyen decisiones de arquitectura: ¿Lua como lógica de juego o solo como scripting de UI?

## Artículos

### The Evolution of Lua
- **Tipo**: artículo
- **Autor**: Roberto Ierusalimschy, L. H. de Figueiredo, W. Celes
- **Enlace**: https://www.lua.org/doc/hopl.pdf
- **Descripción**: Paper académico presentado en HOPL III (History of Programming Languages). La historia oficial de Lua contada por sus creadores: desde el SOL (Simple Object Language) hasta Lua 5.0. Imprescindible para entender las decisiones de diseño.

### Lua and the game industry: why it's still everywhere
- **Tipo**: artículo
- **Autor**: Buscar en Game Developer (antes Gamasutra) y blogs de desarrolladores
- **Enlace**: Buscar "why games use lua" en Google Scholar o blogs de devs
- **Descripción**: Análisis de por qué los estudios siguen eligiendo Lua para scripting después de 30 años, cuando hay alternativas más modernas (C#, Python, JavaScript).

### The Design of Lua
- **Tipo**: artículo
- **Autor**: Roberto Ierusalimschy
- **Enlace**: Buscar en Google Scholar o ACM "The design of Lua"
- **Descripción**: Explica las decisiones fundamentales de diseño (tablas como único tipo compuesto, ausencia de clases, registros ilimitados) y por qué Lua es como es.

## Videos

### Charlas de Roberto Ierusalimschy en conferencias
- **Tipo**: video
- **Autor**: Roberto Ierusalimschy
- **Enlace**: Buscar en YouTube "Roberto Ierusalimschy Lua" (Lua Workshop, Strange Loop, etc.)
- **Descripción**: Ver a Ierusalimschy explicar Lua en persona es como escuchar a Rich Hickey sobre Clojure o a Guido van Rossum sobre Python. Su charla en Strange Loop es particularmente buena para entender la filosofía.

### Documentación de Lua en Neovim (series de YouTube)
- **Tipo**: video
- **Autor**: Varios (TJ DeVries, ThePrimeagen, etc.)
- **Enlace**: Buscar en YouTube "lua neovim tutorial" o "neovim lua guide"
- **Descripción**: El ecosistema Neovim tiene excelentes explicaciones de Lua aplicado. Te muestran cómo Lua reemplazó a Vimscript y por qué fue un cambio tan importante. No son tutoriales paso a paso, sino explicaciones de diseño.

## Cursos

### Exercism — Lua Track (gratuito)
- **Tipo**: curso
- **Plataforma**: Exercism
- **Enlace**: https://exercism.org/tracks/lua
- **Descripción**: A diferencia del módulo (que es lectura), Exercism te da práctica. Los mentores revisan tu código y te enseñan modismos del lenguaje. Complementario a la lectura de este módulo.

## Herramientas

### LuaRocks
- **Tipo**: herramienta
- **Enlace**: https://luarocks.org/
- **Descripción**: El gestor de paquetes de Lua. Como pip/npm pero para Lua. Explorar los paquetes más descargados te da una idea de para qué se usa Lua en la práctica.

### LÖVE2D
- **Tipo**: herramienta
- **Enlace**: https://love2d.org/
- **Descripción**: Framework para juegos 2D completamente en Lua. No es un motor con editor visual — se programa todo. La comunidad tiene juegos comerciales hechos con esto (ej. Move or Die, Kingdom Rush en sus inicios).

### Solar2D (antes Corona SDK)
- **Tipo**: herramienta
- **Enlace**: https://solar2d.com/
- **Descripción**: Framework open source para juegos y apps 2D. Famosa por ser usada en juegos como Angry Birds (versiones originales) y The Lost City.

## Documentación oficial

### Lua 5.4 Reference Manual
- **Tipo**: artículo / referencia
- **Autor**: PUC-Rio
- **Enlace**: https://www.lua.org/manual/5.4/
- **Descripción**: Uno de los manuales de lenguaje más legibles que existen. Son solo ~100 páginas que cubren TODO el lenguaje. Si entendés este manual, entendés Lua.
