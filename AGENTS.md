# AGENTS.md

Libraries and example agents for calling the LLM canister on the Internet Computer, with parallel Rust (`ic-llm`) and Motoko (`mo:llm`) implementations.

## Toolchain

Tool versions are pinned in `mise.toml` and installed via [mise](https://mise.jdx.dev). Run `mise install` before building. CI uses these exact versions:

- Rust 1.94.0 with `rustfmt`, `clippy`, and the `wasm32-unknown-unknown` target
- Node 22, pnpm 10.9.0
- `ic-mops` (Motoko package manager) 2.19.2

## Build, test, lint, format

Rust library (run inside `rust/`):

- Test: `cargo test`
- Lint: `cargo clippy --all-targets --all-features -- -D warnings` (warnings fail CI)
- Format check: `cargo fmt -- --check`

Motoko library (run inside `motoko/`):

- Test: `mops test`

There is no repo-wide build/test command; each library and example is built from its own directory.

## Layout

- `rust/` — the `ic-llm` Rust library.
- `motoko/` — the `mo:llm` Motoko library; its dependencies are declared in `mops.toml`.
- `examples/quickstart-agent-rust/` and `examples/quickstart-agent-motoko/` — standalone example agents (`dfx`-based projects), not part of the published libraries.

## Conventions and gotchas

- PR titles must follow Conventional Commits and are enforced in CI. Allowed types: `feat`, `fix`, `chore`, `build`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`, with an optional `(scope)` and optional `!` for breaking changes, e.g. `fix(motoko): ...`.
- Do not hand-edit generated/ignored artifacts: `rust/target/` and `rust/Cargo.lock`'s build output, Motoko's `.mops/` and `mops.lock`, and per-project `.dfx/`, `.icp/cache`, `node_modules`, and `dist`.
- The example agents depend on a pulled canister with id `w36hm-eqaaa-aaaal-qr76a-cai` (see each example's `dfx.json`).
