```yaml
number: 10961
title: "[`pylint`] Implement `invalid-hash-returned` (`PLE0309`)"
type: pull_request
state: merged
author: tibor-reiss
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-E0309-invalid-hash-returned
created_at: 2024-04-15T20:34:36Z
updated_at: 2024-04-19T03:39:09Z
url: https://github.com/astral-sh/ruff/pull/10961
synced_at: 2026-01-10T22:37:01Z
```

# [`pylint`] Implement `invalid-hash-returned` (`PLE0309`)

---

_Pull request opened by @tibor-reiss on 2024-04-15 20:34_

Add pylint rule invalid-hash-returned (PLE0309)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test Plan: `cargo test`

TBD: from the description: "Strictly speaking `bool` is a subclass of `int`, thus returning `True`/`False` is valid. To be consistent with other rules (e.g. [PLE0305](https://github.com/astral-sh/ruff/pull/10962) invalid-index-returned), ruff will raise, compared to pylint which will not raise."


---

_Comment by @codspeed-hq[bot] on 2024-04-15 21:46_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tibor-reiss:add-E0309-invalid-hash-returned)

### Merging #10961 will **not alter performance**

<sub>Comparing <code>tibor-reiss:add-E0309-invalid-hash-returned</code> (b5d22ac) with <code>main</code> (5d3c9f2)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-04-15 21:54_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-19 03:26_

Thanks!

---

_Renamed from "[pylint] Implement invalid-hash-returned (PLE0309)" to "[`pylint`] Implement `invalid-hash-returned` (`PLE0309`)" by @charliermarsh on 2024-04-19 03:26_

---

_Label `rule` added by @charliermarsh on 2024-04-19 03:26_

---

_Label `preview` added by @charliermarsh on 2024-04-19 03:26_

---

_Merged by @charliermarsh on 2024-04-19 03:33_

---

_Closed by @charliermarsh on 2024-04-19 03:33_

---
