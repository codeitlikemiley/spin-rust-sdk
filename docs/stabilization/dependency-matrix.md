# WASI HTTP dependency matrix

Agent 2 audit of the WASI HTTP / Spin / Leptos stack. This file is the reviewable handoff for repos this agent does not own. Only `codeitlikemiley/spin-rust-sdk` is modified here.

Guest crates that git-depend on this fork should pin `rev = "3cf30012b50a1afb750dac3d28aa6da4a32c3468"` on branch `gol/wasi-dep-stabilization-1365`. That commit is the dependency bump. Later doc-only commits do not change it. After the PR merges, replace the pin with the merge commit if `main` is not that SHA.

## How versions were verified

Resolved versions come from `Cargo.lock`, `cargo metadata --locked --format-version 1`, and `cargo tree --locked`. Declared requirements are quoted only to explain why the resolver stopped where it did.

Commands were run on 2026-10-24.

| Tool | Exact version | Role |
| --- | --- | --- |
| rustc | `1.99.0 (b940084d7 2026-09-28)` | Policy pin. Channel manifest date `2026-10-01`. Not `stable`. |
| cargo | `1.99.0 (5f94df478 2026-08-27)` | Lock updates, tree, metadata, tests |
| rustc MSRV probe | `1.94.0 (4a4ef493e 2026-03-02)` | `cargo +1.94.0 check -p spin-sdk --locked` |
| stable channel | `1.99.1` dated `2026-10-15` | Observed and not used |

`cargo +1.99.0 metadata --locked --format-version 1` on this workspace returned resolve ids including `wasip3@0.9.0+wasi-0.3.0`, `wit-bindgen@0.62.0`, `wit-bindgen@0.51.0`, `wasmtime@50.0.0`, `wit-component@0.259.0`, `wit-component@0.261.0`, `wit-component@0.262.0`, `wasip2@1.0.2+wasi-0.2.9`, `wasi@0.11.1+wasi-snapshot-preview1`, `getrandom@0.2.17`, `getrandom@0.3.4`, `getrandom@0.4.3`.

Other repositories were cloned read-only under `/tmp/wasi-audit` and parsed from `Cargo.toml` / `Cargo.lock`. `cargo metadata` was not re-run inside those trees. Their lockfile numbers below are resolved, not declared, unless a row says declared.

Spin CLI was not installed in this environment. CLI versions are the workspace `package.version` in source. A running `spin --version` is **UNVERIFIED**.

## Repository heads

| Repo | SHA | Commit date | Notes |
| --- | --- | --- | --- |
| `codeitlikemiley/spin-rust-sdk` `main` before this PR | `f0c4aeb2c2c44804906a9bc818397050b45c622d` | wasip3 merge | 0 ahead / 6 behind upstream |
| `spinframework/spin-rust-sdk` `main` | `2dfd93d2e76e89d7cf74ca43140be7730e0746eb` | 2026-08-25 | tag `v7.0.0` |
| `codeitlikemiley/spin` | `c34c584dbf77b3a3528ad0536aa9ce4761b9f772` | 2026-07-10 | 0 ahead / 196 behind upstream |
| `spinframework/spin` | `f1a5577eeac25476d8eced8d08414ac4b8042a94` | 2026-10-23 | workspace `4.3.0-pre0` |
| `codeitlikemiley/leptos-spin` | `db9784b295ad3707fae9c11f7dc1657e2f039c83` | 2026-06-02 | templates only |
| `spinframework/leptos-spin` | `480f1f76a40698489e18a98a75a6d4b4defbcbd0` | 2026-06-03 | locks `wasm-bindgen` 0.2.114; still `wasip3` 0.6.0 |
| `codeitlikemiley/leptos_wasi` | `6727a211b313e78f6760c3d9378728fdf7a3e91b` | 2026-09-17 | crate version `0.4.2` |
| `codeitlikemiley/wasi-auth` | `31aef996f196e54d0ff5d584dfd4add4340850c1` | 2026-09-05 | `0.1.0-rc.5` |
| `codeitlikemiley/wasi-http-middleware` | `61b41995812220c69c4a10530efb0cd5f7522840` | 2026-07-11 | `0.2.0-alpha.3` |
| `codeitlikemiley/ddd-cqrs-es` | `71ac721f48cf236244abc76c17d39e2cd58e1341` | 2026-09-06 | pins SDK side branch |

