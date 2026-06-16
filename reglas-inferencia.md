---
marp: true
theme: default
paginate: true
---

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Fira+Code:wght@400;500&display=swap');

:root {
  --color-background: #ffffff;
  --color-foreground: #0f172a;
  --color-heading: #1e3a8a;
  --color-accent: #2563eb;
  --color-accent-light: #dbeafe;
  --color-code-bg: #f1f5f9;
  --color-border: #cbd5e1;
  --color-hr: #94a3b8;
  --font-default: 'Inter', 'Segoe UI', 'Arial', sans-serif;
  --font-code: 'Fira Code', 'Consolas', 'Monaco', monospace;
}

section {
  background-color: var(--color-background);
  color: var(--color-foreground);
  font-family: var(--font-default);
  font-weight: 400;
  box-sizing: border-box;
  border-bottom: 8px solid var(--color-accent);
  position: relative;
  line-height: 1.7;
  font-size: 22px;
  padding: 56px;
}

section::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  height: 4px;
  background: linear-gradient(90deg, #1e3a8a, #3b82f6, #60a5fa);
}

h1, h2, h3, h4, h5, h6 {
  font-weight: 700;
  color: var(--color-heading);
  margin: 0;
  padding: 0;
}

h1 {
  font-size: 56px;
  line-height: 1.4;
  text-align: left;
  color: #0f172a;
}

h2 {
  position: absolute;
  top: 40px;
  left: 56px;
  right: 56px;
  font-size: 38px;
  padding-top: 0;
  padding-bottom: 16px;
}

h2::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: 6px;
  width: 64px;
  height: 3px;
  background: var(--color-accent);
  border-radius: 2px;
}

h2 + * {
  margin-top: 112px;
}

h3 {
  font-size: 26px;
  margin-top: 28px;
  margin-bottom: 10px;
}

ul, ol {
  padding-left: 32px;
}

li {
  margin-bottom: 10px;
}

li::marker {
  color: var(--color-accent);
  font-weight: 700;
}

footer {
  font-size: 14px;
  color: #64748b;
  position: absolute;
  left: 56px;
  right: 56px;
  bottom: 24px;
  text-align: right;
  font-family: var(--font-code);
}

section.lead {
  background: linear-gradient(135deg, #ffffff 0%, #f0f4ff 50%, #dbeafe 100%);
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-bottom: 8px solid var(--color-accent);
}

section.lead h1 {
  margin-bottom: 16px;
  font-size: 60px;
  color: #1e3a8a;
}

section.lead p {
  font-size: 24px;
  color: #475569;
  font-weight: 500;
}

section.lead::before {
  display: none;
}

pre {
  background-color: var(--color-code-bg);
  border: 1px solid var(--color-border);
  border-radius: 8px;
  padding: 16px;
  overflow-x: auto;
  font-family: var(--font-code);
  font-size: 15px;
  line-height: 1.6;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.06);
}

code {
  background-color: var(--color-accent-light);
  color: #1e40af;
  padding: 2px 6px;
  border-radius: 4px;
  font-family: var(--font-code);
  font-size: 0.85em;
  font-weight: 500;
}

pre code {
  background-color: transparent;
  padding: 0;
  color: var(--color-foreground);
  font-weight: 400;
}

strong {
  color: #1e40af;
  font-weight: 700;
}

blockquote {
  border-left: 4px solid var(--color-accent);
  margin: 16px 0;
  padding: 8px 20px;
  background: var(--color-accent-light);
  border-radius: 0 6px 6px 0;
  color: #334155;
  font-style: italic;
}

.katex {
  font-size: 1.05em;
}
</style>

<!-- _class: lead -->
<!-- _paginate: false -->

# Reglas de Inferencia

Nicolás Cagliero · Mauricio Llugdar · Gregorio Vilardo

FaMAF — Universidad Nacional de Córdoba

---

## Introducción

Las **reglas de inferencia** especifican qué **juicios de tipo** son válidos

