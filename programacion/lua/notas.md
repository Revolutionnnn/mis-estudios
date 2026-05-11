# Notas — Lua

## Introducción

Lua (portugués para "luna") es un lenguaje de scripting creado en 1993 en la PUC-Rio (Brasil) por Roberto Ierusalimschy, Waldemar Celes y Luiz Henrique de Figueiredo. Fue diseñado desde cero para ser **embebido** en aplicaciones escritas en C/C++, aunque también funciona perfectamente como lenguaje standalone.

Filosofía de diseño:
- **Mecanismos en lugar de políticas**: Lua provee pocas herramientas poderosas pero flexibles, y deja que el programador decida cómo combinarlas.
- **Portabilidad extrema**: Lua corre en cualquier plataforma con un compilador ANSI C. Desde microcontroladores hasta servidores.
- **Pequeño y rápido**: El intérprete completo pesa ~300 KB. LuaJIT es uno de los compiladores JIT más rápidos que existen.

---

## Tipos de datos

Lua tiene 8 tipos básicos. Es un lenguaje **dinámicamente tipado**.

| Tipo | Descripción | Ejemplo |
|------|-------------|---------|
| `nil` | Ausencia de valor (el único valor falsy además de `false`) | `nil` |
| `boolean` | Verdadero o falso | `true`, `false` |
| `number` | Números de punto flotante (doble precisión) | `3.14`, `42`, `1e-3` |
| `string` | Cadenas inmutables, pueden usar `'`, `"` o `[[ ]]` | `"hola"`, `[[multilínea]]` |
| `function` | Funciones como ciudadanos de primera clase | `function() end` |
| `table` | La única estructura de datos compuesta (arrays, diccionarios, objetos) | `{1, 2, 3}`, `{a=1}` |
| `thread` | Representa una corutina | |
| `userdata` | Datos arbitrarios de C, opacos para Lua | |

### Detalles importantes sobre tipos

- **Todo es verdadero excepto `false` y `nil`**: `0` y `""` son truthy, a diferencia de Python o JS.
- **No hay enteros nativos en Lua < 5.3**: A partir de Lua 5.3 se introducen subtipos `integer` y `float` dentro de `number`.
- **Las strings son inmutables**: Cualquier operación crea una nueva string. Lua las interna, por lo que comparar strings con `==` es O(1).
- **No hay arrays nativos**: Las tablas con índices numéricos consecutivos empezando desde 1 actúan como arrays.

---

## Variables y scoping

```lua
-- Variables globales por defecto (cuidado)
x = 10          -- global

-- Variables locales (usar siempre que sea posible)
local y = 20    -- local

-- Declaración múltiple
local a, b = 1, 2

-- Bloques do-end para crear nuevo scope
do
  local privada = "no visible afuera"
end
```

**Regla de oro**: Siempre usar `local`. Las globales son más lentas y propensas a colisiones.

---

## Tablas: la navaja suiza

La tabla es la **única estructura de datos compuesta** en Lua. Se usa para todo.

### Como array (base 1)
```lua
local nums = {10, 20, 30, 40}
print(nums[1])    --> 10
print(#nums)      --> 4  (longitud)

-- Iterar
for i = 1, #nums do
  print(nums[i])
end
```

### Como diccionario
```lua
local persona = {
  nombre = "Ana",
  edad = 30,
}

print(persona.nombre)        --> "Ana"
print(persona["nombre"])     --> "Ana"  (equivalente)

-- Agregar campos dinámicos
persona.ciudad = "Bogotá"
```

### Azúcar sintáctico para funciones
```lua
local obj = {}

function obj:saludar()
  print("hola " .. self.nombre)
end

-- Equivale a:
-- obj.saludar = function(self)
--   print("hola " .. self.nombre)
-- end
```

---

## Funciones y closures

Las funciones son **ciudadanas de primera clase**: se pueden asignar a variables, pasar como argumentos y retornar de otras funciones.

```lua
-- Función como valor
local sumar = function(a, b)
  return a + b
end

-- Azúcar sintáctico
local function sumar(a, b)
  return a + b
end
```

### Closures

Un closure es una función que captura variables de su entorno léxico.

```lua
function crearContador()
  local n = 0
  return function()
    n = n + 1
    return n
  end
end

local c1 = crearContador()
print(c1())  --> 1
print(c1())  --> 2
```

Los closures son la base de la encapsulación en Lua. No hay clases nativas; se usan closures o metatables.

---

## Metatables y metaprogramación

Cada tabla y userdata puede tener asociada una **metatable** que define su comportamiento ante ciertas operaciones.

### Metamétodos más comunes

