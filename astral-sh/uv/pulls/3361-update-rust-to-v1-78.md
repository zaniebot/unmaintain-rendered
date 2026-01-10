```yaml
number: 3361
title: Update Rust to v1.78
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: rust-update-178
created_at: 2024-05-03T19:44:05Z
updated_at: 2024-05-03T20:08:24Z
url: https://github.com/astral-sh/uv/pull/3361
synced_at: 2026-01-10T14:37:54Z
```

# Update Rust to v1.78

---

_Pull request opened by @samypr100 on 2024-05-03 19:44_

## Summary

Updates rust to 1.78 in `rust-toolchain.toml`

See: https://blog.rust-lang.org/2024/05/02/Rust-1.78.0.html

### Potential blockers

* homebre still on 1.77 - https://github.com/Homebrew/homebrew-core/pull/170649
* conda-forge still on 1.77 - https://anaconda.org/conda-forge/rust


---

_Comment by @charliermarsh on 2024-05-03 19:46_

I think it's ok for us to upgrade `rust-toolchain.toml` but not the minimum Rust version in `Cargo.toml` (see https://github.com/astral-sh/ruff/pull/11260).

---

_Comment by @charliermarsh on 2024-05-03 19:46_

Which would let us upgrade without having to wait for Homebrew and Conda, IIRC.

---

_Comment by @codspeed-hq[bot] on 2024-05-03 19:54_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100:rust-update-178)

### Merging #3361 will **improve performances by 5.78%**

<sub>Comparing <code>samypr100:rust-update-178</code> (5030cea) with <code>main</code> (1089abd)</sub>



### Summary

`⚡ 1` improvements
`✅ 11` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `samypr100:rust-update-178` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_jupyter` | 376.3 ms | 355.7 ms | +5.78% |


---

_@charliermarsh approved on 2024-05-03 19:54_

Thank you!

---

_Label `internal` added by @charliermarsh on 2024-05-03 19:54_

---

_Marked ready for review by @samypr100 on 2024-05-03 19:55_

---

_Merged by @charliermarsh on 2024-05-03 20:07_

---

_Closed by @charliermarsh on 2024-05-03 20:07_

---

_Branch deleted on 2024-05-03 20:08_

---
