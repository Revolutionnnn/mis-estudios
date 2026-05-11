# Notas — Lua

## El origen: tres brasileños, una petrolera y un lenguaje que no debió existir

Corre 1993. En la PUC-Rio de Brasil, el profesor Roberto Ierusalimschy y sus colegas Waldemar Celes y Luiz Henrique de Figueiredo tienen un problema. Petrobras, la petrolera estatal brasileña, les había encargado programas de simulación geológica. Los ingenieros necesitaban poder modificar parámetros de simulación sin recompilar todo el programa en Fortran. Necesitaban un lenguaje de configuración... pero uno que también pudiera expresar lógica.

En esa época, Tcl era la opción dominante para scripting embebido. Pero tenía un problema serio: su sintaxis era extraña, su modelo de datos era pobre (todo era string) y no escalaba bien. Los brasileños probaron Tcl y dijeron: "podemos hacer algo mejor".

Así nació SOL (Simple Object Language), el ancestro directo de Lua. SOL introdujo la idea de que UN solo tipo de dato compuesto (las tablas) podía reemplazar arrays, diccionarios, objetos y hasta módulos. SOL evolucionó a Lua —"luna" en portugués, en referencia a SOL, el "sol" anterior— y la primera versión pública salió en 1994.

Pero lo que realmente disparó la adopción de Lua no fue Petrobras. Fue un artículo en la revista Dr. Dobb's Journal en 1996, luego una portada en Game Developer Magazine, y sobre todo... World of Warcraft.

---

## La filosofía del "menos es más"

Lua tiene 8 tipos de datos. Python tiene docenas. JavaScript tiene 7 pero con comportamientos complejos. La librería estándar de Lua cabe en un puñado de funciones. ¿Cómo hace tanto con tan poco?

La respuesta está en su principio fundacional: **mecanismos en lugar de políticas**.

Traducción: en lugar de darte 50 características especializadas, Lua te da 5 herramientas increíblemente flexibles y te deja combinarlas como quieras.

Un ejemplo concreto: las **tablas**. En Python necesitás listas, diccionarios, sets, objetos, módulos y namedtuples. En Lua necesitás UNA sola cosa: una tabla. Todas esas estructuras se implementan con tablas.

```
Tabla como array:            {10, 20, 30, 40}
Tabla como diccionario:       {nombre="Ana", edad=30}
Tabla como objeto:            {x=10, y=20, mover=function()...end}
Tabla como módulo:            {func1=..., func2=...}
Tabla como conjunto (set):    {[elemento]=true, [otro]=true}
```

Esto no es un truco ni una limitación. Es una decisión deliberada de diseño que hace que el lenguaje entero quepa en tu cabeza.

Pero la verdadera magia no son las tablas. Son las **metatables**.

---

## Metatables: cuando tus datos tienen superpoderes

Imaginá que tenés una tabla común y corriente. Ahora imaginá que podés decirle: "cuando alguien intente sumar esta tabla con otra, ejecutá esta función". O: "cuando alguien busque una clave que no existe, en lugar de devolver nil, buscá en esta otra tabla". Eso son las metatables.

```lua
local vector = {x = 3, y = 4}
-- Normalmente vector + vector daría error
-- Pero con una metatable...

local mt = {
  __add = function(a, b)
    return {x = a.x + b.x, y = a.y + b.y}
  end
}
setmetatable(vector, mt)
```

Las metatables son lo que permite que Lua tenga "orientación a objetos" sin tener clases. Son lo que permite que puedas sobrecargar operadores. Son el secreto de la flexibilidad de Lua.

Para que se entienda: en la mayoría de lenguajes, los operadores como `+` están definidos por el compilador y no los podés cambiar. En C++ podés sobrecargarlos pero solo dentro de clases. En Lua, cualquier tabla puede tener cualquier comportamiento. Es como si los objetos de JavaScript tuvieran `Proxy` incorporado por defecto, pero sin la complejidad.

---

## Las corutinas: concurrencia sin hilos

Otra joya del diseño de Lua. Las corutinas permiten tener múltiples "hilos" de ejecución que cooperan entre sí, cediendo el control voluntariamente. No son threads del sistema operativo: todo corre en un solo hilo.

¿Para qué sirve esto? Imaginá un videojuego con 100 NPCs. Cada NPC tiene su propia "rutina": caminar hasta un punto, esperar 3 segundos, decir algo, caminar a otro lado. Sin corutinas necesitás máquinas de estado complejas con temporizadores. Con corutinas, cada NPC es una función lineal:

```lua
function comportamientoNPC()
  while true do
    caminarHacia(puntoA)
    esperar(3)         -- yield interno
    decir("Hola")
    caminarHacia(puntoB)
    esperar(5)
  end
end
```

El scheduler del juego va rotando entre corutinas, ejecutando un poquito de cada una. El código se lee como si fuera secuencial, pero en realidad es cooperativo. Esto es lo que usan los juegos en Lua para manejar comportamientos complejos sin volverse locos con callbacks.

---

## ¿Dónde está Lua escondido?

Lua es probablemente el lenguaje más usado que menos gente conoce por nombre. Está en:

### Videojuegos (su fortaleza histórica)
- **World of Warcraft**: Toda la UI y los addons están en Lua. La comunidad de addons de WoW es una de las más grandes del mundo y entera programa en Lua.
- **Roblox**: Usa Luau, un dialecto de Lua con tipos graduales. Millones de juegos creados por usuarios, todos en Luau.
- **Angry Birds** (versiones originales): Hecho con Corona SDK (Lua).
- **Baldur's Gate 3, Factorio, Civilization VI, Don't Starve**: Todos usan Lua para scripting de juego.
- **LÖVE2D**: Framework para juegos indie 2D. Juegos como "Move or Die" se hicieron con esto.

