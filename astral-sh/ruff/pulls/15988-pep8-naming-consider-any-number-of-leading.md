```yaml
number: 15988
title: "[`pep8-naming`] Consider any number of leading underscore for `N801`"
type: pull_request
state: merged
author: VascoSch92
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-false-positive-N801
created_at: 2025-02-06T08:09:29Z
updated_at: 2025-02-06T08:38:28Z
url: https://github.com/astral-sh/ruff/pull/15988
synced_at: 2026-01-10T19:57:22Z
```

# [`pep8-naming`] Consider any number of leading underscore for `N801`

---

_Pull request opened by @VascoSch92 on 2025-02-06 08:09_

## Summary

The PR addresses the issue #15939 

Let me know if you think there are other test cases I should add ;-) 

---

_@dhruvmanila reviewed on 2025-02-06 08:13_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_class_name.rs`:60 on 2025-02-06 08:13_

We can possibly just use [`trim_start_matches`](https://doc.rust-lang.org/stable/std/primitive.str.html#method.trim_start_matches) here as it'll remove all `_` at the start.

---

_Label `bug` added by @dhruvmanila on 2025-02-06 08:13_

---

_Comment by @github-actions[bot] on 2025-02-06 08:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@VascoSch92 reviewed on 2025-02-06 08:17_

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_class_name.rs`:60 on 2025-02-06 08:17_

First of all, thanks. I didn't know about this functionality :-)

Second, if I have something like `class ___ClassName` with 3 times `_` : this should trigger `N801` or not?

In other words, how many `_` are allowed at the beginning of the name? I thought we could use just two `_` at the start. For that reason I was using two times the `strip_prefix`



---

_@MichaReiser reviewed on 2025-02-06 08:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_class_name.rs`:60 on 2025-02-06 08:23_

The upstream rule allows an arbitrary number of leading `_`. I don't see a reason why we should restrict the number of `_` a user uses. 

---

_@dhruvmanila reviewed on 2025-02-06 08:23_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_class_name.rs`:60 on 2025-02-06 08:23_

> Second, if I have something like `class ___ClassName` with 3 times `_` : this should trigger `N801` or not?

I don't think so. The underscores are just a convention while the check is mainly about the alphanumeric characters so this shouldn't trigger it. `pep8-naming` doesn't either.

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/pep8_naming/rules/invalid_class_name.rs`:60 on 2025-02-06 08:24_

Ah ok perfect... I thought that there It was a restriction on the number of _ you can use at the beginning of the class name ;-). I will change.

---

_@VascoSch92 reviewed on 2025-02-06 08:24_

---

_@dhruvmanila approved on 2025-02-06 08:37_

Thanks

---

_Renamed from "[pep8-naming] Fix false positive N801" to "[`pep8-naming`] Consider any number of leading underscore for `N801`" by @dhruvmanila on 2025-02-06 08:38_

---

_Merged by @dhruvmanila on 2025-02-06 08:38_

---

_Closed by @dhruvmanila on 2025-02-06 08:38_

---
