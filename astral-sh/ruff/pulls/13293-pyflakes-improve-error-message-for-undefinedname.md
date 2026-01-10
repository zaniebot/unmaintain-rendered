```yaml
number: 13293
title: "[`pyflakes`] Improve error message for `UndefinedName` when a builtin was added in a newer version than specified in Ruff config (`F821`)"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - linter
assignees: []
merged: true
base: main
head: alex/f821-error-msg
created_at: 2024-09-09T16:11:18Z
updated_at: 2024-09-10T18:13:10Z
url: https://github.com/astral-sh/ruff/pull/13293
synced_at: 2026-01-10T21:38:32Z
```

# [`pyflakes`] Improve error message for `UndefinedName` when a builtin was added in a newer version than specified in Ruff config (`F821`)

---

_Pull request opened by @AlexWaygood on 2024-09-09 16:11_

## Summary

Fixes #13287.

This adds a new function to `ruff_python_stdlib::builtins` that allows us to query if a symbol is a Python builtin that was added in a later version than the one that was specified in a user's Ruff configuration. This function allows us to provide a better error message if a builtin such as `aiter`, `anext` etc. is used, but the user either specified a low `target-version` in their Ruff configuration or didn't specify a `target-version` at all (which leads us to infer the default, currently `py38`).

## Test Plan

`cargo test -p ruff_linter --lib`

---

_Label `rule` added by @AlexWaygood on 2024-09-09 16:11_

---

_Label `linter` added by @AlexWaygood on 2024-09-09 16:11_

---

_@AlexWaygood reviewed on 2024-09-09 16:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/undefined_name.rs`:44 on 2024-09-09 16:11_

This feels quite verbose but I'm not sure how to make it any shorter :/

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-09-09 16:11_

---

_Renamed from "[`pyflakes`] Improve error message for `UndefinedName` when a builtin was added in a newer version than specified in Ruff config (`F821`))" to "[`pyflakes`] Improve error message for `UndefinedName` when a builtin was added in a newer version than specified in Ruff config (`F821`)" by @AlexWaygood on 2024-09-09 16:12_

---

_Comment by @codspeed-hq[bot] on 2024-09-09 16:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex/f821-error-msg)

### Merging #13293 will **not alter performance**

<sub>Comparing <code>alex/f821-error-msg</code> (183ecab) with <code>main</code> (b7cef6c)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @AlexWaygood on 2024-09-09 16:27_

> Merging #13293 will **degrade performances by 6.48%**

Seems spurious to me

---

_Comment by @github-actions[bot] on 2024-09-09 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/9515dfa58a144f3644fd29b256113d723c9c1955/testing/_py/test_local.py#L22'>testing/_py/test_local.py:22:45:</a> F821 Undefined name `EncodingWarning`
+ <a href='https://github.com/pytest-dev/pytest/blob/9515dfa58a144f3644fd29b256113d723c9c1955/testing/_py/test_local.py#L22'>testing/_py/test_local.py:22:45:</a> F821 Undefined name `EncodingWarning`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F821 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/pytest-dev/pytest">pytest-dev/pytest</a> (+1 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pytest-dev/pytest/blob/9515dfa58a144f3644fd29b256113d723c9c1955/testing/_py/test_local.py#L22'>testing/_py/test_local.py:22:45:</a> F821 Undefined name `EncodingWarning`
+ <a href='https://github.com/pytest-dev/pytest/blob/9515dfa58a144f3644fd29b256113d723c9c1955/testing/_py/test_local.py#L22'>testing/_py/test_local.py:22:45:</a> F821 Undefined name `EncodingWarning`. Consider specifying `requires-python = ">= 3.10"` or `tool.ruff.target-version = "py310"` in your `pyproject.toml` file.
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F821 | 2 | 1 | 1 | 0 | 0 |

</p>
</details>




---

_@AlexWaygood reviewed on 2024-09-09 16:38_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/undefined_name.rs`:53 on 2024-09-09 16:38_

This could be done as a single `if-let`, but we get nicer docs if we split it up like this (the generated summary message in the table is _very_ long otherwise):

![image](https://github.com/user-attachments/assets/aa7ebda1-8b8c-4c7e-8c6d-bfa0e99b2cda)




---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/builtins.rs`:187 on 2024-09-09 21:11_

Nit: We could consider returning a `&'static [&'static str]` slice here. 

---

_Review comment by @MichaReiser on `crates/ruff_python_stdlib/src/builtins.rs`:387 on 2024-09-09 21:14_

I don't fully understand why we always return minor version 10 even for builtins added in `3.11` or 3.12?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/undefined_name.rs`:44 on 2024-09-09 21:17_

What do you think of instead mentioning the version inferred by ruff. Like. 

```
Added as builtin in Python 3.{v}; but the project configuration specifies 3.{y}
```

---

_@MichaReiser reviewed on 2024-09-09 21:18_

I'm probably too jetlag to understand the new function you added but maybe you can explain.

---

_@AlexWaygood reviewed on 2024-09-09 21:19_

---

_Review comment by @AlexWaygood on `crates/ruff_python_stdlib/src/builtins.rs`:387 on 2024-09-09 21:19_

no you are very correct here, thanks!

---

_@AlexWaygood reviewed on 2024-09-09 21:30_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/undefined_name.rs`:44 on 2024-09-09 21:30_

I'm not sure... I think the most important thing to convey to the user is "how to fix the problem". For most users who have filed bug reports, the problem isn't that they're specifying the _wrong_ `target-version` -- it's that they aren't specifying a `target-version` at all (which leads us to infer our default, Python 3.8). So I think they might be confused by a message that tells them that their project configuration specifies something

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/undefined_name.rs`:44 on 2024-09-10 00:16_

That's fair. Part of my concern is to explicitly mention `target-version` because the version can also be configured with `requires-python. 

---

_@MichaReiser approved on 2024-09-10 00:16_

---

_@AlexWaygood reviewed on 2024-09-10 17:27_

---

_Review comment by @AlexWaygood on `crates/ruff_python_stdlib/src/builtins.rs`:187 on 2024-09-10 17:27_

`&'static [&'static str]` might be tricky, but we could potentially return `impl Iterator<Item = &'static str>`. It's something of a separate change, though, so I'll pursue it as a followup.

---

_Merged by @AlexWaygood on 2024-09-10 18:03_

---

_Closed by @AlexWaygood on 2024-09-10 18:03_

---

_Branch deleted on 2024-09-10 18:03_

---
