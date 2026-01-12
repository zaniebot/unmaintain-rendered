```yaml
number: 909
title: Adjust markers to match target Python version
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/markers
created_at: 2024-01-13T23:14:30Z
updated_at: 2024-01-14T15:39:16Z
url: https://github.com/astral-sh/uv/pull/909
synced_at: 2026-01-12T16:04:17Z
```

# Adjust markers to match target Python version

---

_@charliermarsh_

## Summary

This PR ensures that when the user passes in `--python-version`, we adjust the _markers_ to match the target version, thus forcing us to select compatible wheels for the `--python-version`, rather than the installed version.

## Context

Let's call Python 3.10 the "installed" environment and Python 3.12 the "target" environment. For each version, we have _both_ a Python version (to match against `Requires-Python`) and a set of tags (to match against wheels).

The rules for resolution are as follows...

- For each package, for each version, we try to find the "best candidate" for resolution and installation.
- We first look for a wheel that's compatible with the _target_ environment. This requires testing against both the `Requires-Python` and the markers. (We won't have to build or run this code, so the _installed_ version is irrelevant.) **(This PR corrects _this_ bullet -- previously, we validated against the _installed_ markers, rather than the target markers.)**
- If we can't find a compatible wheel, we accept any _incompatible_ wheel as long as there's a source distribution. The source distribution _must_ be compatible with the target environment. (We won't have to build or run this code, so the _installed_ version is irrelevant.)
- If there are no wheels, then the source distribution must be compatible with _both_ the installed and target environments, since we need to build it.

This is all true for the top-level resolution. When we perform a sub-resolution (when resolving the build dependencies of a source distribution), we should _only_ use the installed environment, and ignore the target environment, since we assume that the dependencies will be the same in both environments once built -- so our goal is "just" to build the distribution, without concern for which build dependencies it uses.

Closes https://github.com/astral-sh/puffin/issues/883.

---

_Review requested from @zanieb by @charliermarsh on 2024-01-13 23:14_

---

_Review requested from @konstin by @charliermarsh on 2024-01-13 23:14_

---

_@charliermarsh reviewed on 2024-01-13 23:15_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/candidate_selector.rs`:287 on 2024-01-13 23:15_

This isn't strictly necessary, since if `self.resolve()` is a source distribution, it will be the same file as `self.install()` which is validated above. But, that's kind of a detail that this method shouldn't need to know about, so I just added this case to make it clearer.

---

_Label `bug` added by @charliermarsh on 2024-01-13 23:16_

---

_@charliermarsh reviewed on 2024-01-13 23:17_

---

_Review comment by @charliermarsh on `crates/puffin-cli/src/commands/pip_compile.rs`:139 on 2024-01-13 23:17_

FYI: we pass `interpreter` to `BuildDispatch` which is what's used for all source distribution builds, and `BuildDispatch` then grabs the markers and tags from `interpreter` -- so it's guaranteed to only use the _installed_ environment when resolving, which is what we want.

---

_Review comment by @konstin on `crates/puffin-interpreter/src/interpreter.rs`:179 on 2024-01-14 09:52_

```suggestion
        u8::try_from(minor).expect("invalid minor version")
```

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_version.rs`:93 on 2024-01-14 09:53_

```suggestion
        u8::try_from(self.0.release()[1]).expect("invalid minor version")
```

---

_Review comment by @konstin on `crates/puffin-resolver/src/candidate_selector.rs`:287 on 2024-01-14 10:00_

Could you add that as a code comment? Otherwise i'll be really confused coming back to that code in a couple of weeks

---

_@konstin approved on 2024-01-14 10:00_

---

_Comment by @konstin on 2024-01-14 10:00_

> If there are no wheels, then the source distribution must be compatible with both the installed and target environments, since we need to build it.

Does the user get any indication in this case that their resolution to target failed because they have to switch their installed version?

---

_Comment by @charliermarsh on 2024-01-14 15:33_

> Does the user get any indication in this case that their resolution to target failed because they have to switch their installed version?

Yes, because we add the installed version as a dependency, but we should add a packse test for it.

---

_Merged by @charliermarsh on 2024-01-14 15:39_

---

_Closed by @charliermarsh on 2024-01-14 15:39_

---

_Branch deleted on 2024-01-14 15:39_

---
