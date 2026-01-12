```yaml
number: 11450
title: "Locate ruff executable in 'bin' directory as installed by 'pip install --target'."
type: pull_request
state: merged
author: jaraco
labels: []
assignees: []
merged: true
base: main
head: feature/honor-install-target-bin
created_at: 2024-05-16T15:32:55Z
updated_at: 2024-05-16T16:22:25Z
url: https://github.com/astral-sh/ruff/pull/11450
synced_at: 2026-01-12T15:55:38Z
```

# Locate ruff executable in 'bin' directory as installed by 'pip install --target'.

---

_@jaraco_

Fixes #11246

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This change adds an intermediate additional search path for `find_ruff_bin`.

I would have added this path as the last one, except that the last one is the one reported to the user, so I made this one second to last.

## Test Plan

It's shown to work with this command:

```
 ~ @ pip-run git+https://github.com/jaraco/ruff@feature/honor-install-target-bin -- -m ruff --version
ruff 0.4.4
```

I tried running the same command on Windows, which should work in theory, but building ruff from source on Windows is complicated. Even after installing Rust, ruff fails to build when `libmimalloc-sys` fails to build because `gcc` isn't installed (and the error message points to a [broken anchor](https://github.com/rust-lang/cc-rs#compile-time-requirements)). I was really hoping Rust would get us away from the Windows as second-class-citizen model :(.


---

_Comment by @jaraco on 2024-05-16 15:49_

Windows test failures are reported in https://github.com/astral-sh/ruff/issues/11439.

---

_Comment by @github-actions[bot] on 2024-05-16 15:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/77d6947479a7ba2cddc6b50d5600a941a84ca4d4/stdlib/typing.pyi#L326'>stdlib/typing.pyi:326:10:</a> PYI001 Name of private `TypeVar` must start with `_`
+ <a href='https://github.com/python/typeshed/blob/77d6947479a7ba2cddc6b50d5600a941a84ca4d4/stdlib/typing.pyi#L805'>stdlib/typing.pyi:805:1:</a> PYI042 Type alias `_get_type_hints_obj_allowed_types` should be CamelCase
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI001 | 1 | 1 | 0 | 0 | 0 |
| PYI042 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-05-16 16:03_

👍 Thanks. I moved it back to last and decided to use the `sysconfig.get_path("scripts")` path for the error.

---

_Merged by @charliermarsh on 2024-05-16 16:07_

---

_Closed by @charliermarsh on 2024-05-16 16:07_

---
