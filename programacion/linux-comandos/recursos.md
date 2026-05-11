# Recursos — Comandos de Linux poco comunes

## Libros

### The Linux Command Line (William Shotts)
- **Tipo**: libro
- **Autor**: William Shotts
- **Enlace**: https://linuxcommand.org/tlcl.php (gratuito en PDF)
- **Descripción**: No es solo para principiantes. Los capítulos intermedios y avanzados cubren `sed`, `awk`, `xargs` y scripting con una profundidad que la mayoría de tutoriales online ignoran. Ideal para entender no solo el *qué* sino el *por qué*.

### Unix Power Tools (O'Reilly)
- **Tipo**: libro
- **Autor**: Jerry Peek, Shelley Powers, Tim O'Reilly, Mike Loukides
- **Enlace**: Disponible en O'Reilly y librerías técnicas
- **Descripción**: Una enciclopedia de tips, trucos y comandos oscuros de Unix. No es lectura lineal: lo abrís en cualquier página y aprendés algo nuevo. Cubre comandos que ni los veteranos conocen.

### sed & awk (O'Reilly)
- **Tipo**: libro
- **Autor**: Dale Dougherty, Arnold Robbins
- **Enlace**: Disponible en O'Reilly y librerías técnicas
- **Descripción**: El libro definitivo sobre `sed` y `awk`. Estos dos comandos son lenguajes de programación en sí mismos. Este libro te lleva de "buscar y reemplazar" a procesar gigabytes de logs con un one-liner.

## Artículos

### The Art of Command Line
- **Tipo**: artículo
- **Autor**: Comunidad (iniciado por Joshua Levy)
- **Enlace**: https://github.com/jlevy/the-art-of-command-line
- **Descripción**: Una guía colaborativa en GitHub con cientos de tips de línea de comandos. Cubre desde lo básico hasta comandos que solo los administradores de sistemas conocen. Constantemente actualizada.

### Linux Performance Analysis in 60,000 Milliseconds
- **Tipo**: artículo
- **Autor**: Brendan Gregg (Netflix)
- **Enlace**: Buscar "Linux Performance Analysis in 60,000 Milliseconds" en netflixtechblog o brendangregg.com
- **Descripción**: Brendan Gregg es una leyenda del performance en Linux. Este artículo te muestra qué comandos usar en los primeros 60 segundos al investigar un problema de rendimiento. `strace`, `lsof`, `perf` y amigos en acción real.

### Command-line tools can be 235x faster than your Hadoop cluster
- **Tipo**: artículo
- **Autor**: Adam Drake
- **Enlace**: Buscar "Command line tools faster than hadoop" en adamdrake.com
- **Descripción**: Demostración impactante de cómo comandos como `awk`, `sort`, `uniq` y `xargs` procesan datos más rápido que clusters de Hadoop para ciertos problemas. Te cambia la perspectiva de cuándo tirar de herramientas simples vs complejas.

## Videos

### AT&T Archives: The UNIX Operating System
- **Tipo**: video
- **Autor**: AT&T Tech Channel / Bell Labs
- **Enlace**: Buscar en YouTube "AT&T Archives The UNIX Operating System"
- **Descripción**: Documental de los años 80 con Ken Thompson, Dennis Ritchie y Brian Kernighan explicando Unix. No habla de comandos específicos, pero entender la filosofía que los creó hace que todo encaje. Ver a Kernighan usar pipes en vivo es mágico.

### Brian Kernighan on Unix Tools
- **Tipo**: video
- **Autor**: Brian Kernighan (varias conferencias)
- **Enlace**: Buscar en YouTube "Brian Kernighan Unix" o "Brian Kernighan programming"
- **Descripción**: Kernighan (la K de K&R C y la K de AWK) hablando sobre las herramientas Unix. Su charla sobre cómo AWK fue diseñado es particularmente iluminadora: imaginá diseñar un lenguaje de procesamiento de texto en 1977 y que siga siendo insuperable 50 años después.

## Herramientas

### modern-unix (colección curada)
- **Tipo**: herramienta
- **Enlace**: https://github.com/ibraheemdev/modern-unix
- **Descripción**: Lista de reemplazos modernos para comandos clásicos: `bat` en vez de `cat`, `fd` en vez de `find`, `ripgrep` en vez de `grep`, `delta` para `diff`. Mantiene la lista actualizada con herramientas escritas en Rust que son más rápidas y con mejor UI.

### tldr (Too Long; Didn't Read)
- **Tipo**: herramienta
- **Enlace**: https://tldr.sh/
- **Descripción**: Páginas de manual simplificadas y con ejemplos reales. En lugar de leer 2000 líneas de `man tar`, `tldr tar` te da 10 ejemplos prácticos que cubren el 90% de los casos de uso. Instalable con `npm install -g tldr` o `pip install tldr`.
