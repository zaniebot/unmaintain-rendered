```yaml
number: 5845
title: "[`pyflakes`] make F401 autofix \"suggested\" in `__init__.py` files"
type: pull_request
state: closed
author: edgarrmondragon
labels: []
assignees: []
base: main
head: F401-init-only-suggested
created_at: 2023-07-17T22:52:42Z
updated_at: 2024-03-20T01:20:09Z
url: https://github.com/astral-sh/ruff/pull/5845
synced_at: 2026-01-12T15:55:19Z
```

# [`pyflakes`] make F401 autofix "suggested" in `__init__.py` files

---

_@edgarrmondragon_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Make the F401 (`unused-import`) autofix "suggested", instead of "automatic".

Related:

* Closes https://github.com/astral-sh/ruff/issues/5697.

## Test Plan

- [x] Enable the `unused-import` rule in the existing pyflakes test for `__init__.py` files and validate the snapshot


---

_Review comment by @zanieb on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__init.snap`:11 on 2023-07-17 23:01_

It seems like we should suggest `import os as os` instead

---

_@zanieb reviewed on 2023-07-17 23:01_

Thanks for contributing! I just have one comment :)

---

_Assigned to @zanieb by @zanieb on 2023-07-17 23:01_

---

_Comment by @github-actions[bot] on 2023-07-17 23:11_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+2, -2, 0 error(s))

<details><summary>setuptools (+2, -2)</summary>
<p>

<pre>
- <a href='https://github.com/pypa/setuptools/blob/f7532b24c49b8285496734b544ea12a6fb6fdc52/pkg_resources/_vendor/jaraco/text/__init__.py#L9'>pkg_resources/_vendor/jaraco/text/__init__.py:9:58:</a> F401 [*] `pkg_resources.extern.importlib_resources.files` imported but unused
+ <a href='https://github.com/pypa/setuptools/blob/f7532b24c49b8285496734b544ea12a6fb6fdc52/pkg_resources/_vendor/jaraco/text/__init__.py#L9'>pkg_resources/_vendor/jaraco/text/__init__.py:9:58:</a> F401 [*] `pkg_resources.extern.importlib_resources.files` imported but unused; consider adding to `__all__` or using a redundant alias
- <a href='https://github.com/pypa/setuptools/blob/f7532b24c49b8285496734b544ea12a6fb6fdc52/setuptools/_vendor/jaraco/text/__init__.py#L9'>setuptools/_vendor/jaraco/text/__init__.py:9:55:</a> F401 [*] `setuptools.extern.importlib_resources.files` imported but unused
+ <a href='https://github.com/pypa/setuptools/blob/f7532b24c49b8285496734b544ea12a6fb6fdc52/setuptools/_vendor/jaraco/text/__init__.py#L9'>setuptools/_vendor/jaraco/text/__init__.py:9:55:</a> F401 [*] `setuptools.extern.importlib_resources.files` imported but unused; consider adding to `__all__` or using a redundant alias
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| F401 | 4 | 2 | 2 |
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-07-17 23:14_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__init.snap`:11 on 2023-07-17 23:14_

We do include this in the message ("or using a redundant alias"), unsure what the right suggested fix is...

---

_@zanieb reviewed on 2023-07-17 23:16_

---

_Review comment by @zanieb on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__init.snap`:11 on 2023-07-17 23:16_

We don't mention removing it in the message though — perhaps if we say "remove it or consider adding..."

---

_Review comment by @edgarrmondragon on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__init.snap`:11 on 2023-07-17 23:20_

Ok, so maybe the suggested fix should be

```diff
- import os
+ import os as os
```
?

---

_@edgarrmondragon reviewed on 2023-07-17 23:20_

---

_@charliermarsh reviewed on 2023-11-03 03:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/snapshots/ruff__rules__pyflakes__tests__init.snap`:11 on 2023-11-03 03:50_

(Sorry to come back after so long) It's tough because we really want to offer two different fixes here. I guess we should leave the fix as-is, but refine the diagnostic message.

---

_Comment by @charliermarsh on 2023-11-03 03:50_

I'm still up for removing this!

---

_Comment by @zanieb on 2024-03-20 01:12_

Implemented in https://github.com/astral-sh/ruff/pull/10365

Thanks for contributing this was a helpful reference!

---

_Closed by @zanieb on 2024-03-20 01:12_

---

_Branch deleted on 2024-03-20 01:20_

---
