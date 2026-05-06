---
name: rust
description: Rust project conventions with cargo workspaces and proper error handling.
activation:
  - "*.rs"
  - "Cargo.toml"
  - "rust"
  - "cargo"
---

# Rust Project Conventions

## Edition
- Use `edition = "2024"` (latest stable: Rust 1.94)

## Project Structure
- Module first, crate last — prefer deep module trees in a single lib crate over premature workspace splitting (crate boundaries block cross-crate optimization)
- Split into workspace crates only for genuinely independent compilation units
- Use `[workspace.dependencies]` with `workspace = true` for version dedup

## Tooling
- `cargo-nextest` as the default test runner (faster, per-process isolation)
- `cargo deny check` in CI for license/security audits
- `cargo machete` / `cargo udeps` for unused dependency detection

## Error Handling
- **Libraries**: `thiserror` for typed error enums — never `Box<dyn Error>` (callers can't match)
- **Applications**: `anyhow` for ergonomic contextual propagation
- **Complex multi-crate**: `snafu` for context-driven errors with location tracking
- **CLI / user-facing**: `miette` for diagnostic-rich errors with source highlighting
- **Instrumented services**: `tracing-error` for attaching span context to errors
- Never `.unwrap()` in library code — use `?` propagation. `expect("reason")` only where failure is a bug.

## Async
- Bound concurrency with `Semaphore`; prefer `JoinSet` over fire-and-forget spawns
- Design for cancellation safety — dropping a future mid-`.await` can corrupt state; audit every `.await` in state-mutating code

## Performance
- Profile before optimizing — `cargo flamegraph`, `criterion`, `dhat`
- `Bytes` / `&[u8]` for zero-copy networking; `Cow<'a, str>` for borrow-or-own
- `rkyv` for zero-copy deserialization in hot paths
- `jemalloc` or `mimalloc` via `#[global_allocator]` for multi-threaded workloads
- Avoid `Arc<Mutex<T>>` everywhere — consider `dashmap`, channels, or sharding
- Avoid excessive `.clone()` — borrow, move, or `Cow` instead

## Safety / Unsafe
- Minimize `unsafe` surface — isolate in dedicated modules, wrap in safe abstractions
- Every `unsafe` block requires a `// SAFETY:` comment; every `unsafe fn` requires `# Safety` doc
- Run `cargo miri test` in CI for any crate containing unsafe
- `cargo-careful` for extra std library runtime checks
- Never use `unsafe` to fight the borrow checker — restructure ownership instead

## Testing
- `cargo nextest run` for unit/integration
- `proptest` for property-based testing
- `cargo miri nextest run -jN` for parallel UB detection
- `insta` for snapshot testing; `rstest` for parameterized tests
- Fuzzing on nightly CI schedule, not per-PR

## Observability
- `tracing` over `log` — structured, span-based
- `#[instrument]` for automatic span creation
- `tracing-subscriber` + `EnvFilter` for dev; `tracing-opentelemetry` for production
- `metrics` crate with `metrics-exporter-prometheus` for counters/gauges/histograms

## Build Optimization
- `cargo-bloat` to identify dependency size contributions

## Anti-Patterns to Flag
- `.clone()` to satisfy borrow checker — restructure ownership
- `Arc<Mutex<Vec<_>>>` — design smell, consider channels or `dashmap`
- `.unwrap()` / `.expect()` in library code without justification
- `Box<dyn Error>` in library APIs — use typed error enums
- Premature crate splitting — module first
- Over-engineering with proc macros when functions/generics suffice
- `Deref` impl for non-smart-pointer newtypes
- Holding locks across `.await` points