### Editores y herramientas
- **Neovim**: La gran migración de Vimscript a Lua. Hoy todo el ecosistema de plugins de Neovim está en Lua. La promesa: misma velocidad de Vim, pero con un lenguaje real.
- **Wireshark**: Los plugins para diseccionar protocolos de red se escriben en Lua.
- **Adobe Lightroom**: Los plugins son en Lua.
- **MPV**: Reproductor de video. Su sistema de scripting de UI es Lua.
- **Redis**: Lua scripting para operaciones atómicas complejas en el servidor.
- **Nginx / OpenResty**: Lua metido dentro del servidor web para lógica de alto rendimiento.

### Sistemas embebidos
- **NodeMCU**: Firmware para ESP8266/ESP32 que se programa en Lua. Imaginate programar un microcontrolador WiFi con scripts de Lua en lugar de C.
- **AwesomeWM**: Window manager para Linux configurado completamente en Lua.

---

## El código de Lua: fragmentos reveladores

No son tutoriales paso a paso. Son fragmentos que muestran la esencia del lenguaje.

### Los índices empiezan en 1 (por un motivo histórico)

```lua
local t = {"lunes", "martes", "miércoles"}
print(t[1])  --> "lunes"  (no "martes")
```

Esto escandaliza a programadores de C/Java/Python. ¿Por qué 1 y no 0? La respuesta no es técnica: es humana. Lua fue diseñado para ingenieros de Petrobras que no eran programadores. Para un no-programador, contar desde 1 es natural. Y como Lua no es un subset de C, no tiene por qué seguir sus convenciones.

Curiosamente, internamente los arrays de Lua SÍ empiezan en 0. `t[0]` es perfectamente válido. Solo que el operador `#` y las funciones de la librería estándar ignoran el índice 0.

### Funciones que devuelven funciones (closures elegantes)

```lua
function saludar(saludo)
  return function(nombre)
    return saludo .. ", " .. nombre
  end
end

local hola = saludar("Hola")
local bonjour = saludar("Bonjour")

print(hola("Ana"))     --> Hola, Ana
print(bonjour("Ana"))  --> Bonjour, Ana
```

Esto es un closure: `saludo` queda "capturado" en la función retornada. En lenguajes como Java necesitás una clase entera para esto. En Lua es una línea. Este patrón es la base de cómo Lua implementa encapsulación sin clases.

### Herencia simple con metatables

```lua
-- Definimos un "prototipo"
local Animal = {}
function Animal:nuevo(nombre)
  return setmetatable({nombre = nombre}, {__index = self})
end
function Animal:hablar()
  print(self.nombre .. " hace ruido")
end

-- "Herencia": otro prototipo que delega en Animal
local Perro = Animal:nuevo("Perro")
function Perro:hablar()
  print(self.nombre .. " ladra")
end

local fido = Perro:nuevo("Fido")
fido:hablar()  --> Fido ladra
```

No hay clases. No hay `extends`. Solo prototipos y delegación vía `__index`. Es como JavaScript antes de ES6, pero más limpio.

---

## Comparación: Lua al lado de sus pares

### Lua vs Python
Ambos son dinámicos e interpretados. Pero:
- **Python** es una navaja suiza con 200 herramientas. **Lua** es un bisturí.
- Python tiene su propio ecosistema (Django, NumPy, etc.). Lua vive DENTRO de otros programas.
- Python es pesado para embeber (~30 MB). Lua pesa ~300 KB.
- La librería estándar de Python tiene módulos para todo. La de Lua es minimalista a propósito: cuando Lua está embebido, la aplicación host provee las funciones.

### Lua vs JavaScript
Comparten origen en los 90s y herencia prototipal. Pero:
- JavaScript evolucionó en la guerra de navegadores (con prisas). Lua evolucionó en un laboratorio académico (con calma).
- JavaScript tiene `this`, `prototype`, `class`, `new`, `bind`, `call`, `apply` — un zoológico. Lua tiene metatables y `:`.
- El estándar de JavaScript (ECMAScript) mide miles de páginas. El manual de Referencia de Lua mide ~100.

### Lua vs Lisp
Son parientes espirituales. Ambos creen en "mecanismos, no políticas". Ambos tienen pocas primitivas y el resto emerge. Pero Lua cambió los paréntesis por `end` e hizo que las tablas (no las listas) sean el tipo central.

---

## Resumen: lo que no deberías olvidar

- Lua nació en Brasil en 1993 para que ingenieros de Petrobras pudieran configurar simulaciones sin recompilar Fortran. Hoy está en WoW, Neovim, Redis y Marte (literalmente: la NASA lo usó en simulaciones del rover Spirit).
- Su secreto es dar pocas herramientas pero increíblemente flexibles: tablas, metatables y corutinas. Con eso construís lo que quieras.
- No compite con Python o JavaScript como lenguaje standalone. Compite como lenguaje embebido: vive DENTRO de otras aplicaciones.
- LuaJIT es uno de los compiladores JIT más rápidos jamás escritos, y su FFI permite llamar a C sin bindings — algo casi mágico en un lenguaje dinámico.
- Si alguna vez usaste un addon de WoW, configuraste Neovim, escribiste un script de Redis o jugaste un juego de Roblox, ya programaste en Lua sin saberlo.
