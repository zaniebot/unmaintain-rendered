```yaml
number: 15954
title: "[`pep8-naming`] Ignore `@override` methods (`N803`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: N803
created_at: 2025-02-05T00:12:06Z
updated_at: 2025-02-05T17:11:13Z
url: https://github.com/astral-sh/ruff/pull/15954
synced_at: 2026-01-10T19:57:22Z
```

# [`pep8-naming`] Ignore `@override` methods (`N803`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-05 00:12_

## Summary

Resolves #15925.

`N803` now checks for functions instead of parameters. In preview mode, if a method is decorated with `@override` and the current scope is that of a class, it will be ignored.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-02-05 00:13_

See also #15296, which proposes that `N803` and other `pep8-naming` rules should not apply for stubs.

---

_Comment by @github-actions[bot] on 2025-02-05 00:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-02-05 07:31_

Would you mind taking a look at the ecosystem check. They all seem wrong because there are no `@override` decorators involved. 

I'd also be fine to ship this without preview because I consider it a bug fix. But I'm also okay to keep it behind preview.

---

_Comment by @InSyncWithFoo on 2025-02-05 08:15_

@MichaReiser This is due to the rule no longer checking lambdas. Fixed.

---

_Label `bug` added by @MichaReiser on 2025-02-05 08:35_

---

_Label `rule` added by @MichaReiser on 2025-02-05 08:35_

---

_@MichaReiser approved on 2025-02-05 08:35_

Perfect, thank you

---

_Merged by @MichaReiser on 2025-02-05 08:35_

---

_Closed by @MichaReiser on 2025-02-05 08:35_

---

_Branch deleted on 2025-02-05 17:11_

---