- Definen la semántica de tipado de un lenguaje
- Cada regla tiene premisas (arriba) y una conclusión (abajo)
- Se combinan para construir derivaciones


---

## Sistema de Tipos

Gramática base para este trabajo:

$$
\begin{aligned}
\langle type \rangle ::= \; & \mathbf{int} \mid \mathbf{bool} \mid \langle type \rangle \to \langle type \rangle \\
\mid \; & \mathbf{prod}(\langle type \rangle, \dots, \langle type \rangle) \\
\mid \; & \mathbf{sum}(\langle type \rangle, \dots, \langle type \rangle) \\
\mid \; & \mathbf{list} \; \langle type \rangle \mid \mathbf{cont} \; \langle type \rangle
\end{aligned}
$$

Abreviaciones:
$$\theta_0 \times \theta_1 \equiv \mathbf{prod}(\theta_0, \theta_1)$$
$$\mathbf{unit} \equiv \mathbf{prod}()$$
$$\theta_0 + \theta_1 \equiv \mathbf{sum}(\theta_0, \theta_1)$$

---

## Juicios de Tipo

Un **juicio de tipo** tiene la forma:

$$
\pi \vdash e : \theta
$$

- $\pi$: contexto de tipado (asigna tipos a patrones/variables)
- $e$: expresión
- $\theta$: tipo

_Ejemplos con contexto:_

$$
\begin{aligned}
& m : \mathbf{int},\; n : \mathbf{int} \vdash m + n : \mathbf{int} \\
& f : \mathbf{int} \to \mathbf{int},\; x : \mathbf{int} \vdash f(f\,x) : \mathbf{int}
\end{aligned}
$$

---

## Constantes y Operaciones (15.1)

Constantes primitivas — axiomas (sin premisas):

$$
\frac{}{\pi \vdash \mathbf{true} : \mathbf{bool}} \qquad
\frac{}{\pi \vdash 0 : \mathbf{int}} \quad \text{etc.}
$$

Operaciones primitivas:

$$
\frac{\pi \vdash e_0 : \mathbf{int} \quad \pi \vdash e_1 : \mathbf{int}}{\pi \vdash e_0 + e_1 : \mathbf{int}} \qquad
\frac{\pi \vdash e_0 : \mathbf{int} \quad \pi \vdash e_1 : \mathbf{int}}{\pi \vdash e_0 = e_1 : \mathbf{bool}}
$$

$$
\frac{\pi \vdash e_0 : \mathbf{bool} \quad \pi \vdash e_1 : \mathbf{bool}}{\pi \vdash e_0 \land e_1 : \mathbf{bool}}
$$

---

## Condicional (15.2)

La rama `if` une dos caminos en un mismo tipo:

$$
\frac{\pi \vdash e_0 : \mathbf{bool} \quad \pi \vdash e_1 : \theta \quad \pi \vdash e_2 : \theta}{\pi \vdash \mathbf{if}\; e_0\; \mathbf{then}\; e_1\; \mathbf{else}\; e_2 : \theta}
$$

En Rust: `if` es una expresión — ambas ramas deben tener el mismo tipo

```rust
let resultado = if x > 0 { x } else { -x };
// resultado: i32
```

---

## Variables (15.3)

Axioma: una variable tiene el tipo que el contexto le asigna

$$
\pi \vdash v : \theta \quad \text{cuando } \pi \text{ contiene } v : \theta
$$

Es una regla **sin premisas** (un axioma), al igual que las constantes (`true`, `0`) o `error` — actúa como hoja o punto de partida en el árbol de derivación.

> En Rust: No representa la declaración (`let`), sino el **uso**. Cuando el compilador encuentra `x + 5`, aplica esta regla buscando `x` en su contexto actual para resolver su tipo.

---

## _→_ Introducción (15.4)

Crear una función:

$$
\frac{\pi,\; p : \theta \vdash e : \theta'}{\pi \vdash \lambda p.\,e : \theta \to \theta'}
$$

