```yaml
number: 21400
title: "[ty] Reorganize walltime benchmarks"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/reorganize-benchmarks
created_at: 2025-11-12T10:59:44Z
updated_at: 2025-11-12T11:41:41Z
url: https://github.com/astral-sh/ruff/pull/21400
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Reorganize walltime benchmarks

---

_@sharkdp_

## Summary

I would like to merge this before https://github.com/astral-sh/ruff/pull/21363 to establish a new baseline.

* Move `pydantic` to the "large" section, as ty will soon be much slower when type-checking that project
* Use `altair` instead of `pydantic` for the multithreaded benchmark

## Test Plan

Both benchmark shards still take roughly the same amount of time (10% difference):

`medium|multithreaded`:

<img width="411" height="74" alt="image" src="https://github.com/user-attachments/assets/4c191c44-5e68-4765-a851-6f06ac8d0c93" />


`small|large`:

<img width="305" height="62" alt="image" src="https://github.com/user-attachments/assets/57a99b7c-0c98-4420-a097-4e0dc04f4d2b" />


---

_Label `internal` added by @sharkdp on 2025-11-12 10:59_

---

_Label `ty` added by @sharkdp on 2025-11-12 10:59_

---

_Renamed from "[ty] Walltime benchmarks: move pydantic to large, use altair for mult…" to "[ty] Reorganize walltime benchmarks" by @sharkdp on 2025-11-12 11:14_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 11:14_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Marked ready for review by @sharkdp on 2025-11-12 11:17_

---

_Review requested from @MichaReiser by @sharkdp on 2025-11-12 11:17_

---

_@MichaReiser approved on 2025-11-12 11:33_

---

_Merged by @MichaReiser on 2025-11-12 11:41_

---

_Closed by @MichaReiser on 2025-11-12 11:41_

---

_Branch deleted on 2025-11-12 11:41_

---

_Label `internal` removed by @MichaReiser on 2025-11-12 11:41_

---

_Label `ci` added by @MichaReiser on 2025-11-12 11:41_

---
