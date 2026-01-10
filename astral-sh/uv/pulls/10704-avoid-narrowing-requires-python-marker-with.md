```yaml
number: 10704
title: "Avoid narrowing `requires-python` marker with disjunctions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/pypy-req-python
created_at: 2025-01-17T04:33:05Z
updated_at: 2025-01-17T16:25:34Z
url: https://github.com/astral-sh/uv/pull/10704
synced_at: 2026-01-10T11:45:05Z
```

# Avoid narrowing `requires-python` marker with disjunctions

---

_Pull request opened by @charliermarsh on 2025-01-17 04:33_

## Summary

A bug in `requires_python` (which infers the Python requirement from a marker) was leading us to break an invariant around the relationship between the marker environment and the Python requirement. This, in turn, was leading us to drop parts of the environment space when solving.

Specifically, in the linked example, we generated a fork for `python_full_version < '3.10' or platform_python_implementation != 'CPython'`, which was later split into `python_full_version == '3.8.*'` and `python_full_version == '3.9.*'`, losing the `platform_python_implementation != 'CPython'` portion.

Closes https://github.com/astral-sh/uv/issues/10669.


---

_Label `bug` added by @charliermarsh on 2025-01-17 04:33_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-17 04:34_

---

_Review requested from @konstin by @charliermarsh on 2025-01-17 04:34_

---

_Review requested from @ibraheemdev by @charliermarsh on 2025-01-17 04:34_

---

_Marked ready for review by @charliermarsh on 2025-01-17 04:34_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/marker.rs`:16 on 2025-01-17 05:17_

The search here only cares about a single type of node (`CanonicalMarkerValueVersion::PythonFullVersion`), and importantly that node can only show up once on a given solution path in the decision diagram. I believe this means that you shouldn't need an inner `Vec` here, and you can default to `Ranges::full()` for a solution that does not have an explicit python version requirement. That should simplify this a little bit — there is no list of versions to take the intersection of for a given solution.

---

_@ibraheemdev approved on 2025-01-17 05:17_

Other than the potential simplification I pointed out this looks good to me! A little surprised that we didn't notice this bug sooner.

---

_@konstin approved on 2025-01-17 09:27_

---

_@BurntSushi approved on 2025-01-17 12:34_

---

_Merged by @charliermarsh on 2025-01-17 16:25_

---

_Closed by @charliermarsh on 2025-01-17 16:25_

---

_Branch deleted on 2025-01-17 16:25_

---
