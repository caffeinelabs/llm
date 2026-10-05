# AGENTS.md

Libraries (`ic-llm` for Rust, `mo:llm` for Motoko) and example agents for calling the LLM canister on the Internet Computer.

## Toolchain

Tool versions are pinned in `mise.toml` and installed with [mise](https://mise.jdx.dev). Run `mise install` before working. This provides Rust 1.94.0 (with `rustfmt`, `clippy`, and the `wasm32-unknown-unknown` target), pnpm 10.9.0, Node 22, and `ic-mops` 2.19.2. CI installs the same versions via `jdx/mise-action`, so use these rather than system-wide installs.

## Build, test, lint, format

Rust (run from `rust/`):

- Test: `cargo test`
- Lint: `cargo clippy --all-targets --all-features -- -D warnings` (clippy warnings fail CI)
- Format check: `cargo fmt -- --check` (run `cargo fmt` to fix)

Motoko (run from `motoko/`):

- Test: `mops test`

CI (`.github/workflows/tests.yml`) runs exactly these three Rust steps and the Motoko test step; all must pass.

## Layout

- `rust/` — the `ic-llm` library crate.
- `motoko/` — the `mo:llm` library (mops package); tests live in `motoko/test/`.
- `examples/quickstart-agent-rust/` and `examples/quickstart-agent-motoko/` — standalone example agents, each with its own build config and README. They are deployed with `dfx` and have a `src/frontend/` built via `pnpm build`.

## Conventions

- PR titles must follow Conventional Commits (`.github/workflows/conventional-commits.yml`). Allowed types: `feat`, `fix`, `chore`, `build`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`, with optional `(scope)` and `!`. Example: `fix(rust): handle empty prompt`.
- Do not commit generated or build output: `target`, `node_modules`, `.dfx`, and `.icp/cache` are git-ignored.
- In `examples/quickstart-agent-rust/`, Candid bindings under `src/frontend/src/bindings` are generated via the `justfile` `generate` recipe (`icp-bindgen`); regenerate them rather than editing by hand.
- The library crate version and the mops package version are independent (`rust/Cargo.toml` vs `motoko/mops.toml`); bump the one you change.