---

## _→_ Eliminación (15.5)

Aplicar una función:

$$
\frac{\pi \vdash e_0 : \theta \to \theta' \quad \pi \vdash e_1 : \theta}{\pi \vdash e_0 \; e_1 : \theta'}
$$

---

## Producto (15.6 — 15.7)

**Introducción** — crear una tupla:

$$
\frac{\pi \vdash e_0 : \theta_0 \quad \cdots \quad \pi \vdash e_{n-1} : \theta_{n-1}}{\pi \vdash \langle e_0,\dots,e_{n-1} \rangle : \mathbf{prod}(\theta_0,\dots,\theta_{n-1})}
$$

**Eliminación** — proyectar un campo:

$$
\frac{\pi \vdash e : \mathbf{prod}(\theta_0,\dots,\theta_{n-1})}{\pi \vdash e.k : \theta_k} \quad (k < n)
$$

---

## Producto en Rust

```rust
let x: i32 = 33;
let y: bool = true;
let tupla = (x, y);        // prod(i32, bool)
let otra_tupla = (3, 2, true);

// Eliminación: acceso con .k
println!("{}", otra_tupla.1); // i32
```

**`unit`** = `prod()`. En Rust: `()` — "ausencia de información útil"

```rust
fn funcion_void() -> () {
    // retorna el producto vacío ()
}
```

---

## Suma (15.8 — 15.9)

**Introducción** — construir un valor alternativo:

$$
\frac{\pi \vdash e : \theta_k}{\pi \vdash @k\; e : \mathbf{sum}(\theta_0,\dots,\theta_{n-1})} \quad (k < n)
$$

**Eliminación** — _pattern matching_:

$$
\frac{
\begin{array}{c}
\pi \vdash e : \mathbf{sum}(\theta_0,\dots,\theta_{n-1}) \\
\pi \vdash e_0 : \theta_0 \to \theta \quad \cdots \quad \pi \vdash e_{n-1} : \theta_{n-1} \to \theta
\end{array}
}{
\pi \vdash \mathbf{sumcase}\; e\;\mathbf{of}\; (e_0,\dots,e_{n-1}) : \theta
}
$$

---

## Suma en Rust

`sum(i32, bool)` equivale a un `enum`:

```rust
enum Respuesta {
    Numerica(i32), // @0
    Valor(bool),   // @1
}

let e: Respuesta = Respuesta::Numerica(33); // @0 33 

let res = match e { // sumcase e of (f_0: u32, f_1: u32) => res: u32
    Respuesta::Numerica(v) => { f_0() },
    Respuesta::Valor(v)    => { f_1() },
} 
```

El `match` de Rust es exactamente la **eliminación de la suma** con clausura exhaustiva

**`sum()`** — suma vacía → `enum SumaVacia {}` → tipo **`!`** (never)

> **`unit`** ≠ **`empty`**: `unit` expresa _ausencia de información_; `empty` representa _imposibilidad_

---

## Patrones (15.10)

El patrón de producto descompone una tupla en el contexto:

$$
\frac{\pi, p_0{:}\theta_0,\dots,p_{n-1}{:}\theta_{n-1},\pi' \vdash e : \theta}{\pi, \langle p_0,\dots,p_{n-1} \rangle {:} \mathbf{prod}(\theta_0,\dots,\theta_{n-1}),\pi' \vdash e : \theta}
$$

En Rust: _destructuring_ en `let` o `match`

```rust
let punto: (i32, i32) = (3, 7);
let (x, y) = punto; // x: i32, y: i32
let res = x + y; //res: i32
```

---

## let (15.11)

Introducción de variables locales:

$$
\frac{
\begin{matrix}
\pi \vdash e_0 : \theta_0 \\
\vdots \\
\pi \vdash e_{n-1} : \theta_{n-1} \\
\pi, p_0{:}\theta_0,\dots,p_{n-1}{:}\theta_{n-1} \vdash e : \theta
\end{matrix}
}{
\pi \vdash \mathbf{let}\; p_0 \equiv e_0,\dots,p_{n-1} \equiv e_{n-1}\; \mathbf{in}\; e : \theta
}
$$

