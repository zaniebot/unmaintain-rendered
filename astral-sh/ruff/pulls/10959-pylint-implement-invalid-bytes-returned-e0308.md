```yaml
number: 10959
title: "[`pylint`] Implement `invalid-bytes-returned` (`E0308`)"
type: pull_request
state: merged
author: tibor-reiss
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: add-E0308-invalid-bytes-returned
created_at: 2024-04-15T19:09:36Z
updated_at: 2024-04-18T01:43:15Z
url: https://github.com/astral-sh/ruff/pull/10959
synced_at: 2026-01-12T15:55:33Z
```

# [`pylint`] Implement `invalid-bytes-returned` (`E0308`)

---

_@tibor-reiss_

Add pylint rule invalid-bytes-returned (PLE0308)

See https://github.com/astral-sh/ruff/issues/970 for rules

Test Plan: `cargo test`

---

_Comment by @codspeed-hq[bot] on 2024-04-15 21:44_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tibor-reiss:add-E0308-invalid-bytes-returned)

### Merging #10959 will **not alter performance**

<sub>Comparing <code>tibor-reiss:add-E0308-invalid-bytes-returned</code> (ced76b9) with <code>main</code> (06b3e37)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-04-15 21:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/invalid_bytes_return.rs`:73 on 2024-04-16 06:51_

Should this rule and the other invalid-*-returned rules also flag the case where the method has no return statement

```
class Test:
	def __bytes__(self):
		print("nope")
```

---

_@MichaReiser reviewed on 2024-04-16 06:51_

---

_@MichaReiser approved on 2024-04-16 06:52_

---

_@tibor-reiss reviewed on 2024-04-16 18:03_

---

_Review comment by @tibor-reiss on `crates/ruff_linter/src/rules/pylint/rules/invalid_bytes_return.rs`:73 on 2024-04-16 18:03_

I added this case as well. If this becomes the consensus, I can also add it to the already included bool (E0304) and str (E0307) cases.

---

_@tibor-reiss reviewed on 2024-04-17 18:00_

---

_Review comment by @tibor-reiss on `crates/ruff_linter/src/rules/pylint/rules/invalid_bytes_return.rs`:73 on 2024-04-17 18:00_

@MichaReiser As it turned out, the no-return case is not that simple: if there is just a pass/ellipsis/raise statement, then this rule should not raise. However, if there is e.g. a print statement and a raise statement, then it should raise - see the tests for examples.

---

_@charliermarsh approved on 2024-04-18 01:25_

---

_@charliermarsh reviewed on 2024-04-18 01:25_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/rules/invalid_bytes_return.rs`:73 on 2024-04-18 01:25_

I changed this to use `is_stub` for detecting the other cases.

---

_Renamed from "[pylint] Implement invalid-bytes-returned (PLE0308)" to "[`pylint`] Implement `invalid-bytes-returned` (`E0308`)" by @charliermarsh on 2024-04-18 01:25_

---

_Label `rule` added by @charliermarsh on 2024-04-18 01:25_

---

_Label `preview` added by @charliermarsh on 2024-04-18 01:25_

---

_Merged by @charliermarsh on 2024-04-18 01:38_

---

_Closed by @charliermarsh on 2024-04-18 01:38_

---
