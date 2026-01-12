```yaml
number: 16451
title: "[`pyupgrade`] Do not offer fix when at least one target is `global`/`nonlocal` (`UP028`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
assignees: []
merged: true
base: main
head: UP028
created_at: 2025-03-01T16:33:57Z
updated_at: 2025-03-04T10:28:02Z
url: https://github.com/astral-sh/ruff/pull/16451
synced_at: 2026-01-12T15:55:55Z
```

# [`pyupgrade`] Do not offer fix when at least one target is `global`/`nonlocal` (`UP028`)

---

_@InSyncWithFoo_

## Summary

Resolves #16445.

`UP028` is now no longer always fixable: it will not offer a fix when at least one `ExprName` target is bound to either a `global` or a `nonlocal` declaration.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-03-01 16:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-03-03 09:55_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP028_1.py`:129 on 2025-03-03 09:58_

What's the motivation for moving this test?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:146 on 2025-03-03 09:59_

Does it make sense to even raise the diagnostic in the case of a non-local or global? How would a user rewrite the code in that case to preserve the existing semantics?

---

_@MichaReiser reviewed on 2025-03-03 10:00_

---

_@InSyncWithFoo reviewed on 2025-03-03 12:43_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/pyupgrade/UP028_1.py`:129 on 2025-03-03 12:43_

I added it there in #15543, but it should have been in the other file, since this one is for "no error" cases.

---

_@InSyncWithFoo reviewed on 2025-03-03 12:44_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:146 on 2025-03-03 12:44_

I would write something like this:

```python
global some_global

for element in iterable:
	some_global = element
	yield some_global
```

The assignment makes it clear that this loop is not just reyielding: it has side effects.

---

_@MichaReiser reviewed on 2025-03-03 12:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/yield_in_for_loop.rs`:146 on 2025-03-03 12:48_

That makes sense. Thank you. And this should then not raise the rule. Should we add this as an example to the `no_errors` test or is it already covered by other examples?

---

_@MichaReiser approved on 2025-03-04 10:27_

---

_Merged by @MichaReiser on 2025-03-04 10:28_

---

_Closed by @MichaReiser on 2025-03-04 10:28_

---
