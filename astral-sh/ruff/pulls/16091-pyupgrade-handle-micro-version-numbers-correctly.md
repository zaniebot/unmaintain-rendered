```yaml
number: 16091
title: "[`pyupgrade`] Handle micro version numbers correctly (`UP036`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: UP036
created_at: 2025-02-11T01:46:07Z
updated_at: 2025-02-11T07:43:28Z
url: https://github.com/astral-sh/ruff/pull/16091
synced_at: 2026-01-10T19:57:22Z
```

# [`pyupgrade`] Handle micro version numbers correctly (`UP036`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-11 01:46_

## Summary

Resolves #16082.

`UP036` will now also take into consideration whether or not a micro version number is set:

* If a third element doesn't exist, the existing logic is preserved.
* If it exists but is not an integer literal, the check will not be reported.
* If it is an integer literal but doesn't fit into a `u8`, the check will be reported as invalid.
* Otherwise, the compared version is determined to always be less than the target version when:
	* The target's minor version is smaller than that of the comparator, or
	* The operator is `<`, the micro version is 0, and the two minor versions compare equal.

As this is considered a bugfix, it is not preview-gated.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-11 01:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/8a53592f3f832702cade085ad7f96e8a45d4524b/src/scikit_build_core/builder/sysconfig.py#L170'>src/scikit_build_core/builder/sysconfig.py:170:39:</a> RUF100 [*] Unused `noqa` directive (unused: `UP036`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/8a53592f3f832702cade085ad7f96e8a45d4524b/tests/test_pyproject_pep660.py#L100'>tests/test_pyproject_pep660.py:100:39:</a> RUF100 [*] Unused `noqa` directive (unused: `UP036`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/scikit-build/scikit-build-core">scikit-build/scikit-build-core</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/8a53592f3f832702cade085ad7f96e8a45d4524b/src/scikit_build_core/builder/sysconfig.py#L170'>src/scikit_build_core/builder/sysconfig.py:170:39:</a> RUF100 [*] Unused `noqa` directive (unused: `UP036`)
+ <a href='https://github.com/scikit-build/scikit-build-core/blob/8a53592f3f832702cade085ad7f96e8a45d4524b/tests/test_pyproject_pep660.py#L100'>tests/test_pyproject_pep660.py:100:39:</a> RUF100 [*] Unused `noqa` directive (unused: `UP036`)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF100 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @MichaReiser on 2025-02-11 07:28_

---

_Label `rule` added by @MichaReiser on 2025-02-11 07:28_

---

_@MichaReiser approved on 2025-02-11 07:36_

Thank you. It's great to see that this fixes two false positives

---

_Merged by @MichaReiser on 2025-02-11 07:40_

---

_Closed by @MichaReiser on 2025-02-11 07:40_

---
