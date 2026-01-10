```yaml
number: 745
title: Model Python version as a PubGrub package
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/py-version
created_at: 2024-01-02T22:13:35Z
updated_at: 2024-01-03T15:20:46Z
url: https://github.com/astral-sh/uv/pull/745
synced_at: 2026-01-10T15:44:44Z
```

# Model Python version as a PubGrub package

---

_Pull request opened by @charliermarsh on 2024-01-02 22:13_

## Summary

This PR modifies the resolver to treat the Python version as a package, which allows for better error messages (since we no longer treat incompatible packages as if they "don't exist at all").

There are a few tricky pieces here...

First, we need to track both the interpreter's Python version and the _target_ Python version, because we support resolving for other versions via `--python 3.7`.

Second, we allow using incompatible wheels during resolution, as long as there's a compatible source distribution. So we still need to test for `requires-python` compatibility when selecting distributions.

This could use more testing, but it feels like an area where `packse` would be more productive than writing PyPI tests.

Closes https://github.com/astral-sh/puffin/issues/406.


---

_Review requested from @zanieb by @charliermarsh on 2024-01-02 22:13_

---

_Review requested from @konstin by @charliermarsh on 2024-01-02 22:13_

---

_@charliermarsh reviewed on 2024-01-02 22:20_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:673 on 2024-01-02 22:20_

I am confused by `Python*` here 🤔 

---

_@charliermarsh reviewed on 2024-01-02 22:29_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:673 on 2024-01-02 22:29_

I guess that's the simplified set, based on the `simplify` docs? "If all the versions are contained in the original than the range will be simplified to full."?

I should probably avoid simplifying Python versions then.

---

_Converted to draft by @charliermarsh on 2024-01-02 22:37_

---

_Comment by @charliermarsh on 2024-01-02 22:44_

This is actually harder than I thought. Consider the case of resolving NumPy with `--python-version 3.8`. In that case, we want to skip building all the source distributions for versions later than `1.24.4`. But we want to return them to PubGrub so that they're modeled as incompatibilities and not ignored entirely. So we need some new abstraction around `Candidate` to indicate whether it's compatible or incompatible.

---

_Comment by @charliermarsh on 2024-01-02 23:20_

I think I know how to solve but will be a bit more work, so marking as draft.

---

_Marked ready for review by @charliermarsh on 2024-01-03 01:38_

---

_Comment by @charliermarsh on 2024-01-03 01:38_

Okay, should be ready for review.

---

_Review comment by @konstin on `crates/puffin-resolver/src/finder.rs`:173 on 2024-01-03 10:54_

Shouldn't we explicitly allow the source dist here only to then reject it (with some shortcut before building, e.g. fake metadata that only depends on the python version we know we'll reject) through having the wrong python version requirement? Otherwise this will not be represented in the pubgrub errors.

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:504 on 2024-01-03 11:00_

Shouldn't this only be the target version?

---

_@konstin approved on 2024-01-03 11:05_

---

_@charliermarsh reviewed on 2024-01-03 14:41_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/finder.rs`:173 on 2024-01-03 14:41_

The `DistFinder` is not used in PubGrub -- this assumes that dependencies are already locked.

---

_@charliermarsh reviewed on 2024-01-03 14:43_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:504 on 2024-01-03 14:43_

What if this is a source distribution that we'll then need to build?

---

_Merged by @charliermarsh on 2024-01-03 15:20_

---

_Closed by @charliermarsh on 2024-01-03 15:20_

---

_Branch deleted on 2024-01-03 15:20_

---