Side pin used by wasi-auth and ddd-cqrs-es:

`a02d330fe9357be2d18e6deef400511195ce6f7f` (2026-07-12, `build: restore Rust 1.93 compatibility`) is `origin/codex/final-wasi-rust-1.93`, tag `final-wasi-0.7-rust-1.93.0`. Parent is `f0c4aeb`. It is **not** an ancestor of upstream `v7.0.0`. That commit declares `spin-sdk` 6.0.0, `wasip3` 0.7.0, `rust-version` 1.93.

## crates.io targets checked 2026-10-24

| Crate | Newest version observed | MSRV field | Date |
| --- | --- | --- | --- |
| `wasip3` | `0.9.0+wasi-0.3.0` | 1.87.0 | 2026-09-10 |
| `wasip2` | `2.0.1+wasi-0.2.12` (1.x latest `1.0.4+wasi-0.2.12`) | 1.87.0 | 2026-09-22 / 2026-06-12 |
| `wasi` | `0.14.7+wasi-0.2.4` and preview1 `0.11.1+wasi-snapshot-preview1` | 1.82.0 on 0.14.7 | 2025-09-15 |
| `wit-bindgen` | `0.63.0` | unset on 0.63.0 | 2026-10-14 |
| `wit-component` / `wit-parser` | `0.262.0` | 1.88.0 | 2026-10-13 |
| `wasmtime` / `wasmtime-wasi-http` | `50.0.0` | 1.97.0 | 2026-10-20 |
| `spin-sdk` | `7.0.0` | 1.94 | 2026-08-25 |
| `leptos` | stable `0.8.22`; prerelease `0.9.0-rc0` | 1.88 | 2026-10-05 / 2026-10-15 |
| `server_fn` | stable `0.8.13`; prerelease `0.9.0-rc0` | 1.88 | 2026-06-25 / 2026-10-15 |
| `wasm-bindgen` | `0.2.129` | 1.81 | 2026-09-25 |
| `getrandom` | `0.4.3` | 1.85 | 2026-06-17 |
| `leptos-wasi-runtime` | `0.4.2` (also `0.4.2-rc.1`) | 1.93.0 | 2026-08-05 / 2026-07-13 |

`wasip3` 0.7.1, 0.8.0, and 0.9.0 all embed `package wasi:http@0.3.0`. `diff -rq` of the 0.7.1 and 0.9.0 crate sources did not report WIT file differences. The Rust binding and `wit-bindgen` pin changed: 0.7.1 uses `wit-bindgen` 0.57.1, 0.8.0 uses 0.61.0, 0.9.0 uses 0.62.0. Do not bump `wit-bindgen` past the version `wasip3` pins.

`wasip3` 0.4.0 is `+wasi-0.3.0-rc-2026-01-06`. `wasip3` 0.6.0 is `+wasi-0.3.0-rc-2026-03-15`. Those are not the final `wasi:http@0.3.0` package.

`getrandom` 0.4.3 changelog: "Drop `wasip2` and `wasip3` dependencies in favor of manual bindings". It links `wasi:random/random@0.3.0` directly. `wasip3` 0.8.0 and 0.9.0 renamed `wit_bindgen::spawn` to `wit_bindgen::spawn_local` and still gate that re-export on feature `async-spawn`. `http-compat` enables `async-spawn`.

`leptos` 0.8.22 (downloaded crate) depends on `server_fn` 0.8.13, `leptos_macro` `=0.8.19`, `leptos_dom` 0.8.8, `leptos_config` 0.8.10, `leptos_hot_reload` 0.8.7, `leptos_server` 0.8.8, `wasm-bindgen` 0.2. `wit-bindgen` 0.62.0 still has features `async-spawn` and `inter-task-wakeup`.

## Matrix

Classification key: **Fixed upstream** / **Dep update** / **Upstream PR** / **Internal** / **Pinned**.

