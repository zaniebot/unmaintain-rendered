```yaml
number: 21864
title: "[ty] Add mypy primer check comparing same revisions"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/mypy-primer-same-revision
created_at: 2025-12-09T12:27:05Z
updated_at: 2025-12-10T16:37:18Z
url: https://github.com/astral-sh/ruff/pull/21864
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Add mypy primer check comparing same revisions

---

_Pull request opened by @MichaReiser on 2025-12-09 12:27_

## Summary

This PR adds a new CI job that runs mypy primer and compares the output between two runs using the same ty version to detect non-deterministic output. 

It's almost impossible to detect non-deterministic output in large ecosystem reports because the results in isolation look okay. That's why an automated way to find non-determinism in CI will help us prevent regressions like the ones we see now after we changed the cycle recovery function for better convergence.

We won't be able to immediately enable this check because ty's output is currently non-deterministic, but adding this CI job now should make it easy to enable this check in whichever PR claims to fix all the nondeterminism ;)

---

_Label `testing` added by @MichaReiser on 2025-12-09 12:27_

---

_Label `ty` added by @MichaReiser on 2025-12-09 12:27_

---

_Label `testing` added by @MichaReiser on 2025-12-09 12:27_

---

_Label `ty` added by @MichaReiser on 2025-12-09 12:27_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 12:30_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 491 diagnostics
+ Found 489 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5259 diagnostics
+ Found 5258 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-09 12:39_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @MichaReiser on 2025-12-09 13:25_

---

_@sharkdp approved on 2025-12-09 18:51_

Thanks!

---

_Merged by @MichaReiser on 2025-12-10 16:37_

---

_Closed by @MichaReiser on 2025-12-10 16:37_

---

_Branch deleted on 2025-12-10 16:37_

---
