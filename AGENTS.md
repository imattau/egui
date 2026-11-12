# Repository guidance for agents

This file summarizes the repository's contribution and architecture guidance. Read [`CONTRIBUTING.md`](CONTRIBUTING.md) and [`ARCHITECTURE.md`](ARCHITECTURE.md) for the full context, especially before making broad or cross-crate changes.

## Before changing code

- Read nearby code and the README for the crate or example you are changing. Follow existing patterns and keep changes small and focused.
- Identify the crate that owns the behavior. Preserve the dependency boundaries described in `ARCHITECTURE.md`; keep `egui` platform independent and its dependencies minimal. Backend, window-system, framework, and optional functionality belongs in their corresponding crates.
- For a substantial feature or architectural change, first check whether an issue or discussion exists and whether the maintainers have agreed on the direction.
- Treat code quality and maintainability as requirements. Avoid unnecessary complexity and dependencies.

## Rust and API style

- Write idiomatic Rust and follow the [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/).
- Avoid `unsafe`, panics, `unwrap`, and `expect`. Handle missing or invalid data explicitly; prefer methods such as `.first()` or `.get()` where indexing could panic.
- Give public types, public functions, and public struct fields useful documentation. Add examples or doctests for APIs where practical.
- Add blank lines around functions and type definitions. Use `// Comment like this.` and use `TODO` rather than `FIXME`; include a GitHub handle in new TODOs, for example `TODO(emilk): ...`.
- Prefer clear names, avoid double negatives, and consider extracting a helper before making a function substantially longer. Keep imports local to a function when they are only used there; import a trait as `use Trait as _;` when only its methods are needed.
- Sort sets of entries such as Cargo dependencies lexicographically. Prefer one type per file unless the type is trivial. Break these conventions when doing so makes the code clearer.
- Keep coordinate and angle behavior consistent: screen origin is top-left, +X points right, +Y points down; positions use logical points scaled by `pixels_per_point`; angles are clockwise radians from +X.

## Compatibility and documentation

- Consider downstream users when changing public APIs or behavior. Document breaking changes in the PR description; where practical, deprecate old APIs before removing them.
- Keep crate READMEs and examples accurate when changing crate responsibilities, usage, features, or platform support. Update `ARCHITECTURE.md` when crate relationships or architectural boundaries change.
- Add links to the root README's integrations list when contributing a new egui integration.

## Tests and checks

- Add or update focused tests for behavior changes. For UI snapshot changes, see [`crates/egui_kittest/README.md`](crates/egui_kittest/README.md).
- The repository's CI-like local check is `./scripts/check.sh`. It runs formatting, linting, docs, compilation across feature/platform combinations, tests, and policy checks; it can take time and needs the relevant toolchains and dependencies installed.
- To intentionally regenerate all snapshots, use `UPDATE_SNAPSHOTS=true cargo test --workspace --all-features`. Snapshot rendering can differ by platform; for CI-rendered snapshots, see `./scripts/update_snapshots_from_ci.sh` in `CONTRIBUTING.md`.
- Snapshot and other large files use Git LFS. If a snapshot test reports `InvalidSignature`, check the local Git LFS setup and fetched files.

## Pull request expectations

- Keep each change focused and its PR description short and useful. Include user-visible behavior changes and breaking changes.
- Review the diff for readability, useful documentation, and adherence to this repository's style before considering work complete. A human must review the code before a PR is undrafted, as required by `CONTRIBUTING.md`.