| Repo | Dependency | Resolved before this PR | Target | Class | Evidence |
| --- | --- | --- | --- | --- | --- |
| spin-rust-sdk fork `main` | `wasip3` direct | `0.7.0+wasi-0.3.0` | `0.9.0+wasi-0.3.0` | Dep update. `0.7.1` is Fixed upstream in v7 and is not the latest final binding | root `Cargo.lock`; crates.io 0.9.0; WIT package still `wasi:http@0.3.0` |
| spin-rust-sdk fork `main` | `wasip3` duplicate | `0.4.0+wasi-0.3.0-rc-2026-01-06` via `getrandom` 0.4.2 | removed | Dep update | `getrandom` 0.4.3 manual `wasi:random/random@0.3.0` |
| spin-rust-sdk upstream v7 | `wasip3` direct | `0.7.1+wasi-0.3.0` | `0.9.0+wasi-0.3.0` | Dep update, not yet upstream | upstream lock still has 0.4.0 beside 0.7.1 |
| spin-rust-sdk upstream v7 | `wasmtime` / `wasmtime-wasi` / `wasmtime-wasi-http` | declared `47.0.2`, resolved `47.0.4` | `50.0.0` | Dep update to match `spinframework/spin` `f1a5577` | upstream Spin workspace dep and root lock are `50.0.0`; MSRV 1.97.0 |
| spin-rust-sdk fork `main` | same wasmtime trio | declared and resolved `46.0.1` | `50.0.0` | Dep update. 47.0.4 is Fixed upstream and still behind Spin | fork lock |
| spin-rust-sdk | `wit-bindgen` for the guest | `0.57.1` via `wasip3` 0.7.x, plus `0.51.0` via `wasip2` 1.0.2 | `0.62.0` for the guest. Keep `0.51.0` only as the feature-stripped `wasip2` edge | Dep update for 0.62. `0.63.0` would fight `wasip3` 0.9.0's exact `0.62.0` | `wasip3` 0.9.0 `Cargo.toml`; `cargo tree --target all -i wit-bindgen@0.51.0` |
| spin-rust-sdk | `wit-component` dev-dep | declared `0.247`, plus `0.244` from `wit-bindgen` 0.51 | direct `0.262.0` | Dep update to Spin's wasm-tools line | Spin workspace pins `wit-component` `0.262.0` |
| spin-rust-sdk | `wit-component` duplicates that remain | n/a | `0.259.0` from `wit-bindgen-rust` 0.62.0; `0.261.0` from `wasmtime-internal-wit-bindgen` 50.0.0; `0.262.0` direct | Pinned by those crates | `cargo tree -d` |
| spin-rust-sdk | `wasip2` | `1.0.2+wasi-0.2.9` via `getrandom` 0.3.4 | leave on 1.0.2 | Pinned. `1.0.4` switches that edge to `wit-bindgen` 0.57.1 and brings `wit-component` 0.247 back. `2.0.1` is a new major `getrandom` 0.3 does not accept | `wasip2` 1.0.4 crate toml; tree above |
| spin-rust-sdk | `wasi` preview1 | `0.11.1+wasi-snapshot-preview1` via `getrandom` 0.2.17 | leave | Pinned by `getrandom` 0.2 | lock |
| spin-rust-sdk | nested test locks | `spin-sdk` 6.0.0, `wasip3` 0.6.0 RC and 0.4.0, even on upstream v7 | `spin-sdk` 7.0.0, single `wasip3` `0.9.0+wasi-0.3.0`, `wit-bindgen` 0.62.0 | Internal. Upstream v7 did not refresh these locks | `crates/spin-sdk/test-cases/*/Cargo.lock` |
| spin-rust-sdk | Rust | declared `1.94`. CI was `1.94` | library `rust-version` stays `1.94`. CI and `rust-toolchain.toml` pin `1.99.0` | Internal toolchain pin. Wasmtime 50 needs `>=1.97` to compile dev-deps | `cargo +1.94.0 check -p spin-sdk` succeeded; `cargo +1.99.0 test -p spin-sdk` succeeded |
| spin-rust-sdk | package version | fork 6.0.0, upstream 7.0.0 | stay `7.0.0` | No release. This PR is not a crates.io publish | release workflow still gated on `github.repository_owner == 'spinframework'` |
| `codeitlikemiley/spin` | `wasmtime` 46.0.1, `wit-*` 0.247, `rust-version` 1.86 | lock matches those declarations | upstream `f1a5577`: wasmtime `50.0.0`, wit `0.262.0`, `rust-version` 1.96, CI `1.97` | Fixed upstream. Fork is 196 commits behind | upstream `Cargo.toml` and root lock |
| `spinframework/spin` | `wasip3` crate | not a root dependency | host uses `wasmtime-wasi-http` 50 `p3` | Pinned to the Wasmtime embedding of `wasi:http@0.3.0` | root lock has `wit-bindgen` 0.57.1 and `wasip2` `1.0.4+wasi-0.2.12`, no `wasip3` package |
| `spinframework/spin` | historical `spin-sdk` `2.2.0` / `3.0` / `5.x` test components | still on upstream `f1a5577` | leave | Pinned. They are old ABI fixtures, present on both fork and upstream | test-component `Cargo.toml` files |
| `spinframework/spin` | `spin-sdk` guest examples | `examples/http-rust` and `open-ai-rust` declare `7.0.0` | move to this PR's git rev only if they need `wasip3` 0.9 / `spawn_local` | Dep update after this PR. crates.io 7.0.0 is still `wasip3` 0.7.1 | example manifests |
| `spinframework/spin` | git revs `cargo-target-dep` `482f269`, `conformance-tests` `ffc2c879` | same rev on the July fork and the Oct 23 upstream | leave | Pinned. Same rev upstream, not abandoned | both `Cargo.toml` files |
| leptos-spin fork and upstream | `wasip3` | templates declare `0.6.0` (RC `2026-03-15`) | `0.9.0` with `http-compat` | Dep update. Not fixed upstream | both template `Cargo.toml` files |
| leptos-spin | `spin-sdk` | templates declare `6.0.0` | `7.0.0` plus this PR's rev for `spawn_local` | Dep update | templates |
| leptos-spin | `leptos_wasi` | templates declare `0.3.1` | `leptos-wasi-runtime` `0.4.2` after A5 retargets `wasip3` | Dep update | crates.io max `0.4.2` |
| leptos-spin | `leptos` / `server_fn` | fork templates `leptos` 0.8.9, `server_fn` 0.8.7. Upstream templates `leptos` 0.8.19, `server_fn` 0.8.12, `wasm-bindgen` `=0.2.114` | `leptos` 0.8.22, `server_fn` 0.8.13, `leptos_macro` `=0.8.19`, `wasm-bindgen` `=0.2.129` | Dep update. Upstream only fixed the bindgen pin | downloaded `leptos` 0.8.22 crate |
| leptos_wasi | `wasip3` | lock `0.7.0+wasi-0.3.0`, single `wit-bindgen` 0.57.1 | `=0.9.0`, which pulls `wit-bindgen` 0.62.0 | Dep update | root and fixture locks |
| leptos_wasi | `leptos` / `server_fn` | lock `leptos` 0.8.20, `server_fn` 0.8.13, `wasm-bindgen` 0.2.126 | `leptos` 0.8.22 (still `server_fn` 0.8.13) and `wasm-bindgen` 0.2.129 | Dep update inside 0.8. Do not jump to `0.9.0-rc0` in this pass | crate tomls |
| leptos_wasi | `wasip3::spawn` | `src/executor/p3.rs` calls `wasip3::spawn` for both `spawn` and `spawn_local` | `wasip3::spawn_local` | Internal once the dep moves. `wasip3` 0.9 has no `spawn` | crate source |
| wasi-http-middleware | `wasip3` / `wit-bindgen` | lock `wasip3` 0.7.0, `wit-bindgen` 0.57.1 and 0.59.0, `wit-component` 0.253.0 | `wasip3` `=0.9.0`, `wit-bindgen` `=0.62.0` | Dep update. The `=0.59.0` pin is a second bindgen next to the one `wasip3` 0.7 pulls | root lock |
| wasi-http-middleware | WIT | vendored `wit/deps/http.wit` is `package wasi:http@0.3.0` | keep that package | Already the final package. The Rust crate pin is what is old | `wit/deps/http.wit` line 1 |
| wasi-auth | `spin-sdk` | git `a02d330` resolved as 6.0.0 | this PR's commit, version 7.0.0 | Dep update. The pin is a 1.93 side branch, not `main` | `git merge-base --is-ancestor` failed |
| wasi-auth | `wasip3` / `wit-bindgen` | lock `wasip3` 0.7.0, `wit-bindgen` 0.57.1 and 0.59.0 | `=0.9.0` and `=0.62.0` | Dep update | root lock |
| wasi-auth | `server_fn` | declared `0.8.12`, resolved `0.8.13` | declare `0.8.13` | Internal declaration drift. `leptos` 0.8.20 requires 0.8.13 | workspace toml vs lock |
| wasi-auth | `wasip3::wit_bindgen::spawn` | `crates/wasi-auth/src/spin_grpc.rs` | `wasip3::spawn_local` or `wasip3::wit_bindgen::spawn_local` | Internal with the dep bump | source |
| ddd-cqrs-es | `wasip3` | declared `0.7`, resolved `0.7.1+wasi-0.3.0`, single `wit-bindgen` 0.57.1 | `=0.9.0` | Dep update. Resolver already left 0.7.0 | root lock |
| ddd-cqrs-es | `spin-sdk` | git `a02d330` resolved 6.0.0 | this PR, version 7.0.0 | Dep update | lock source URL |
| ddd-cqrs-es | `leptos-wasi-runtime` | examples declare `=0.4.2-rc.1` | `=0.4.2` after A5 ships the 0.9 guest, or a git rev of A5's branch until then | Dep update. Comment in `Cargo.toml` still says the rc is the shared final binding | example manifests |
| ddd-cqrs-es | browser wasm | lock `wasm-bindgen` 0.2.128; examples declare `=0.2.126` | `=0.2.129` | Dep update. Declaration and lock already disagree | lock vs toml |

