```yaml
number: 416
title: Add initial wasm32-wasi support
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: initial-wasi-support
created_at: 2022-10-13T11:51:03Z
updated_at: 2022-10-15T08:45:42Z
url: https://github.com/astral-sh/ruff/pull/416
synced_at: 2026-01-12T15:55:04Z
```

# Add initial wasm32-wasi support

---

_@konstin_

I'm currently experimenting whether it's feasible to make wasm32-wasi wheels. This is also the groundwork needed to make a web version of ruff, which might the more interesting use case.

Debug fails in wasmtime with "too many locals: locals exceed maximum", so release only atm.

Usage:

Either

```
cargo build --target wasm32-wasi --release --no-default-features
wasmtime run --dir . ~/ruff/target/wasm32-wasi/release/ruff.wasm .
```

or with latest main maturin

```
maturin build --target wasm32-wasi --release --strip --no-default-features
pip install target/wheels/ruff-0.0.69-py3-none-any.whl
ruff . # Currenly limit to . due to wasi isolation
```

I hope that https://github.com/magiclen/path-absolutize/pull/12 lands soon so i can switch back to crates.io path-absolutize before this gets merged

---

_Comment by @charliermarsh on 2022-10-13 13:19_

This is awesome. I did basic `wasm32-wasi` compilation on a few branches (`charlie/wasm` plus some local vendor-specific branches like `charlie/workers`) to power some of my Wasm explorations, but this is light-years better. Will review in-full + test when I get home later today.


---

_@charliermarsh reviewed on 2022-10-13 13:28_

---

_Review comment by @charliermarsh on `src/cache.rs`:1 on 2022-10-13 13:28_

Could we instead (or in addition) avoid calling into `cache` at all when `target_family = "wasm"`?

---

_@charliermarsh reviewed on 2022-10-13 13:28_

---

_Review comment by @charliermarsh on `Cargo.toml`:26 on 2022-10-13 13:28_

👍 

---

_Review comment by @konstin on `src/cache.rs`:1 on 2022-10-14 17:43_

done; i'd keep compiling cache.rs on wasm though because caching there should be possible (either in cwd like pytest or to some extent in local storage), it's just cacache that is inactive and hasn't completed tokio support (https://github.com/zkat/cacache-rs/pull/29)

---

_@konstin reviewed on 2022-10-14 17:43_

---

_@konstin reviewed on 2022-10-14 17:44_

---

_Review comment by @konstin on `Cargo.toml`:26 on 2022-10-14 17:44_

updated to crates.io release version

---

_Comment by @konstin on 2022-10-14 17:44_

The `charlie/wasm` looks good already, i'd definitely keep the whole frontend setup. i've also added cargo settings so that `cargo check --target wasm32-unknown-unknown` passes, but i haven't checked with wasm-pack.



---

_Merged by @charliermarsh on 2022-10-15 00:58_

---

_Closed by @charliermarsh on 2022-10-15 00:58_

---

_Branch deleted on 2022-10-15 08:45_

---
