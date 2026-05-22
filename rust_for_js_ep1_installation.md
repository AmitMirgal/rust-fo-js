# Rust for JS Devs — Episode 01: Getting Rust Installed

> **Series:** Rust for JS developers &nbsp;·&nbsp; **Episode:** 01 of N  
> **Topic:** rustup, cargo, and your first "hello, world"

---

## The JS parallel

In JS you'd manage your Node version with **nvm**, and install packages with **npm** — two separate tools. In Rust, this collapses into one:

| JavaScript / Node | Rust |
|---|---|
| `nvm` — manages Node versions | `rustup` — manages Rust versions |
| `npm` — installs packages | `cargo` — installs packages AND builds AND runs AND tests |
| `npm init` — creates a project | `cargo new` — creates a project |
| `node index.js` — runs your code | `cargo run` — compiles + runs your code |

> [!TIP]
> Think of `rustup` as nvm, and `cargo` as npm + node + npx + jest all rolled into one binary.

---

## Installing Rust

### macOS / Linux

One curl command — same pattern as nvm:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Follow the prompts — press `1` to accept defaults.

### Windows

Download and run the installer from [rustup.rs](https://rustup.rs) — it's a `.exe`, no terminal needed to start.

> [!WARNING]
> You may also need the MSVC build tools (Visual Studio Build Tools). The installer will tell you if so.

### Verify the install

Reload your shell (or open a new terminal tab), then:

```bash
source ~/.cargo/env   # or open a new terminal tab

rustc --version       # rustc 1.x.x (...)
cargo --version       # cargo 1.x.x (...)
```

---

## What just got installed

| Tool | JS equivalent | What it does |
|---|---|---|
| `rustup` | nvm | Manages Rust versions. Lets you switch between `stable` / `beta` / `nightly`. |
| `rustc` | node (the runtime/compiler) | The Rust compiler. You rarely call this directly — cargo does it for you. |
| `cargo` | npm + npx + jest | Build, run, test, add deps, publish. Your daily driver. |

> [!NOTE]
> Unlike JS, there is no "which package manager should I use" debate. Everyone uses `cargo`. It ships with Rust and it's official.

---

## Your first project

In JS you'd do:

```bash
mkdir my-app && cd my-app
npm init -y
node index.js
```

In Rust, the equivalent is:

```bash
cargo new my-app
cd my-app
cargo run
```

`cargo new` creates a ready-to-run project. Inside you'll find:

```
my-app/
├── Cargo.toml    ← like package.json (project metadata + deps)
└── src/
    └── main.rs   ← your entry point (like index.js)
```

And `src/main.rs` already has working code:

```rust
fn main() {
    println!("Hello, world!");
}
```

Run it:

```bash
cargo run
# Compiling my-app v0.1.0
# Finished dev [unoptimized + debuginfo] target(s) in 0.42s
# Running `target/debug/my-app`
# Hello, world!
```

---

## What JS developers find surprising

### 1. There's a compilation step

There's no "just run the file." Rust compiles to a native binary first. `cargo run` handles compile + run in one step, so it feels seamless — but the first build takes a few seconds. Subsequent builds are incremental and fast.

### 2. No `node_modules` in your project

Dependencies download to a global cache at `~/.cargo` — not inside your project folder. No more 300 MB `node_modules` per project. Your actual build output lands in `target/`.

```
my-app/
├── Cargo.toml
├── Cargo.lock    ← like package-lock.json
├── src/
│   └── main.rs
└── target/       ← compiled output lives here (gitignore this)
```

### 3. `Cargo.lock` — commit it or not?

`Cargo.lock` pins exact dependency versions, like `package-lock.json`. The convention differs from JS though:

| Project type | Commit `Cargo.lock`? |
|---|---|
| Binary / application | ✅ Yes — ensures reproducible builds |
| Library (published to crates.io) | ❌ No — let consumers resolve versions |

Cargo handles this automatically based on your project type.

---

## Quick reference card

```bash
# Install / update Rust
rustup install stable
rustup update

# Create a new project
cargo new my-app        # binary (has a main.rs)
cargo new my-lib --lib  # library (has a lib.rs)

# Build and run
cargo run               # compile + run (debug mode)
cargo run --release     # compile + run (optimized)
cargo build             # compile only

# Dependencies
cargo add serde         # like: npm install serde
cargo add serde --dev   # like: npm install --save-dev serde

# Other
cargo test              # run tests
cargo fmt               # format code (like prettier)
cargo clippy            # lint (like eslint)
```

---

## Episode summary

- **rustup** = nvm. One install, manages all your Rust versions.
- **cargo** = npm + node + npx + jest. One tool for everything.
- `cargo new` scaffolds a project. `cargo run` builds and executes it.
- No `node_modules` — deps live in `~/.cargo` globally.
- Rust compiles before running. `cargo run` makes this transparent.

---

*Next episode: Variables, types, and mutability — `let`, `const`, and why Rust's type system is stricter than TypeScript.*