`leptos` 0.9.0-rc0 is intentionally not the target. The fullstack crates are on the 0.8 line, and 0.8.22 still resolves `server_fn` 0.8.13.

## What this PR changes

Fast-forward `f0c4aeb` to upstream `2dfd93d` (`v7.0.0`, `wasip3` 0.7.1, wasmtime declared 47.0.2 / resolved 47.0.4), then:

- `crates/spin-sdk/Cargo.toml`: `wasip3` `0.9.0` with `http-compat` and `wit-bindgen-macros`; dev `wasmtime`, `wasmtime-wasi`, `wasmtime-wasi-http` `50.0.0` (`p3` on the http crate); dev `wit-component` `0.262`.
- `Cargo.lock`: `getrandom` 0.4.2 to 0.4.3, which deletes `wasip3` `0.4.0+wasi-0.3.0-rc-2026-01-06`. Guest `wit-bindgen` is 0.62.0. `http` moved 1.4.0 to 1.5.0 because wasmtime 50's range allows it.
- `rust-toolchain.toml` and CI `RUST_VERSION` are `1.99.0`. Workspace `rust-version` stays `1.94`.
- Call sites of `wasip3::spawn` in this repo are `wasip3::spawn_local`.
- `crates/spin-sdk/src/test.rs` follows Wasmtime 50: HTTP view types and `default_hooks()` live at `wasmtime_wasi_http::*`, and `Request::from_http` takes hooks first. `p3::add_to_linker`, `p3::Request`, and `p3::bindings::Service` stay under `p3`.
- Nested `simple-http` and `simple-redis` locks regenerated. They no longer resolve `wasip3` 0.6.0.
- Three examples that declared `rust-version = "1.90"` now use `rust-version.workspace` (`1.94`). The comment that 1.90 was required to build WASIp3 on `wasm32-wasip2` was stale.
- `examples/grpc` and `examples/grpc-streaming` allow `clippy::result_large_err` and `clippy::double_must_use` because Rust 1.99 clippy flags tonic's generated traits. `cargo clippy -p spin-sdk` is clean without those allows.