| Metamétodo | Operación |
|-----------|-----------|
| `__add` | `a + b` |
| `__sub` | `a - b` |
| `__mul` | `a * b` |
| `__index` | `tabla.clave` cuando la clave no existe |
| `__newindex` | `tabla.clave = valor` cuando la clave no existe |
| `__call` | Llamar como función: `tabla()` |
| `__tostring` | `tostring(tabla)` |
| `__len` | `#tabla` |
| `__eq` | `a == b` |

### Programación orientada a prototipos

Lua no tiene clases. La OOP se implementa con metatables:

```lua
-- "Clase" Animal
local Animal = {}
Animal.__index = Animal

function Animal:nuevo(nombre)
  local instancia = { nombre = nombre }
  setmetatable(instancia, self)
  return instancia
end

function Animal:hablar()
  print(self.nombre .. " hace un sonido")
end

-- "Subclase" Perro
local Perro = Animal:nuevo()
Perro.__index = Perro

function Perro:hablar()
  print(self.nombre .. " ladra")
end

local perro = Perro:nuevo("Firulais")
perro:hablar()  --> Firulais ladra
```

---

## Corutinas

Las corutinas permiten **cooperar múltiples hilos de ejecución** dentro de un solo hilo OS. No son threads: son concurrentes pero no paralelas.

```lua
local co = coroutine.create(function()
  for i = 1, 3 do
    print("corutina", i)
    coroutine.yield()  -- cede el control
  end
end)

coroutine.resume(co)  --> corutina 1
coroutine.resume(co)  --> corutina 2
coroutine.resume(co)  --> corutina 3
coroutine.resume(co)  --> true (terminó)
```

Se usan para:
- Simular hilos en motores de juego (cada NPC en su corutina)
- Máquinas de estado sin callbacks anidados
- Pausar y reanudar ejecución de lógica compleja

---

## Módulos y paquetes

```lua
-- mi_modulo.lua
local M = {}

function M.saludar(nombre)
  return "Hola " .. nombre
end

return M

-- main.lua
local mi_modulo = require("mi_modulo")
print(mi_modulo.saludar("mundo"))
```

LuaRocks es el gestor de paquetes estándar:
```bash
luarocks install luasocket
luarocks install lpeg
```

---

## Ámbitos de uso de Lua

### Videojuegos
Lua es el rey indiscutido del scripting en videojuegos. Motores y juegos que lo usan:
- **Roblox**: Millones de juegos programados en Luau (dialecto de Lua)
- **World of Warcraft**: UI y addons en Lua
- **LÖVE2D**: Framework para juegos 2D totalmente en Lua
- **Baldur's Gate 3, Factorio, Don't Starve, Civilization**: Motores internos con Lua
- **Unity/Unreal**: Plugins y bindings disponibles (MoonSharp, UnLua)

### Sistemas embebidos y firmware
- **NodeMCU**: firmware para ESP8266/ESP32 en Lua
- **Redis**: Lua scripting para operaciones atómicas en el servidor
- **Nginx/OpenResty**: Lua para lógica web en el servidor HTTP

### Herramientas y editores
- **Neovim**: Configuración y plugins en Lua (gran migración desde Vimscript)
- **Wireshark**: Plugins de disección de protocolos
- **Adobe Lightroom**: Plugins en Lua
- **MPV**: Reproductor de video, scripting de UI en Lua

### Programación de sistemas
- **LuaJIT**: Uno de los compiladores JIT más rápidos, con FFI para llamar directamente a C
- **AwesomeWM**: Window manager para Linux configurado completamente en Lua

---

## Conceptos clave del ecosistema Lua moderno

### LuaJIT
Compilador JIT compatible con Lua 5.1, con extensiones propias. Su FFI (Foreign Function Interface) permite llamar funciones y estructuras de C directamente, sin bindings complejos:

```lua
local ffi = require("ffi")
ffi.cdef[[
  int printf(const char *fmt, ...);
]]
ffi.C.printf("Hola %s\n", "desde C")
```

### Luau
Dialecto de Lua creado por Roblox con:
- Tipado gradual (`type` opcional)
- Análisis estático integrado
- Sintaxis de tipos anotados: `function sum(a: number, b: number): number`

### Teal
Lenguaje que compila a Lua añadiendo tipos estáticos al estilo TypeScript. `módulo.tl` → `módulo.lua`.

---

## Resumen

- Lua es minimalista por diseño: un puñado de tipos y las tablas como navaja suiza.
- Las funciones son ciudadanas de primera clase y los closures permiten encapsulación sin clases.
- Las metatables dan control total sobre el comportamiento de tablas, incluyendo OOP.
- Se usa en videojuegos, sistemas embebidos, editores y como lenguaje de scripting/glue.
- LuaJIT es un superpoder: rendimiento cercano a C con la flexibilidad de un lenguaje dinámico.
