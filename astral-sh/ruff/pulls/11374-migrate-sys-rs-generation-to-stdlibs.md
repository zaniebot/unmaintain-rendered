```yaml
number: 11374
title: Migrate sys.rs generation to stdlibs
type: pull_request
state: merged
author: rmodpur
labels:
  - isort
assignees: []
merged: true
base: main
head: use-stdlibs
created_at: 2024-05-12T06:18:09Z
updated_at: 2024-05-13T16:07:59Z
url: https://github.com/astral-sh/ruff/pull/11374
synced_at: 2026-01-10T22:05:26Z
```

# Migrate sys.rs generation to stdlibs

---

_Pull request opened by @rmodpur on 2024-05-12 06:18_

## Summary

Closes #11347 


---

_Comment by @github-actions[bot] on 2024-05-12 06:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/ced16da65f07b7079ffc3ab7d7dd8ae574102fef/test/test_from_sdist.py#L1'>test/test_from_sdist.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/pypa/cibuildwheel/blob/ced16da65f07b7079ffc3ab7d7dd8ae574102fef/test/test_pure_wheel.py#L1'>test/test_pure_wheel.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/pypa/cibuildwheel/blob/ced16da65f07b7079ffc3ab7d7dd8ae574102fef/test/test_same_wheel.py#L1'>test/test_same_wheel.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/ced16da65f07b7079ffc3ab7d7dd8ae574102fef/test/test_from_sdist.py#L1'>test/test_from_sdist.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/pypa/cibuildwheel/blob/ced16da65f07b7079ffc3ab7d7dd8ae574102fef/test/test_pure_wheel.py#L1'>test/test_pure_wheel.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
+ <a href='https://github.com/pypa/cibuildwheel/blob/ced16da65f07b7079ffc3ab7d7dd8ae574102fef/test/test_same_wheel.py#L1'>test/test_same_wheel.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-05-13 00:21_

---

_@charliermarsh approved on 2024-05-13 01:21_

---

_Comment by @charliermarsh on 2024-05-13 01:21_

Nice, thank you! The changes in `cibuildwheel` look right to me -- we were categorizing `test` as standard library before.

---

_Merged by @charliermarsh on 2024-05-13 01:21_

---

_Closed by @charliermarsh on 2024-05-13 01:21_

---

_Label `isort` added by @charliermarsh on 2024-05-13 01:21_

---

_@thatch reviewed on 2024-05-13 15:54_

---

_Review comment by @thatch on `scripts/generate_known_standard_library.py`:14 on 2024-05-13 15:54_

stdlibs also supports 3.13 already, btw: https://gist.github.com/thatch/0c7015ef2e0b6c457eb52a558f14beb1

---

_@thatch reviewed on 2024-05-13 15:56_

---

_Review comment by @thatch on `scripts/generate_known_standard_library.py`:37 on 2024-05-13 15:56_

These hardcoded ones should probably be removed and change to `modules = {}` -- we haven't had `sre` since 2.7.  I suspect these were added to work around bugs in the sphinx inventory?

---

_@thatch reviewed on 2024-05-13 15:57_

---

_Review comment by @thatch on `scripts/generate_known_standard_library.py`:41 on 2024-05-13 15:57_

I don't think `__main__` would ever be reported as stdlib.

---

_Comment by @thatch on 2024-05-13 15:59_

re `test` being stdlib, this was sometimes the case on py2 but we made the call that it was problematic to include.  See https://github.com/omnilib/stdlibs/pull/4

---

_Comment by @charliermarsh on 2024-05-13 16:00_

I think that's the right call @thatch.

---

_@charliermarsh reviewed on 2024-05-13 16:07_

---

_Review comment by @charliermarsh on `scripts/generate_known_standard_library.py`:41 on 2024-05-13 16:07_

https://github.com/astral-sh/ruff/pull/11409

---

_@charliermarsh reviewed on 2024-05-13 16:07_

---

_Review comment by @charliermarsh on `scripts/generate_known_standard_library.py`:37 on 2024-05-13 16:07_

https://github.com/astral-sh/ruff/pull/11409

---