Package version stays `7.0.0`. This is not a release.

## Exact edits for other owners

Apply these in the owning repo. Then run the verification block at the bottom. Do not `cargo update` the whole tree unless a command below says so.

### A1 `codeitlikemiley/spin`

Do not hand-edit wasmtime from 46 to 50. Merge `spinframework/spin` `f1a5577eeac25476d8eced8d08414ac4b8042a94`. That commit already contains:

```toml
rust-version = "1.96"
wasmtime = "50.0.0"
wasmtime-wasi = { version = "50.0.0", features = ["p3"] }
wasmtime-wasi-http = { version = "50.0.0", features = ["p3", "component-model-async"] }
wit-component = "0.262.0"
wit-parser = "0.262.0"
wasmparser = "0.262.0"
```

Upstream CI `RUST_VERSION` is `1.97`, which is Wasmtime 50's MSRV. Leave historical `spin-sdk = "2.2.0"` test components alone.

After this SDK PR merges, `examples/http-middleware/{yelling,animal-fact,app}` track `spinframework/spin-rust-sdk` `branch = "main"`, which will not contain `spawn_local` until upstream takes the same bump. `examples/http-middleware/yelling/src/lib.rs` calls `spin_sdk::wasip3::spawn`. Point those three git deps at `3cf30012b50a1afb750dac3d28aa6da4a32c3468`, or wait for an upstream SDK PR, and rename that call to `spawn_local`.

