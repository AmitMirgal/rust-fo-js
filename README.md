# 🦀 Rust for JS Devs

> Learn Rust the way you think — through JavaScript.

If you already know JavaScript and want to learn Rust, most guides throw you into systems programming concepts cold. This series doesn't. Every concept is introduced by comparing it to something you already know from JS — then explaining what's different, why Rust works that way, and what will surprise you.

No C. No C++. No assumed knowledge of memory management. Just JS → Rust, concept by concept.

---

## Who this is for

- You write JavaScript (or TypeScript) day-to-day
- You're curious about Rust but don't know where to start
- You've tried Rust before and bounced off the borrow checker
- You want to understand *why* Rust works the way it does, not just *how*

---

## How this series works

Each episode covers one concept. It always follows the same structure:

1. **The JS equivalent** — what you'd write in JavaScript
2. **The Rust version** — the direct translation or closest parallel
3. **The key differences** — why Rust does it differently and what to watch out for

If a concept has no JS equivalent (ownership, lifetimes, the borrow checker), that's called out explicitly and explained from scratch.

---

## Episodes

| # | Topic | Concepts covered |
|---|---|---|
| [01](./rust_for_js_ep1_installation.md) | **Getting Rust installed** | `rustup`, `cargo`, project structure, `Cargo.toml` |
| [02](./episode-02-variables-types-mutability.md) | **Variables & types** | `let`, `const`, mutability, type inference *(coming soon)* |
| 03 | **Functions** | `fn`, return values, expressions vs statements *(coming soon)* |
| 04 | **Ownership** | The concept JS doesn't have — explained from scratch *(coming soon)* |
| 05 | **Borrowing & references** | `&`, `&mut`, the borrow checker *(coming soon)* |
| 06 | **Structs** | Like JS objects, but typed and without a prototype chain *(coming soon)* |
| 07 | **Enums & pattern matching** | Better than `switch`, nothing like it in JS *(coming soon)* |
| 08 | **Error handling** | `Result`, `Option` — no more `try/catch` *(coming soon)* |
| 09 | **Closures & iterators** | `.map()`, `.filter()` — familiar but with new rules *(coming soon)* |
| 10 | **Traits** | Like interfaces, but more powerful *(coming soon)* |

---

## Quick start

If you want to follow along, Episode 01 covers installation. Once you have Rust installed, every other episode only needs a text editor and a terminal.

→ **[Start with Episode 01: Getting Rust Installed](./rust_for_js_ep1_installation.md)**

---

## A note on difficulty

Rust has a reputation for being hard. Some of that is deserved — the borrow checker is genuinely a new way of thinking. But a lot of the difficulty comes from learning Rust through the lens of C/C++, which doesn't apply to JS developers at all.

Your mental models from JS are actually *good* starting points for most of Rust. This series leans into that.

---

*Episodes release one concept at a time. Star the repo to follow along.*
