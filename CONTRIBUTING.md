# Contributing to Prism

Thank you for helping build better Stellar tooling!

## How to Contribute

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/your-feature`
3. Commit your changes: `git commit -m "feat: your feature"`
4. Push and open a Pull Request

## Areas to Contribute

- New explorer views (contract state, event logs, storage)
- Indexer improvements in `indexer/src/`
- UI components and design
- RPC integration and data parsing
- Soroban smart contracts in `contracts/`
- Tests and documentation

## Project Structure

| Directory     | Description                          |
| ------------- | ------------------------------------ |
| `frontend/`   | Next.js explorer UI                  |
| `indexer/`    | Event indexer (TypeScript + Prisma)  |
| `contracts/`  | Soroban smart contracts (Rust)       |

## Commands

### Frontend (`frontend/`)

```bash
cd frontend
npm install
npm run dev        # Start dev server
npm run lint       # Run linter
npm run test       # Run tests
npm run build      # Production build
```

### Indexer (`indexer/`)

```bash
cd indexer
npm install
npm run dev             # Start indexer in watch mode
npm run test            # Run tests
npm run prisma:generate # Generate Prisma client
npm run prisma:migrate  # Run database migrations
npm run build           # Compile TypeScript
npm run start           # Run compiled output
```

### Contracts (`contracts/`)

```bash
cd contracts
cargo build         # Build contracts
cargo test          # Run contract tests
```

## Rust / Soroban Contract Contributions

The contracts in `contracts/` are written in Rust and target the Soroban
smart contract platform. If you are contributing a contract, follow the
workflow below.

### Prerequisites

- Rust toolchain (stable) with the `wasm32-unknown-unknown` target:
  `rustup target add wasm32-unknown-unknown`
- `cargo fmt` and `cargo clippy` components (installed by default with
  `rustup component add rustfmt clippy`)

### Build, Test, Format, and Lint

Run these from the `contracts/` directory (or the workspace root):

```bash
cd contracts

cargo build                              # Build all contracts
cargo build --target wasm32-unknown-unknown --release  # Build deployable Wasm
cargo test                               # Run all contract tests
cargo fmt --all                          # Format Rust code
cargo fmt --all -- --check               # Verify formatting (CI)
cargo clippy --all-targets -- -D warnings  # Lint, fail on warnings
```

Before opening a PR, make sure `cargo fmt --all -- --check` and
`cargo clippy --all-targets -- -D warnings` both pass, and that
`cargo test` is green.

### Adding a New Contract

1. Create a new crate under `contracts/`, e.g. `contracts/my_contract/`,
   with its own `Cargo.toml` and `src/lib.rs`.
2. Register the crate as a workspace member in `contracts/Cargo.toml`
   (add it to the `members` list) so it is built and tested with the rest
   of the workspace.
3. Implement the contract using the `soroban-sdk` crate, annotating the
   entry point with `#[contract]` and its methods with `#[contractimpl]`.
4. Add unit tests in the crate (typically a `#[cfg(test)]` module in
   `src/lib.rs` or a `tests/` directory) using `soroban_sdk::Env`.
5. If the contract emits events or exposes storage that the indexer or
   frontend consumes, update the relevant TypeScript types and document
   the new contract in this guide.
6. Run `cargo fmt --all`, `cargo clippy --all-targets -- -D warnings`, and
   `cargo test` before submitting.

### Soroban / Stellar Documentation

- Soroban smart contracts: https://developers.stellar.org/docs/smart-contracts
- Soroban Rust SDK (`soroban-sdk`): https://docs.rs/soroban-sdk
- Stellar developer docs: https://developers.stellar.org/docs
- Stellar SDKs (JS, Rust, and others): https://developers.stellar.org/docs/tools/sdks

## Code Style

Run the linter before submitting:

```bash
cd frontend && npm run lint
```

For Rust contracts, run `cargo fmt --all` and
`cargo clippy --all-targets -- -D warnings` from `contracts/`.

## Issues

Check open issues for tasks labeled `good first issue` or `help wanted`.
