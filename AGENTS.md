# AGENTS.md

Libraries and example agents for using the LLM canister on the Internet Computer, with a Rust library (`ic-llm`) and a Motoko library (`mo:llm`).

## Toolchain

Tool versions are pinned in `mise.toml` and installed by [mise](https://mise.jdx.dev/). Run `mise install` to get the exact versions used by CI (Rust 1.94.0 with `rustfmt`, `clippy`, and the `wasm32-unknown-unknown` target; pnpm 10.9.0; Node 22; `ic-mops` 2.19.2). CI uses these same pinned versions, so prefer them over system toolchains.

`mise.toml` adds `./node_modules/.bin` to `PATH`. Run `pnpm install` at the repo root to make those binaries available.

## Build, test, lint, format

Rust library (run inside `rust/`):

- Test: `cargo test`
- Lint: `cargo clippy --all-targets --all-features -- -D warnings` (warnings fail CI)
- Format check: `cargo fmt -- --check` (format with `cargo fmt`)

Motoko library (run inside `motoko/`):

- Test: `mops test`

These are the exact commands run by `.github/workflows/tests.yml`. No repo-wide build, lint, or format command is defined at the root.

## Directory map

- `motoko/` — the `mo:llm` Motoko library (`src/`, `test/`, `mops.toml`).
- `rust/` — the `ic-llm` Rust library (`src/`, `Cargo.toml`).
- `examples/` — standalone quickstart agents (`quickstart-agent-motoko`, `quickstart-agent-rust`), each with its own build config, not part of the library builds or CI test matrix.

## Conventions and gotchas

- PR titles must follow Conventional Commits and are checked by `.github/workflows/conventional-commits.yml`. The title must match `verb(scope)!: description`, where verb is one of `feat`, `fix`, `chore`, `build`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`; `(scope)` and `!` are optional.
- Do not edit generated or ignored paths (see `.gitignore`): `target`, `node_modules`, `.dfx`, `.icp/cache`.
- The Rust and Motoko libraries are versioned and published independently; versions live in `rust/Cargo.toml` and `motoko/mops.toml`.
