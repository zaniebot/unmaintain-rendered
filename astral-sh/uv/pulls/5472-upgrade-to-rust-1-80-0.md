```yaml
number: 5472
title: Upgrade to Rust 1.80.0
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/rust
created_at: 2024-07-26T01:49:23Z
updated_at: 2024-07-27T01:51:30Z
url: https://github.com/astral-sh/uv/pull/5472
synced_at: 2026-01-10T13:37:23Z
```

# Upgrade to Rust 1.80.0

---

_Pull request opened by @charliermarsh on 2024-07-26 01:49_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2024-07-26 01:49_

---

_@charliermarsh reviewed on 2024-07-26 01:49_

---

_Review comment by @charliermarsh on `crates/uv-python/src/interpreter.rs`:207 on 2024-07-26 01:49_

This isn't stable, unfortunately, so it's manually implemented below.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-26 01:49_

---

_@konstin approved on 2024-07-26 09:06_

---

_@BurntSushi approved on 2024-07-26 14:20_

---

_Comment by @samypr100 on 2024-07-27 01:01_

re: `CI / check windows trampoline | x86_64 (pull_request` seems to be missing this lockfile update

[crates/uv-trampoline/Cargo.lock](https://github.com/samypr100/uv/blob/c14269ce3d3333c3a7c38182e412a53c57ced04e/crates/uv-trampoline/Cargo.lock) from [#5473](https://github.com/astral-sh/uv/pull/5473/files#diff-cb549a4acbe420d0dbd1f671df5d641f757052cf9837e00dca86cabe788bc648)


---

_Comment by @codspeed-hq[bot] on 2024-07-27 01:45_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/rust)

### Merging #5472 will **degrade performances by 11.11%**

<sub>Comparing <code>charlie/rust</code> (4331aad) with <code>main</code> (3ea5e16)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/rust)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/rust` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_tag_compatibility[flyte-long-incompatible]` | 1.4 µs | 1.6 µs | -11.11% |


---

_Merged by @charliermarsh on 2024-07-27 01:49_

---

_Closed by @charliermarsh on 2024-07-27 01:49_

---

_Branch deleted on 2024-07-27 01:49_

---

_@samypr100 reviewed on 2024-07-27 01:51_

---

_Review comment by @samypr100 on `crates/uv-python/src/interpreter.rs`:207 on 2024-07-27 01:51_

Some day https://github.com/rust-lang/rust/issues/109737 🙏 

---