`examples/http-rust` and `examples/open-ai-rust` already declare `spin-sdk = "7.0.0"`. That crates.io crate is `wasip3` 0.7.1. Switch them to `3cf30012b50a1afb750dac3d28aa6da4a32c3468` only when they need 0.9.

### A3 `codeitlikemiley/wasi-http-middleware`

`Cargo.toml` workspace:

```toml
rust-version = "1.94"
wit-bindgen = { version = "=0.62.0", features = ["async-spawn"] }
wasip3 = { version = "=0.9.0", features = ["http-compat"] }
```

`rust-toolchain.toml`:

```toml
[toolchain]
channel = "1.99.0"
components = ["clippy", "rustfmt"]
targets = ["wasm32-wasip2"]
profile = "minimal"
```

Replace `wasip3::spawn(` with `wasip3::spawn_local(` in `test-components/echo-service/src/lib.rs` and `test-components/mock-authn-broker/src/lib.rs`.

```bash
cargo +1.99.0 update -p wasip3 --precise 0.9.0+wasi-0.3.0
cargo +1.99.0 update -p wit-bindgen --precise 0.62.0
```

`1.93.0` was not recompiled against `wasip3` 0.9.0 here. `1.94.0` compiled `wasip3` 0.9.0 inside spin-sdk. Treat `1.93` as insufficient until A3 proves otherwise.

### A4 `codeitlikemiley/wasi-auth`

Workspace dependency edits:

```toml
leptos = { version = "0.8.22", default-features = false, features = ["ssr"] }
server_fn = { version = "0.8.13", default-features = false, features = ["axum-no-default"] }
wasip3 = { version = "=0.9.0", features = ["http-compat"] }
wit-bindgen = { version = "=0.62.0", features = ["async-spawn"] }
```

`crates/wasi-auth/Cargo.toml` spin-sdk git dep, keeping the existing feature list:

```toml
spin-sdk = { git = "https://github.com/codeitlikemiley/spin-rust-sdk", rev = "3cf30012b50a1afb750dac3d28aa6da4a32c3468", version = "7.0.0", default-features = false, features = ["json", "pg"], optional = true }
```

`crates/wasi-auth/src/spin_grpc.rs`: `wasip3::wit_bindgen::spawn` becomes `wasip3::spawn_local`.

The legacy middleware tree under `legacy/wasi-http-middleware` is a second copy of A3. Apply A3's pin there too, or delete the duplicate from the resolver if it is truly legacy and unused. Today it is why the lock contains `wit-bindgen` 0.59.0 beside 0.57.1.

Raise `rust-version` to `1.94` and the toolchain channel to `1.99.0`, same caveat as A3.

### A5 `codeitlikemiley/leptos_wasi`

Root `Cargo.toml`:

```toml
rust-version = "1.94"
wasip3 = { version = "=0.9.0", features = ["http-compat"], optional = true }
leptos = { version = "0.8.22", default-features = false, features = ["nonce", "ssr"] }
leptos_macro = { version = "=0.8.19", default-features = false, features = ["generic"] }
server_fn = { version = "0.8.13", features = ["generic", "axum-no-default"], default-features = false }
```

