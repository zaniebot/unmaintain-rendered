```yaml
number: 18334
title: "[`pyupgrade`]: new rule UP050 (`useless-class-metaclass-type`)"
type: pull_request
state: merged
author: chirizxc
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: feat/useless-class-metaclass-type
created_at: 2025-05-27T16:09:44Z
updated_at: 2025-05-28T07:44:00Z
url: https://github.com/astral-sh/ruff/pull/18334
synced_at: 2026-01-12T15:56:17Z
```

# [`pyupgrade`]: new rule UP050 (`useless-class-metaclass-type`)

---

_@chirizxc_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
/closes #18320

Add `pyupgrade` rule `UP050` to flag metaclass=type usage, which is redundant in Python 3+. Includes safe auto-fix.
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
add snapshot

---

_Comment by @github-actions[bot] on 2025-05-27 16:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-05-27 17:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs`:56 on 2025-05-27 17:06_

I think we need to be careful here because older Python versions allow `type` as identifier in which case it isn't guaranteed that `type` points to the builtin `type` "type". 

Do you know if `type` is a reguler builtin or is it imported form typeshed?

---

_@MichaReiser reviewed on 2025-05-27 17:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs`:65 on 2025-05-27 17:06_

We shouldn't use `AlwaysFixable` in combination with `try_set_fix` because the try means that we sometimes fail to provide a fix

---

_@chirizxc reviewed on 2025-05-27 17:14_

---

_Review comment by @chirizxc on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs`:56 on 2025-05-27 17:14_

> I think we need to be careful here because older Python versions allow `type` as identifier in which case it isn't guaranteed that `type` points to the builtin `type` "type".
> 
> Do you know if `type` is a reguler builtin or is it imported form typeshed?

`type` is builtin

---

_@MichaReiser reviewed on 2025-05-27 17:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/useless_class_metaclass_type.rs`:56 on 2025-05-27 17:16_

We should then use `semantic.match_builtin_expr` to ensure we only run this code when `type` indeed is a builtin, see 

https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_linter/src/rules/flake8_bandit/rules/exec_used.rs#L36

---

_Label `rule` added by @MichaReiser on 2025-05-28 07:01_

---

_Label `preview` added by @MichaReiser on 2025-05-28 07:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:555 on 2025-05-28 07:02_

```suggestion
        (Pyupgrade, "050") => (RuleGroup::Preview, rules::pyupgrade::rules::UselessClassMetaclassType),
```

We add new rules in preview mode first, and stabilize them in a future minor release.

---

_@MichaReiser approved on 2025-05-28 07:03_

Nice

---

_Merged by @MichaReiser on 2025-05-28 07:22_

---

_Closed by @MichaReiser on 2025-05-28 07:22_

---

_Branch deleted on 2025-05-28 07:44_

---
