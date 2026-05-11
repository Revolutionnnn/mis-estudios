# Notas — Comandos de Linux poco comunes

## ¿Por qué existen tantos comandos extraños?

En 1969, Ken Thompson tenía un mes libre mientras su esposa y su hijo estaban de vacaciones. Escribió un sistema operativo. Tres semanas después, Unix arrancaba en una PDP-7.

Lo genial no fue Unix. Fue la **filosofía Unix**: programas que hacen una sola cosa, y la hacen bien. Programas que se conectan entre sí con pipes. Texto como interfaz universal.

De esa filosofía nacieron cientos de pequeñas herramientas. Algunas sobrevivieron y se volvieron famosas (`grep`, `ls`, `cd`). Otras quedaron en las sombras pero son igual de brillantes. Este módulo es sobre esas joyas ocultas.

Pensá en la terminal como una navaja suiza donde solo usás la hoja principal. Hay otras 20 herramientas esperando a que las descubras.

---

## Procesamiento de texto como un cirujano

### `sed`: el editor que no abre archivos

`sed` (Stream EDitor) edita texto línea por línea sin cargar el archivo en memoria. Nació en 1974 en Bell Labs, escrito por Lee McMahon. Su sintaxis es críptica pero su poder es absurdo.

```
# Cambiá la extensión de cientos de archivos en un script
find . -name "*.html" | sed 's/\.html$/.php/' | xargs -I {} mv {}.html {}.php

# Borrá líneas en blanco (incluyendo las que tienen espacios)
sed '/^[[:space:]]*$/d' archivo.txt

# Imprimí solo de la línea 10 a la 20
sed -n '10,20p' archivo.log

# Reemplazo en múltiples archivos (in-place)
sed -i 's/foo/bar/g' *.txt
```

Lo que hace especial a `sed`: puede procesar archivos de gigabytes sin consumir memoria. Un script de `sed` de 20 caracteres puede hacer lo que haría un programa de Python de 30 líneas.

### `awk`: un lenguaje de programación disfrazado de comando

AWK son las iniciales de sus creadores: Alfred Aho, Peter Weinberger y Brian Kernighan. Lo crearon en 1977 para procesar reportes de texto. Hoy es un lenguaje completo con variables, arrays asociativos, funciones y expresiones regulares.

La magia de `awk` es que divide automáticamente cada línea en columnas (`$1`, `$2`, `$3`...) y ejecuta un bloque de código por línea.

```
# Sumá la tercera columna de un CSV
awk -F',' '{suma += $3} END {print suma}' datos.csv

# Encontrá valores duplicados por la columna 2
awk -F',' 'seen[$2]++ {print $2 " duplicado"}' datos.csv

# Promedio de la columna 4, agrupado por la columna 1
awk '{suma[$1] += $4; count[$1]++} END {for (k in suma) print k, suma[k]/count[k]}' ventas.txt

# Top 5 IPs en un log de Apache
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -5
```

Ese último one-liner merece una pausa. En una línea procesás un archivo de log, extraés IPs, las contás, las ordenás y mostrás las 5 más frecuentes. Sin importar si el log tiene 10 líneas o 10 millones.

**Dato curioso**: Brian Kernighan cuenta que el nombre AWK iba a ser un chiste interno, pero les gustó cómo sonaba. Aho, Weinberger y Kernighan. Hoy nadie sabe pronunciarlo: algunos dicen "awk" (como hawk), otros "áuk" (como audit).

### `tr`: traductor de caracteres, el comando más subestimado

`tr` (Translate) convierte, elimina o comprime caracteres. Es diminuto. Pero resuelve problemas que de otra forma requerirían scripts.

