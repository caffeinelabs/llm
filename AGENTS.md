# AGENTS.md

Libraries and example agents for calling the on-chain LLM canister on the Internet Computer, provided for both Rust (`ic-llm`) and Motoko (`mo:llm`).

## Layout

- `rust/` — the `ic-llm` Rust crate (library source under `rust/src`).
- `motoko/` — the `mo:llm` Motoko library (source under `motoko/src`, tests under `motoko/test`).
- `examples/quickstart-agent-rust/` — Rust example agent (canister backend + web frontend).
- `examples/quickstart-agent-motoko/` — Motoko example agent (canister backend + web frontend).

## Toolchain

Tool versions are pinned in `mise.toml`; run `mise install` to match CI. Pinned: rust 1.94.0 (with `rustfmt`, `clippy`, `wasm32-unknown-unknown` target), node 22, pnpm 10.9.0, ic-mops 2.19.2. Motoko compiler `moc` 1.8.2 is pinned in `motoko/mops.toml`. Root dependencies install with `pnpm install`.

## Build, test, lint, format

CI (`.github/workflows/tests.yml`) runs and must pass:

- Motoko tests: `mops test` (run in `motoko/`).
- Rust tests: `cargo test` (run in `rust/`).
- Rust lint: `cargo clippy --all-targets --all-features -- -D warnings` (run in `rust/`); clippy warnings fail the build.
- Rust formatting: `cargo fmt -- --check` (run in `rust/`).

Example frontends expose these package.json scripts (run in each `src/frontend/`): `build`, `start`, `format`, and (motoko example) `typecheck`. The Rust example's `justfile` provides `generate` (regenerate frontend bindings from the backend `.did`) and `start`.

## Conventions and gotchas

- PR titles must follow Conventional Commits and are enforced by `.github/workflows/conventional-commits.yml`. Allowed types: `feat`, `fix`, `chore`, `build`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`; optional `(scope)` and `!` for breaking changes.
- Do not hand-edit generated artifacts: dfx build output (`.dfx/`, `target/`, `node_modules/`), the dfx-generated `.env`, pulled canister deps under `examples/quickstart-agent-motoko/deps/`, and the Rust example's frontend bindings (regenerate via `just generate`).
- The `ic-llm` (Rust) and `llm` (Motoko) libraries are versioned and published independently; bump the version in the respective `Cargo.toml` / `mops.toml` when releasing.
