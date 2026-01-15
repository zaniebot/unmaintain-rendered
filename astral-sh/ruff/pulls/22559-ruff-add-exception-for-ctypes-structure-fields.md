```yaml
number: 22559
title: "[`ruff`] Add exception for `ctypes.Structure._fields_` (`RUF012`)"
type: pull_request
state: merged
author: caiquejjx
labels:
  - rule
assignees: []
merged: true
base: main
head: fix-22166
created_at: 2026-01-13T20:59:00Z
updated_at: 2026-01-15T20:50:01Z
url: https://github.com/astral-sh/ruff/pull/22559
synced_at: 2026-01-15T21:12:57Z
```

# [`ruff`] Add exception for `ctypes.Structure._fields_` (`RUF012`)

---

_@caiquejjx_

Closes #22166 
## Summary
Adds an exception for `ctypes.Structure._fields_` to the rule RUF012 as it has it's own way of enforcing immutability:

> The fields class variable can only be set once. Later assignments will raise an [AttributeError](https://docs.python.org/3/library/exceptions.html#AttributeError).



---

_Label `rule` added by @amyreese on 2026-01-13 22:34_

---

_Comment by @amyreese on 2026-01-13 22:35_

Can you add a test case to the snapshots demonstrating the expected passing case?

---

_Comment by @caiquejjx on 2026-01-13 23:10_

@amyreese Done, thanks

---

_Comment by @astral-sh-bot[bot] on 2026-01-14 09:00_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/mutable_class_default.rs`:143 on 2026-01-15 18:57_

```suggestion
                    // The `_fields_` property of a `ctypes.Structure` base class has its
```

---

_@ntBre approved on 2026-01-15 19:48_

Thank you! This looks great.

I'm just waiting on the release today to finish to merge this.

---

_Renamed from "[RUF012] Add exception for `ctypes.Structure._fields_`" to "[`ruff`] Add exception for `ctypes.Structure._fields_` (`RUF012`)" by @ntBre on 2026-01-15 19:49_

---

_Merged by @ntBre on 2026-01-15 20:50_

---

_Closed by @ntBre on 2026-01-15 20:50_

---