```
# Convertí todo a minúsculas
tr '[:upper:]' '[:lower:]' < archivo.txt

# Eliminá todos los saltos de línea (uní líneas)
tr -d '\n' < archivo.txt

# Comprimí espacios múltiples en uno solo
tr -s ' ' < archivo.txt

# ROT13 (cifrado clásico, solo por diversión)
echo "hola mundo" | tr 'A-Za-z' 'N-ZA-Mn-za-m'

# Reemplazá comas por tabuladores (útil para CSVs)
tr ',' '\t' < datos.csv
```

`tr` opera byte a byte. No sabe de líneas ni de campos. Es brutalmente simple y por eso es tan rápido.

### `cut` y `paste`: las tijeras y el pegamento

`cut` extrae columnas. `paste` las une. Son hermanos complementarios que llevan décadas resolviendo problemas de formato.

```
# Extraé las columnas 1 y 3 de un CSV
cut -d',' -f1,3 datos.csv

# Extraé los caracteres 5 al 10 de cada línea
cut -c5-10 archivo.txt

# Uní dos archivos columna a columna
paste archivo1.txt archivo2.txt

# Creá un CSV juntando columnas de tres archivos distintos
paste -d',' nombres.txt edades.txt ciudades.txt > personas.csv
```

### `column`: el formateador que no sabías que existía

`column` toma texto desordenado y lo convierte en columnas alineadas. Es de esos comandos que, una vez que lo conocés, no entendés cómo vivías sin él.

```
# Convertí una lista simple en columnas
mount | column -t

# Formateá la salida de un comando como tabla legible
ps aux | column -t

# Creá una tabla a partir de datos separados por dos puntos
column -s':' -t /etc/passwd
```

---

## Buscar y encontrar de verdad

### `find` avanzado: más allá de `-name`

Todo el mundo sabe `find . -name "*.txt"`. Pero `find` es un motor de búsqueda con operadores booleanos, ejecución de comandos y filtrado por metadatos.

```
# Archivos modificados en las últimas 24 horas
find . -mtime -1

# Archivos mayores a 100 MB (y mostrá cuánto pesan)
find . -size +100M -exec ls -lh {} \;

# Archivos vacíos
find . -empty

# Borrá todos los .tmp que tengan más de 7 días
find . -name "*.tmp" -mtime +7 -delete

# Buscá archivos que NO coinciden
find . -not -name "*.md"

# Combinación booleana: PDFs O DOCs modificados hoy
find . \( -name "*.pdf" -o -name "*.doc" \) -mtime -1
```

**Pero cuidado**: `find -exec` crea un proceso por cada archivo. Para miles de archivos, usá `xargs`.

### `xargs`: el puente entre comandos

`xargs` toma una lista de la entrada estándar y la convierte en argumentos para otro comando. Es el pegamento universal de Unix.

```
# La diferencia entre esto:
find . -name "*.tmp" | xargs rm        # Un solo rm con todos los archivos
# y esto:
find . -name "*.tmp" -exec rm {} \;    # Un rm por cada archivo (lento)

# Procesamiento paralelo con 4 hilos
find . -name "*.jpg" | xargs -P 4 -I {} convert {} -resize 50% {}

# Buscar en archivos cuyos nombres vienen de otro comando
find . -name "*.log" | xargs grep "ERROR"

# Contar líneas totales de todos los .py del proyecto
find . -name "*.py" | xargs wc -l | tail -1
```

El `-P` de `xargs` es un secreto a voces: paralelismo real en una línea de comandos. Sin dependencias, sin código multihilo.

### `comm`: el comando que te dice qué tienen en común dos listas

`comm` compara dos archivos ordenados y te dice qué líneas están solo en el primero, solo en el segundo, o en ambos.

```
# ¿Qué usuarios hay en server1 que no están en server2?
comm -23 <(sort usuarios_server1.txt) <(sort usuarios_server2.txt)

# ¿Qué paquetes están instalados en ambos servidores?
comm -12 <(ssh srv1 dpkg --get-selections | sort) <(ssh srv2 dpkg --get-selections | sort)
```

