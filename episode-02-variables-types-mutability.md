# Episode 02 — Variables, Types & Mutability

> **Series:** Rust for JS Devs &nbsp;|&nbsp; **Prerequisite:** Episode 01 — Installing Rust

*Why `let` isn't `let`, why `const` isn't `const`, and why Rust's type system makes TypeScript look lenient.*

---

## 01 · The `let` trap — same keyword, completely different rules

Rust uses `let` to declare variables — same word as JavaScript. But the similarity ends at the spelling.

In JS, `let` declares a **mutable, reassignable** variable. In Rust, `let` declares an **immutable binding by default**. It behaves more like JS's `const`.

**JavaScript:**
```js
let score = 0;
score = 42;   // ✅ totally fine
score += 1;   // ✅ also fine
```

**Rust:**
```rust
let score = 0;
score = 42;  // ❌ compile error!
// error: cannot assign twice to immutable variable `score`
```

To make a variable reassignable in Rust, you add the `mut` keyword — explicit opt-in to mutability:

```rust
let mut count = 0;
count = count + 1;  // ✅ now fine
```

> [!WARNING]
> Rust's `let` feels like JS's `const`. If you try to reassign it, the compiler refuses **at compile time** — before your code ever runs. This is not a runtime error; it's a wall.

> [!TIP]
> **Why does Rust do this?** Immutability by default eliminates a massive class of bugs — accidental state changes, race conditions in concurrent code, and unexpected side effects. If something changes that you didn't intend to change, Rust tells you immediately.

---

## 02 · Rust's `const` — stricter than you think

Both JS and Rust have `const`. But Rust's version is far more strict.

In JavaScript, `const` only prevents **reassignment** — the value inside (like an object) can still be mutated. In Rust, `const` means the value is **baked into the binary at compile time**. It's a true constant.

**JavaScript `const` (partial protection):**
```js
const config = { debug: false };

config = {};            // ❌ TypeError — can't reassign
config.debug = true;    // ✅ mutation still works!?

const MAX = 100;        // computed at runtime
```

**Rust `const` (real constant):**
```rust
// Must be UPPERCASE by convention
// Must have an explicit type annotation
// Must be a compile-time value
const MAX_SCORE: u32 = 100;

// No mut allowed on const — ever
// Inlined at every use site, zero runtime cost
```

> [!NOTE]
> Rust `const` requires an explicit type annotation — always. You cannot write `const X = 5`; it must be `const X: u32 = 5`. And by convention, constants are `SCREAMING_SNAKE_CASE`.

---

## 03 · Shadowing — Rust's surprising superpower

Here's something Rust does that has no real JS equivalent: **shadowing**. You can re-declare a variable with `let` in the same scope. The new binding *shadows* the old one — and it can even be a completely different type.

```rust
let x = 5;          // x is i32
let x = x * 2;      // x is now 10 (still i32)
let x = "hello";    // x is now &str — a different type entirely!

// This is NOT the same as mut.
// Each `let x` creates a NEW binding.
// The previous one is gone from scope.
```

A practical example — transforming a value without inventing a new name:

```rust
let spaces = "   ";             // &str (a string slice)
let spaces = spaces.len();      // usize (an integer — type changed!)
```

> [!TIP]
> **When to use shadowing vs `mut`:** Use `mut` when you're updating a value over time (a counter, accumulator). Use shadowing when you're *transforming* a value — parsing a string to a number, trimming whitespace — and want to reuse a meaningful name without inventing `spaces_trimmed`, `spaces_len`, etc.

---

## 04 · The type system — stricter than TypeScript

TypeScript adds types on top of JavaScript. Rust's type system is built into the language from the ground up. The difference shows:

- TypeScript types are **erased at runtime**
- Rust types are enforced **at compile time** *and* exist at runtime, affecting memory layout, performance, and correctness

**Type inference** — Rust can infer most types. You don't have to write them. But unlike TypeScript, Rust's inference is often smarter and stricter. It looks at how you *use* a variable downstream to determine its type.