En Rust: `let` vincula nombres a expresiones en el bloque

```rust
let p_0 = 32;
let p_1 = 31;
let e = p_0 + p_1;
// en el contexto: p_0: i32, p_1: i32, e: i32
```

---

## letrec (15.12)

Definiciones recursivas:

$$
\frac{
\begin{matrix}
\pi, p_0{:}\theta_0,\dots,p_{n-1}{:}\theta_{n-1} \vdash e_0 : \theta_0 \\
\vdots \\
\pi, p_0{:}\theta_0,\dots,p_{n-1}{:}\theta_{n-1} \vdash e_{n-1} : \theta_{n-1} \\
\pi, p_0{:}\theta_0,\dots,p_{n-1}{:}\theta_{n-1} \vdash e : \theta
\end{matrix}
}{
\pi \vdash \mathbf{letrec}\; p_0 \equiv e_0,\dots,p_{n-1} \equiv e_{n-1}\; \mathbf{in}\; e : \theta
}
$$

En Rust: funciones recursivas con `fn`

```rust
fn fact(n: u32) -> u32 {
    if n == 0 { 1 } else { n * fact(n - 1) }
}
// fact puede referenciarse a sí misma en el cuerpo
// letrec fact ≡ λn. if n = 0 then 1 else n x fact(n — 1) 
// fact:int -> int 
```

---

## Punto Fijo (15.13)

Combinador de punto fijo — base teórica de la recursión:

$$
\frac{\pi \vdash e : \theta \to \theta}{\pi \vdash \mathbf{rec}\; e : \theta}
$$

- Toda función $e : \theta \to \theta$ tiene un punto fijo (en semántica denotacional)
- Permite definir recursión _sin nombres_

---

## Listas (15.14 — 15.15)

**Introducción** — nil y cons:

$$
\frac{}{\pi \vdash \mathbf{nil} : \mathbf{list}\; \theta} \qquad
\frac{\pi \vdash e_0 : \theta \quad \pi \vdash e_1 : \mathbf{list}\; \theta}{\pi \vdash e_0 :: e_1 : \mathbf{list}\; \theta}
$$

**Eliminación** — listcase:

$$
\frac{
\pi \vdash e_0 : \mathbf{list}\; \theta \quad
\pi \vdash e_1 : \theta' \quad
\pi \vdash e_2 : \theta \to \mathbf{list}\; \theta \to \theta'
}{
\pi \vdash \mathbf{listcase}\; e_0\; \mathbf{of}\; (e_1, e_2) : \theta'
}
$$

En Rust: `Vec<T>` es el tipo lista estándar

```rust
let v: Vec<i32> = vec![1, 2, 3];
let primero = v[0];  // acceso por índice
```

---

## Continuaciones (15.16 — 15.17)

**callcc** — capturar la continuación actual:

$$
\frac{\pi \vdash e : \mathbf{cont}\; \theta \to \theta}{\pi \vdash \mathbf{callcc}\; e : \theta}
$$

**throw** — saltar a una continuación:

$$
\frac{\pi \vdash e_0 : \mathbf{cont}\; \theta \quad \pi \vdash e_1 : \theta}{\pi \vdash \mathbf{throw}\; e_0\; e_1 : \theta'}
$$

Rust **no tiene** continuaciones de primera clase — estas reglas pertenecen a lenguajes con control imperativo (Scheme, SML)

---

## Error (15.18)

Axioma: `error` puede tener **cualquier tipo**

$$
\frac{}{\pi \vdash \mathbf{error} : \theta}
$$

En Rust: `panic!()` y `unreachable!()` tipan a cualquier tipo

```rust
fn dividir(a: i32, b: i32) -> i32 {
    if b == 0 { panic!("division por cero") }
    else { a / b }
}
```

Regla de _escape_ — permite abortar la evaluación bajo demanda

---

## RustBelt

Rust tiene un modelo formal que respalda la solidez de su sistema de tipos

**RustBelt** (Jung et al., POPL 2018) provee una interpretación semántica para:

- Tipos producto (tuplas)
- Préstamos (_borrowing_) y _lifetimes_
- Propiedad (_ownership_)

> Securing the Foundations of the Rust Programming Language

---

## Ejemplo: Derivación

Probar: $\vdash \lambda f.\,\lambda x.\,f(f\,x) : (\mathbf{int}\!\to\!\mathbf{int})\!\to\!\mathbf{int}\!\to\!\mathbf{int}$

Estrategia — buscar desde la meta hacia los axiomas:

1. Es un lambda → **intro (15.4)** → $f{:}\mathbf{int}{\to}\mathbf{int} \vdash \lambda x.\,f(f\,x) : \mathbf{int}{\to}\mathbf{int}$
2. Otro lambda → **intro (15.4)** → $f{:}\mathbf{int}{\to}\mathbf{int},\,x{:}\mathbf{int} \vdash f(f\,x) : \mathbf{int}$
3. Es aplicación → **elim (15.5)** → $f{:}\mathbf{int}{\to}\mathbf{int}$, $f\,x : \mathbf{int}$
4. $f\,x$ es aplicación → **elim (15.5)** → $f{:}\mathbf{int}{\to}\mathbf{int}$ ✓, $x{:}\mathbf{int}$ ✓
5. $f$ y $x$ son variables → **regla (15.3)** — axiomas en contexto

---

## Derivación completa

La prueba se escribe de axiomas a meta (las hojas primero):

$$
\begin{array}{r l @{\hspace{2em}} l}
1. & f{:}\mathbf{int}{\to}\mathbf{int},\,x{:}\mathbf{int} \vdash f : \mathbf{int}{\to}\mathbf{int} & (15.3)\\
2. & f{:}\mathbf{int}{\to}\mathbf{int},\,x{:}\mathbf{int} \vdash x : \mathbf{int} & (15.3)\\
3. & f{:}\mathbf{int}{\to}\mathbf{int},\,x{:}\mathbf{int} \vdash f\,x : \mathbf{int} & (15.5,\,1,\,2)\\
4. & f{:}\mathbf{int}{\to}\mathbf{int},\,x{:}\mathbf{int} \vdash f(f\,x) : \mathbf{int} & (15.5,\,1,\,3)\\
5. & f{:}\mathbf{int}{\to}\mathbf{int} \vdash \lambda x.\,f(f\,x) : \mathbf{int}{\to}\mathbf{int} & (15.4,\,4)\\
6. & \vdash \lambda f.\,\lambda x.\,f(f\,x) : (\mathbf{int}{\to}\mathbf{int}){\to}\mathbf{int}{\to}\mathbf{int} & (15.4,\,5)
\end{array}
$$

Pasos 1–5: _cómo encontrar_ la prueba (de meta a axiomas).
Pasos 1–6: _prueba final_ (de axiomas a meta) — misma derivación en orden inverso.

---

## Resumen

- Las **reglas de inferencia** definen formalmente el tipado
- Cubren 18 reglas: constantes, condicional, variables, funciones, producto, suma, patrones, definiciones, punto fijo, listas, continuaciones y error
- **Rust** implementa fielmente varias: tuplas (`prod`), `enum` (`sum`), `!` (`sum()`), `if` (condicional), `let`, destructuring, `Vec`, `panic!`
- Una **derivación** se construye combinando reglas desde axiomas hasta la meta
- RustBelt fundamenta semánticamente el sistema de tipos de Rust

---

## Referencias

**John C. Reynolds.** _Theories of Programming Languages._ Cambridge University Press, 1998.

**RustBelt.** _Securing the Foundations of the Rust Programming Language._ Jung et al., POPL 2018.