La notación `<(...)` se llama "process substitution". Es como crear un archivo temporal anónimo con la salida de un comando. Elegante y sin archivos basura.

---

## Redes y depuración

### `strace`: espiá lo que hace tu programa

`strace` (System Trace) intercepta y muestra todas las llamadas al sistema que hace un proceso. Es como ponerle un micrófono oculto a tu programa.

```
# ¿Qué archivos abre este comando?
strace -e openat cat /etc/hostname

# ¿Por qué este programa está colgado? (mirá en qué llamada se quedó)
strace -p 12345

# Contá qué syscalls usa más tu programa
strace -c mi_programa

# Filtro avanzado: solo llamadas de red
strace -e trace=network curl ejemplo.com
```

`strace` es la herramienta de debugging que usás cuando `printf` no alcanza y GDB es demasiado. Si un programa falla misteriosamente con "permission denied" o "file not found" y no sabés qué archivo está buscando, `strace` te lo dice sin pestañear.

### `lsof`: todo es un archivo (y este comando te lo demuestra)

`lsof` (List Open Files) muestra todos los archivos abiertos por procesos. En Unix, "archivo" incluye sockets de red, pipes, dispositivos y directorios.

```
# ¿Qué proceso está usando el puerto 8080?
lsof -i :8080

# ¿Qué archivos tiene abiertos este proceso?
lsof -p 12345

# ¿Quién está usando este archivo? (para saber si podés borrarlo)
lsof /ruta/al/archivo

# ¿Qué conexiones de red tiene establecidas el usuario?
lsof -u revo -i
```

### `nc` (netcat): la navaja suiza de redes

`nc` lee y escribe datos a través de conexiones de red. Es tan simple que asusta. Podés hacer de todo: desde chatear hasta transferir archivos o crear servidores mínimos.

```
# Servidor que escucha en el puerto 4444
nc -l 4444

# Cliente que se conecta (chat improvisado)
nc localhost 4444

# Transferir un archivo (servidor)
nc -l 4444 < archivo_grande.tar.gz

# Transferir un archivo (cliente)
nc servidor 4444 > archivo_grande.tar.gz

# Escanear puertos abiertos en un rango
nc -zv localhost 20-100

# Servidor HTTP mínimo (devuelve una respuesta cruda)
while true; do echo -e "HTTP/1.1 200 OK\n\nHola" | nc -l -p 8080 -q 1; done
```

### `tee`: lee una cosa y la escribe en dos lugares

`tee` toma la entrada estándar, la escribe a uno o más archivos, Y la pasa a la salida estándar. Es como una bifurcación en una tubería de agua.

```
# Guardá la salida en un log Y mirala en tiempo real
./script_largo.sh | tee salida.log

# Guardá en múltiples archivos a la vez
echo "importante" | tee archivo1.txt archivo2.txt

# Append en lugar de sobrescribir
echo "otra línea" | tee -a log.txt

# El clásico: instalación guardada para debugging
make install 2>&1 | tee build.log
```

---

## Joyas ocultas

### `mktemp`: archivos temporales sin riesgo de colisión

Crear archivos temporales con nombres aleatorios es una operación sorprendentemente peligrosa (race conditions, colisiones). `mktemp` lo resuelve de forma segura.

```
# Creá un archivo temporal
tmpfile=$(mktemp)
echo "datos temporales" > "$tmpfile"
# ... usá el archivo ...
rm "$tmpfile"

# Creá un directorio temporal
tmpdir=$(mktemp -d)
cp archivos/* "$tmpdir/"
```

### `script` y `scriptreplay`: grabá tu terminal

`script` graba una sesión de terminal completa, incluyendo colores y timestamps. `scriptreplay` la reproduce como una película.

```
# Grabá una sesión
script -t 2> timing.txt sesion.txt
# ... hacé tus comandos ...
exit

# Reproducila (con el timing original)
scriptreplay timing.txt sesion.txt
```