```rust
let x = 42;       // inferred: i32
let y = 3.14;     // inferred: f64
let s = "hello";  // inferred: &str
let b = true;     // inferred: bool

// Or be explicit — same result:
let x: i32 = 42;
let y: f64 = 3.14;
```

### Numeric types — where JS developers get surprised

JavaScript has one number type (`number`, always a 64-bit float). TypeScript adds annotations but doesn't change runtime behavior. Rust has many distinct integer and float types — and **they do not silently convert**.

| Rust Type | Bits | Signed | JS Equivalent |
|-----------|------|--------|---------------|
| `i8`      | 8    | ✅     | — |
| `i32`     | 32   | ✅     | closest to JS integer math |
| `i64`     | 64   | ✅     | like `BigInt` |
| `u32`     | 32   | ❌     | — |
| `usize`   | arch | ❌     | used for array indexes |
| `f32`     | 32   | ✅     | less precise float |
| `f64`     | 64   | ✅     | **≈ JS `number`** |

> [!WARNING]
> **No implicit coercion — ever.** In JS, `5 + 3.14` silently works. In Rust, adding an `i32` to an `f64` is a compile error. You must cast explicitly: `5 as f64 + 3.14`. Rust never silently converts between numeric types.

```rust
let a: i32 = 10;
let b: f64 = 3.14;

// This won't compile:
// let c = a + b;  ❌ mismatched types

// You must cast explicitly:
let c = a as f64 + b;  // ✅ c = 13.14

// Same story with array indexes:
let index: i32 = 2;
let arr = [10, 20, 30];
// arr[index]           ❌ — indexes must be usize
let v = arr[index as usize];  // ✅
```

---

## 05 · Rust vs TypeScript — side by side

| Concept | JavaScript | TypeScript | Rust |
|---------|-----------|------------|------|
| Mutable variable | `let x = 5` | `let x: number = 5` | `let mut x: i32 = 5` |
| Immutable variable | `const x = 5` | `const x: number = 5` | `let x = 5` |
| True constant | — | `readonly` (partial) | `const X: u32 = 5` |
| Type annotation | None | Optional (inferred) | Optional (inferred), required on `const` & fn params |
| Type enforcement | Runtime (if at all) | Compile time (erased after) | Compile time + runtime layout |
| Implicit coercion | Yes (notorious) | Some (via `any`) | Never |
| Null / undefined | `null`, `undefined` | `null`, `undefined` | No null — uses `Option<T>` |

> [!NOTE]
> Rust has no `null` or `undefined`. The absence of a value is expressed via `Option<T>`. This is a big topic — we'll cover it in a dedicated episode. For now: if you're thinking "what replaces `null`?", the answer is *the type system itself*.

---

## 06 · Quick reference card

```rust
// Immutable variable (default)
let x = 5;

// Mutable variable
let mut x = 5;

// With explicit type
let x: i32 = 5;

// Compile-time constant (requires type + SCREAMING_SNAKE_CASE)
const MAX_SCORE: u32 = 100;

// Shadowing — re-declare in same scope, can change type
let x = "hello";
let x = x.len();   // now usize

// Explicit type cast (never implicit in Rust)
let y = x as f64;
```

**Default types when inferred:**
- Integer → `i32`
- Float → `f64`
- Text → `&str`
- Boolean → `bool`

---

## Summary

- ✅ Rust's `let` is **immutable by default** — the opposite of JS's `let`. Add `mut` to opt into mutability.
- ✅ Rust's `const` is a **true compile-time constant** with a required type annotation — stricter than JS `const`.
- ✅ **Shadowing** lets you re-declare a variable with `let` in the same scope, even changing its type. Great for transformations.
- ✅ Rust has many numeric types (`i32`, `u32`, `f64`…) that **never coerce silently** — cast explicitly with `as`.
- ✅ Types are inferred in most cases, but Rust's system is enforced at compile time and runtime — deeper than TypeScript.

---

## Up next → Episode 03

**Functions, return values & expressions**
Why Rust functions don't always need `return`, and why *everything* is an expression.
