```yaml
number: 1389
title: Non-deterministic overload matching failure
type: issue
state: open
author: mtshiba
labels:
  - bug
assignees: []
created_at: 2025-10-17T15:10:36Z
updated_at: 2025-11-21T21:15:32Z
url: https://github.com/astral-sh/ty/issues/1389
synced_at: 2026-01-10T01:58:59Z
```

# Non-deterministic overload matching failure

---

_Issue opened by @mtshiba on 2025-10-17 15:10_

### Summary

An error has been observed randomly appearing and disappearing in the mypy_primer report.
This has been seen in many PRs (e.g. https://github.com/astral-sh/ruff/pull/20871).

```
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
```

https://github.com/scikit-build/scikit-build-core/blob/3f371fff3d0dd424811ef73f58d88389263d79db/src/scikit_build_core/build/_wheelfile.py#L51

Unfortunately, this is not easy to reproduce locally, and the only known way is to run mypy_primer.
So I haven't been able to identify the cause of this error.

Based on my current investigation, this error appears to have become apparent from https://github.com/astral-sh/ruff/pull/20476. However, I believe that the PR only exposed the error, and the seed of nondeterminism was likely planted before that.

Possibly related: https://github.com/astral-sh/ty/issues/369

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-10-17 15:59_

---

_Comment by @MichaReiser on 2025-10-17 17:00_

Does the overload matching iterate over any hash maps?

---

_Comment by @mtshiba on 2025-10-19 06:44_

`TypeInferenceBuilder::check_overloaded_functions` seems suspicious. It uses `TypeInferenceBuilder::called_functions: FxHashSet`, which is used as an iterator.

---

_Closed by @MichaReiser on 2025-10-19 10:13_

---

_Reopened by @MichaReiser on 2025-11-21 17:27_

---

_Comment by @MichaReiser on 2025-11-21 17:28_

This is back. The message is slightly different but also on `scikit-build-core`

Example runs:

* https://github.com/astral-sh/ruff/pull/21549#issuecomment-3560212530
* https://github.com/astral-sh/ruff/pull/21560#issuecomment-3563186797


---

_Added to milestone `Stable` by @carljm on 2025-11-21 21:15_

---