Se usa para demostraciones, documentación, o para que quede registro de exactamente qué comandos ejecutaste en un servidor de producción.

### `fc`: arreglá ese comando kilométrico sin reescribirlo

`fc` (Fix Command) es un builtin de bash que abre tu último comando en el editor para que lo edites y lo ejecutes de nuevo.

```
# Abrí el último comando en tu editor ($EDITOR)
fc

# Abrí los últimos 5 comandos
fc -5

# Ejecutá el comando número 42 del historial sin editarlo
fc -s 42
```

Si escribiste un comando de 200 caracteres y tiene un typo en el medio, `fc` es mil veces mejor que usar las flechas.

### `shred`: borrá archivos de verdad

Cuando hacés `rm`, el archivo no se borra realmente: solo se marca el espacio como libre. Los datos siguen ahí hasta que algo los sobrescribe. `shred` sobrescribe el archivo múltiples veces con datos aleatorios antes de borrarlo.

```
# Sobrescribí 3 veces con datos aleatorios y borrá
shred -n 3 -z -u archivo_secreto.txt

# Sobrescribí un disco entero (peligroso, irreversible)
shred -n 1 -z /dev/sdb
```

### `watch`: ejecutá un comando cada N segundos

```
# Monitoreá el espacio en disco cada 2 segundos
watch -n 2 df -h

# Resaltá diferencias entre ejecuciones
watch -d ls -la

# Monitoreá conexiones de red activas
watch -n 1 'ss -tunap'
```

### `yes`: el comando más inútil... hasta que lo necesitás

`yes` imprime "y" (o lo que le digas) infinitamente.

```
# Respondé "yes" a todas las preguntas de un script
yes | ./script_que_pregunta_mucho

# Generá un archivo de 1 GB lleno de "hola"
yes "hola" | head -c 1G > archivo_grande.txt

# Probá throughput de un pipe
yes | pv > /dev/null
```

---

## Recetas: combinaciones que resuelven problemas reales

### Encontrá los 10 directorios más pesados
```bash
du -h --max-depth=1 | sort -hr | head -10
```

### Encontrá archivos duplicados por hash
```bash
find . -type f -exec md5sum {} \; | sort | uniq -w32 -d
```

### Contá líneas de código por lenguaje en un proyecto
```bash
find . -name "*.py" | xargs wc -l | tail -1
```

### Monitoreá en vivo las IPs que más golpean tu servidor web
```bash
tail -f access.log | awk '{print $1}' | while read ip; do echo "$(date): $ip"; done
```

### Buscá y reemplazá recursivamente en todos los archivos de un tipo
```bash
find . -name "*.txt" -exec sed -i 's/viejo/nuevo/g' {} \;
```

### Creá un backup con timestamp
```bash
tar -czf "backup_$(date +%Y%m%d_%H%M%S).tar.gz" /ruta/a/datos
```

---

## Resumen: lo que no deberías olvidar

- La terminal de Linux es un lenguaje de programación en sí mismo. No son "comandos sueltos": son verbos (`sed`, `awk`), filtros (`grep`, `tr`), conectores (`|`, `xargs`) y recolectores (`sort`, `uniq`) que se componen.
- El 80% de los problemas de "necesito procesar estos datos" se resuelven con una combinación de 3 o 4 comandos encadenados. No necesitás Python, Pandas ni Excel.
- `strace` y `lsof` son tus ojos cuando algo falla y no sabés por qué. Son el equivalente a "ponerle un logger a todo" sin modificar una línea de código.
- Los comandos "oscuros" (`mktemp`, `shred`, `script`, `tee`) existen porque alguien, en algún momento, tuvo un problema muy concreto y lo resolvió de forma genial. Aprenderlos es heredar décadas de ingeniería de sistemas.
- La filosofía Unix de 1969 sigue siendo la mejor herramienta de productividad para programadores. No es nostalgia: es pragmatismo que sobrevivió 50 años por una razón.
