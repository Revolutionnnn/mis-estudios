# Ejercicios — Lua

## Preguntas de reflexión

1. ¿Por qué Lua usa tablas como única estructura de datos compuesta en lugar de tener arrays, diccionarios y objetos separados? ¿Qué ventajas y desventajas tiene este diseño?
2. ¿En qué casos usarías un closure en lugar de una metatable con `__index` para implementar encapsulación?
3. Si Lua está diseñado para ser embebido, ¿por qué sigue siendo útil como lenguaje standalone?
4. ¿Por qué Lua indexa los arrays desde 1 en lugar de 0? ¿Qué implicaciones prácticas tiene esto al portar código desde otros lenguajes?
5. ¿Cuándo conviene usar LuaJIT en lugar de Lua estándar, y cuándo no?
6. ¿Qué rol juega el garbage collector de Lua en un contexto embebido (ej. dentro de un motor de juego) donde el rendimiento es crítico?

## Ejercicios prácticos

### 1. Contador de palabras con tablas
Escribí una función que reciba un string y devuelva una tabla donde cada clave es una palabra y el valor es la cantidad de veces que aparece.
```lua
-- Entrada: "hola mundo hola"
-- Salida: { hola = 2, mundo = 1 }
```

### 2. Implementar una lista enlazada
Usando tablas, implementá `crearLista()`, `agregar(lista, valor)`, `buscar(lista, valor)` y `imprimir(lista)`.

### 3. Sistema de eventos simple
Crea una tabla `EventEmitter` que permita registrar callbacks con `on(evento, callback)` y dispararlos con `emit(evento, ...)`. Usá closures para mantener el estado interno.

### 4. Sobrecarga de operadores con metatables
Crea un tipo `Vector2D` con campos `x` e `y`. Definí los metamétodos `__add`, `__sub`, `__mul` (producto escalar) y `__tostring`. Probá:
```lua
local v1 = Vector2D:nuevo(3, 4)
local v2 = Vector2D:nuevo(1, 2)
print(v1 + v2)   --> Vector2D(4, 6)
print(v1 - v2)   --> Vector2D(2, 2)
print(v1 * v2)   --> 11  (3*1 + 4*2)
print(v1)        --> "(3, 4)"
```

### 5. Planificador de corutinas simple
Implementá un scheduler que mantenga una cola de corutinas y las ejecute en round-robin (cada una avanza un paso cuando se llama a `scheduler:step()`). Probá creando 3 corutinas que impriman números y hagan `coroutine.yield()`.

### 6. Caché con memoización
Escribí una función `memoizar(fn)` que devuelva una versión cacheada de `fn`. La caché debe usar una tabla como diccionario y la versión cacheada debe ser un closure. Probá con la función de Fibonacci recursiva.

### 7. Análisis básico de logs
Dado un archivo de log con líneas como:
```
[INFO] 2024-01-15: Servidor iniciado
[ERROR] 2024-01-15: Conexión rechazada
[INFO] 2024-01-16: Usuario autenticado
```
Lee el archivo y generá un reporte en consola que muestre la cantidad de eventos por tipo y por día.

### 8. Servidor HTTP mínimo con LuaSocket (o copas)
Usando la librería LuaSocket (instalable con `luarocks install luasocket`), creá un servidor HTTP mínimo que:
- Escuche en `localhost:8080`
- Responda con un mensaje de bienvenida en `/`
- Devuelva 404 para cualquier otra ruta

## Mini-proyectos

### Proyecto 1: Juego "Adiviná el número"
- **Objetivo**: Un juego de consola donde la computadora elige un número aleatorio y el jugador intenta adivinarlo con pistas de "más alto" o "más bajo"
- **Duración estimada**: 1-2 horas
- **Entregable**: Script .lua que corre en consola, mantiene puntaje entre partidas y permite elegir rango de números

### Proyecto 2: Clon de `wc` (word count)
- **Objetivo**: Implementar en Lua una herramienta de línea de comandos equivalente a `wc` de Unix, que cuente líneas, palabras y bytes de archivos de texto
- **Duración estimada**: 2-3 horas
- **Entregable**: Script que soporte las opciones `-l`, `-w`, `-c` y múltiples archivos. Usá `io.lines()` y closures para procesamiento perezoso.

### Proyecto 3: To-Do list con persistencia
- **Objetivo**: Aplicación de consola para gestionar tareas (agregar, completar, eliminar, listar) que persista los datos en un archivo JSON o en un formato propio
- **Duración estimada**: 3-4 horas
- **Entregable**: Script interactivo con menú. Las tareas deben guardarse al salir y cargarse al iniciar. Usá metatables para modelar cada tarea como un "objeto".

## Autoevaluación

- [ ] Puedo escribir funciones y closures en Lua sin tener que consultar sintaxis
- [ ] Entiendo cuándo y por qué usar `local` en vez de variables globales
- [ ] Puedo crear y manipular tablas como arrays, diccionarios y prototipos
- [ ] Sé implementar herencia simple usando metatables y `__index`
- [ ] Entiendo para qué sirven las corutinas y cómo usarlas en un scheduler básico
- [ ] Puedo estructurar código Lua en módulos con `require`
- [ ] Conozco al menos 3 ámbitos diferentes donde Lua se usa en producción
- [ ] Sé la diferencia entre Lua estándar, LuaJIT y Luau, y cuándo usar cada uno
