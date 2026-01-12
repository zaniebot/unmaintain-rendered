```yaml
number: 7650
title: Require opt-in to use alternative Python implementations
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/pypy-managed
created_at: 2024-09-23T23:40:10Z
updated_at: 2024-09-24T17:54:07Z
url: https://github.com/astral-sh/uv/pull/7650
synced_at: 2026-01-12T16:07:56Z
```

# Require opt-in to use alternative Python implementations

---

_@zanieb_

Closes #7118 

This only really affects managed interpreters, as we exclude alternative Python implementations from the search path during the `VersionRequest::executable_names` part of discovery. 

---

_Label `bug` added by @zanieb on 2024-09-23 23:40_

---

_@zanieb reviewed on 2024-09-23 23:40_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:918 on 2024-09-23 23:40_

I prefer the bool as written, it matches the style of the other ones.

---

_@zanieb reviewed on 2024-09-23 23:41_

---

_Review comment by @zanieb on `crates/uv-python/src/lib.rs`:1793 on 2024-09-23 23:41_

Note we don't have test coverage for this because we don't test Python discovery with managed interpreters. I'll create an issue to track infrastructure for this.

---

_Review requested from @charliermarsh by @zanieb on 2024-09-24 00:18_

---

_@charliermarsh reviewed on 2024-09-24 01:01_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:1409 on 2024-09-24 01:01_

Why is `Any` true but (e.g.) `Version` and `Default` false?

---

_@charliermarsh reviewed on 2024-09-24 01:02_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:949 on 2024-09-24 01:02_

Can we include a little more info on what "aren't allowed" means here, and what "opt-in" means for alternative implementations?

---

_@charliermarsh reviewed on 2024-09-24 01:02_

---

_Review comment by @charliermarsh on `crates/uv-python/src/implementation.rs`:65 on 2024-09-24 01:02_

(`value` or a longer name?)

---

_Review comment by @charliermarsh on `crates/uv-python/src/installation.rs`:180 on 2024-09-24 01:03_

Can we rephrase to make clear that this returns `false` for CPython, `true` for everything else? As written it sounds like it returns `true` when the installation is CPython or any other implementation.

---

_@charliermarsh reviewed on 2024-09-24 01:03_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:956 on 2024-09-24 01:04_

I think I need this part explained. This is, like, if the executable is named `python` but it's really PyPy?

---

_@charliermarsh reviewed on 2024-09-24 01:04_

---

_@zanieb reviewed on 2024-09-24 01:08_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1409 on 2024-09-24 01:08_

Per #7526 we want to distinguish between discovery that allows any interpreter and discovery of a good default interpreter. `Any` should never filter an interpreter. `Default` filters alternative implementations because they require opt-in. `Version` filters alternative implementations because asking for `3.13` isn't enough to select PyPy, we need `pypy@3.13` which is `ImplementationVersion`.

---

_@zanieb reviewed on 2024-09-24 01:11_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:956 on 2024-09-24 01:11_

Yes, if you have PyPy on the front of your PATH as `python` or `python3` that counts as opting in, to fulfill the principle that `$ python` and `$ uv python find --python-preference only-system` should generally use the same executable.

---

_@zanieb reviewed on 2024-09-24 01:12_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:956 on 2024-09-24 01:12_

I futzed with the commentary and implementation here a fair bit. I would kind of like to track the executable name on `PythonSource::SearchPath` and implement this logic in `PythonSource::allows_alternative_implementations()` but that would be fairly disruptive.

---

_@zanieb reviewed on 2024-09-24 01:13_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:949 on 2024-09-24 01:13_

Note this roughly matches the commentary above about pre-releases, but neither is explained in detail. I'll see what I can do.

---

_Review requested from @charliermarsh by @zanieb on 2024-09-24 16:45_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:943 on 2024-09-24 17:28_

Oh ok, this makes sense, comment is helpful.

---

_@charliermarsh reviewed on 2024-09-24 17:28_

---

_@charliermarsh reviewed on 2024-09-24 17:28_

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:953 on 2024-09-24 17:28_

Does this need to be under `if first_prerelease.is_none()`, so we don't set it repeatedly?

---

_@charliermarsh approved on 2024-09-24 17:28_

---

_Merged by @zanieb on 2024-09-24 17:52_

---

_Closed by @zanieb on 2024-09-24 17:52_

---

_Branch deleted on 2024-09-24 17:52_

---

_@zanieb reviewed on 2024-09-24 17:54_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:953 on 2024-09-24 17:54_

Yes! https://github.com/astral-sh/uv/pull/7666

---