Keep the existing feature sets on `leptos_meta` / `leptos_router` but let `cargo update -p leptos` select the 0.8 versions that 0.8.22 pulls (`leptos_dom` 0.8.8, `leptos_config` 0.8.10, `leptos_hot_reload` 0.8.7, `leptos_server` 0.8.8 were the versions inside the `leptos` 0.8.22 crate). Do not invent a newer `leptos_router` than the resolver prints.

`src/executor/p3.rs`, both methods:

```rust
wasip3::spawn_local(future);
```

Every test and example `Cargo.toml` that says `wasip3 = { version = "=0.7.0" ...}` becomes `=0.9.0`. `tests/authz-lifecycle` pins `wit-bindgen = { version = "=0.59.0", features = ["async-spawn"] }`; change that to `=0.62.0`. Example `wasm-bindgen = { version = "=0.2.126" ...}` becomes `=0.2.129`.

Fixture `server_fn = "0.8.12"` should be `0.8.13` so the declaration matches the lock and `leptos` 0.8.22.

### A6 `codeitlikemiley/leptos-spin` and upstream `spinframework/leptos-spin`

Upstream `480f1f7` only locked `wasm-bindgen` to `=0.2.114`. Both trees still template `wasip3` 0.6.0. That is the March 2026 RC, not `wasi:http@0.3.0`.

In `templates/leptos-spin/content/Cargo.toml` and `templates/leptos-wasmtime/content/Cargo.toml`:

```toml
rust-version = "1.94"
leptos = { version = "0.8.22" }
server_fn = { version = "0.8.13", features = ["axum-no-default"] }
leptos_wasi = { package = "leptos-wasi-runtime", version = "=0.4.2", default-features = false, features = ["wasip3"], optional = true }
spin-sdk = { version = "7.0.0", optional = true }
wasip3 = { version = "=0.9.0", features = ["http-compat"], optional = true }
wasm-bindgen = { version = "=0.2.129", optional = true }
```

`spin-sdk = "7.0.0"` is crates.io, which is `wasip3` 0.7.1 and `spawn`, not `spawn_local`. If the template calls `wasip3::spawn` through the SDK, it will not compile against 0.9 until spin-sdk 7.0.0 grows that re-export upstream. Until an upstream SDK release exists, template the git rev of this PR:

```toml
spin-sdk = { git = "https://github.com/codeitlikemiley/spin-rust-sdk", rev = "3cf30012b50a1afb750dac3d28aa6da4a32c3468", version = "7.0.0", optional = true }
```

Set `leptos_meta`, `leptos_router`, and `leptos_macro` from `cargo update -p leptos` after the 0.8.22 bump. `leptos` 0.8.22 requires `leptos_macro` `=0.8.19`. This agent did not open a fresh leptos app, so template compile is **UNVERIFIED**.

Also fast-forward the fork to `480f1f7` before editing so the wasm-bindgen lock commit is not dropped.

### A7 `codeitlikemiley/ddd-cqrs-es`

Root `Cargo.toml`:

```toml
rust-version = "1.94"
wasip3 = { version = "=0.9.0", features = ["http-compat"], optional = true }
spin-sdk = { git = "https://github.com/codeitlikemiley/spin-rust-sdk", rev = "3cf30012b50a1afb750dac3d28aa6da4a32c3468", version = "7.0.0", optional = true }
```

Replace the comment that names `leptos-wasi-runtime` `0.4.2-rc.1` and `wasip3` 0.7 as the shared final binding. The final WIT package is still `wasi:http@0.3.0`; the binding crate target is `wasip3` 0.9.0.

`examples/counter-app/Cargo.toml` and `examples/fullstack-app/Cargo.toml`:

```toml
rust-version = "1.94"
leptos = { version = "0.8.22", default-features = false }
server_fn = { version = "0.8.13", default-features = false }
leptos_wasi = { package = "leptos-wasi-runtime", version = "=0.4.2", default-features = false, features = ["wasip3", "islands-router", "tracing"], optional = true }
spin-sdk = { git = "https://github.com/codeitlikemiley/spin-rust-sdk", rev = "3cf30012b50a1afb750dac3d28aa6da4a32c3468", version = "7.0.0", optional = true }
wasip3 = { version = "=0.9.0", features = ["http-compat"], optional = true }
wit-bindgen = { version = "=0.62.0", features = ["async-spawn", "inter-task-wakeup"], optional = true }
wasm-bindgen = { version = "=0.2.129", optional = true }
```

