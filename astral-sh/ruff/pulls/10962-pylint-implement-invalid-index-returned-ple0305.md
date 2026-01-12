```yaml
number: 10962
title: "[`pylint`] Implement `invalid-index-returned` (`PLE0305`)"
type: pull_request
state: merged
author: tibor-reiss
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-E0305-invalid-index-returned
created_at: 2024-04-15T20:37:00Z
updated_at: 2024-04-19T03:50:39Z
url: https://github.com/astral-sh/ruff/pull/10962
synced_at: 2026-01-12T15:55:33Z
```

# [`pylint`] Implement `invalid-index-returned` (`PLE0305`)

---

_@tibor-reiss_

Add pylint rule invalid-index-returned (PLE0305)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test Plan: `cargo test`

TBD: from the description: "Note: Strictly speaking `bool` is a subclass of `int`, thus returning `True`/`False` is valid. However, a DeprecationWarning (`DeprecationWarning: __index__ returned non-int (type bool)`) for such cases was already introduced, thus this is a conscious difference between the original pylint rule and the current ruff implementation."

---

_Comment by @codspeed-hq[bot] on 2024-04-15 21:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tibor-reiss:add-E0305-invalid-index-returned)

### Merging #10962 will **not alter performance**

<sub>Comparing <code>tibor-reiss:add-E0305-invalid-index-returned</code> (ad9d443) with <code>main</code> (97acf1d)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-04-15 21:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-04-19 03:38_

Thanks!

---

_Label `rule` added by @charliermarsh on 2024-04-19 03:38_

---

_Label `preview` added by @charliermarsh on 2024-04-19 03:38_

---

_Renamed from "[pylint] Implement invalid-index-returned (PLE0305)" to "[`pylint`] Implement `invalid-index-returned` (`PLE0305`)" by @charliermarsh on 2024-04-19 03:38_

---

_Merged by @charliermarsh on 2024-04-19 03:44_

---

_Closed by @charliermarsh on 2024-04-19 03:44_

---
