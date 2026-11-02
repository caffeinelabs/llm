# AGENTS.md

Libraries and example agents for using the on-chain LLM canister on the Internet Computer, in Rust (`ic-llm`) and Motoko (`mo:llm`).

## Layout

- `rust/` — the `ic-llm` Rust library (`src/` contains `chat.rs`, `lib.rs`, `tool.rs`).
- `motoko/` — the `mo:llm` Motoko library (`src/` plus `test/`).
- `examples/quickstart-agent-rust/`, `examples/quickstart-agent-motoko/` — standalone example agents, each with its own backend + frontend and build config.

## Toolchain

Tool versions are pinned in `mise.toml`; install them with [mise](https://mise.jdx.dev) by running `mise install`. This provides Rust 1.94.0 (with `rustfmt`, `clippy`, and the `wasm32-unknown-unknown` target), Node 22, pnpm 10.9.0, and `ic-mops` 2.19.2. The Motoko toolchain uses `moc` 1.8.2 (see `motoko/mops.toml`).

## Build, test, lint, format

Run these from the directory indicated (matching CI in `.github/workflows/tests.yml`):

- Motoko tests — from `motoko/`: `mops test`
- Rust tests — from `rust/`: `cargo test`
- Rust lint — from `rust/`: `cargo clippy --all-targets --all-features -- -D warnings` (CI treats warnings as errors)
- Rust format check — from `rust/`: `cargo fmt -- --check`

JavaScript dependencies are managed with pnpm (`pnpm-lock.yaml`); example frontends build with `pnpm build` and start with `pnpm start`.

## Conventions and gotchas

- PR titles must follow Conventional Commits and are enforced in CI (`.github/workflows/conventional-commits.yml`): `type(scope)!: description` where type is one of `feat`, `fix`, `chore`, `build`, `ci`, `docs`, `style`, `refactor`, `perf`, `test`.
- Build output and caches are git-ignored and must not be committed or hand-edited: `target`, `node_modules`, `.dfx`, `.icp/cache`.
- In `examples/quickstart-agent-rust/`, frontend Candid bindings are generated via the `justfile` `generate` target (`icp-bindgen`); regenerate them rather than editing by hand.
- The `motoko` and `rust` libraries and the two examples are independent packages — run their build/test commands from their own directories.