`=0.4.2` only works after A5 publishes a 0.4.2 that depends on `wasip3` 0.9.0. Until then use a git dependency on A5's branch. The published `0.4.2` (2026-08-05) is older than leptos_wasi git `6727a21` (2026-09-17) and still declares `wasip3` 0.7.0 in that git tree. Whether the published tarball matches that git commit was not byte-compared (**UNVERIFIED**).

`examples/counter-app/redis-trigger/Cargo.toml` declares `spin-sdk = "6.0.0"`. Change it to `7.0.0` or the same git rev. `6.0.0` resolves `wasip3` 0.7.0 / `spawn`, which will not match a 0.9 guest.

## Verification commands

Run from the repo being updated, with the toolchain pin:

```bash
rustc +1.99.0 --version
cargo +1.99.0 --version
cargo +1.99.0 update -p wasip3 --precise 0.9.0+wasi-0.3.0
cargo +1.99.0 metadata --locked --format-version 1 \
  | python3 -c 'import json,sys; d=json.load(sys.stdin); want=("wasip3","wasip2","wasi","wit-bindgen","wit-component","wasmtime","getrandom","leptos","server_fn","wasm-bindgen","spin-sdk")
for n in d["resolve"]["nodes"]:
    pid=n["id"].split("#")[-1]
    if pid.split("@")[0] in want or pid.split("@")[0].startswith("wasmtime"):
        print(pid)'
cargo +1.99.0 tree --locked --target all -i wasip3 -i wit-bindgen@0.62.0 -i wit-bindgen@0.51.0
cargo +1.99.0 test --locked
```

Pass condition for a guest crate: one `wasip3@0.9.0+wasi-0.3.0`, guest `wit-bindgen@0.62.0`, no `wasip3@0.4` and no `wasip3@0.6`. `wit-bindgen@0.51.0` may remain only under `wasip2` 1.0.2 / `getrandom` 0.3.4. `wit-component` 0.259 and 0.261 remaining beside 0.262 is the wasmtime 50 / wit-bindgen 0.62 split, not a mistake in the direct pin.

Host Spin pass condition is different: root lock `wasmtime@50.0.0` and `wit-component@0.262.0`, matching `f1a5577`. Do not add a direct `wasip3` dependency to the Spin CLI.

## Tested on this PR

- `cargo +1.99.0 test -p spin-sdk --all-features --locked` — 31 lib tests passed, including `test::simple_http` and `test::simple_redis` (Wasmtime 50 loaded `wasm32-wasip2` components). Doctests passed, including `http::body::stream`.
- `cargo +1.99.0 fmt --all -- --check`
- `cargo +1.99.0 clippy --workspace --all-targets --all-features --locked -- -D warnings`
- `cargo +1.99.0 check --no-default-features --locked`
- `cargo +1.99.0 check --no-default-features --features json --locked`
- `cargo +1.99.0 check --no-default-features --features postgres4-types --locked`
- `cargo +1.94.0 check -p spin-sdk --locked` — library and `wasip3` 0.9.0 compiled. Dev-dependency Wasmtime 50 was not built by that command.

## Not tested

- `spin` CLI binary, `spin build --up`, and any example `spin.toml` runtime. Spin is not installed here.
- `cargo audit`. `.cargo/audit.toml` still ignores `RUSTSEC-2026-0204` and `RUSTSEC-2026-0258`. Those ignores were inherited from upstream v7. This PR did not re-evaluate them.
- Compiling A3, A4, A5, A6, or A7 after the recommended edits.
- `leptos` 0.9 prereleases.
- Whether published `leptos-wasi-runtime` 0.4.2 bytes match git `6727a21`.
- A full `cargo +1.94.0 test`. Wasmtime 50's crate MSRV is 1.97.0, so the 1.94 toolchain cannot compile the dev-dependency tests. That is why CI is 1.99.0 while `package.rust-version` stays 1.94.
