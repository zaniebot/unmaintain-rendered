```yaml
number: 10965
title: Resolve classes and functions relative to script name
type: pull_request
state: merged
author: charliermarsh
labels:
  - configuration
assignees: []
merged: true
base: main
head: charlie/script
created_at: 2024-04-16T02:58:44Z
updated_at: 2024-04-18T04:55:56Z
url: https://github.com/astral-sh/ruff/pull/10965
synced_at: 2026-01-10T22:37:01Z
```

# Resolve classes and functions relative to script name

---

_Pull request opened by @charliermarsh on 2024-04-16 02:58_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

If the user is analyzing a script (i.e., we have no module path), it seems reasonable to use the script name when trying to identify paths to objects defined _within_ the script.

Closes https://github.com/astral-sh/ruff/issues/10960.

## Test Plan

Ran:

```shell
check --isolated --select=B008 \
    --config 'lint.flake8-bugbear.extend-immutable-calls=["test.A"]' \
    test.py
```

On:

```python
class A: pass

def f(a=A()):
    pass
```

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-04-16 02:58_

---

_Label `bug` added by @charliermarsh on 2024-04-16 02:58_

---

_Label `bug` removed by @charliermarsh on 2024-04-16 02:59_

---

_Label `configuration` added by @charliermarsh on 2024-04-16 02:59_

---

_@charliermarsh reviewed on 2024-04-16 03:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:771 on 2024-04-16 03:00_

I consider changing `Module` to return `&[script_name]` in the script case, but the path is typically used for resolving relative imports, and I'm not sure it makes as much sense for us to resolve relative imports relative to the script name given that it's definitionally a standalone script.


---

_@zanieb approved on 2024-04-16 03:00_

---

_Comment by @zanieb on 2024-04-16 03:01_

Not worth unit test coverage?

---

_Comment by @charliermarsh on 2024-04-16 03:02_

Will try.

---

_Comment by @github-actions[bot] on 2024-04-16 03:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/12b9e48324df292379db3eca4e3c54175e5f374e/stdlib/typing.pyi#L330'>stdlib/typing.pyi:330:10:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/12b9e48324df292379db3eca4e3c54175e5f374e/stdlib/typing.pyi#L330'>stdlib/typing.pyi:330:10:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-04-16 03:29_

I think those typeshed changes are arguably good but I'll let @AlexWaygood review.

---

_Comment by @AlexWaygood on 2024-04-16 06:28_

> I think those typeshed changes are arguably good but I'll let @AlexWaygood review.

The `typing.pyi` change is definitely correct — we already flag that in flake8-pyi, and I'd expect it to be flagged.

The builtins hit seems like a false positive though — the existing annotation looks correct there. Something now isn't being inferred as being a builtin symbol because we're actually in builtins.pyi itself?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:49 on 2024-04-16 09:28_

Nit: Unrelated to this PR. It's a bit surprising that `ModuleSource` is defined in `visibility` when it is used for much more. 

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/model.rs`:838 on 2024-04-16 09:30_

I think the terminology is inconsistent which makes this change difficult to understand. For example, I don't understand when a file is a script? Sould we rename one of the variants in `ModuleSource` to script? or are the concepts independent from each other?

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/definition.rs`:45 on 2024-04-16 09:31_

Nit: I wonder if we should name this `qualified_name` instead. `Path` is ambigious because it could either be a file path or a module path.

---

_@MichaReiser reviewed on 2024-04-16 09:31_

---

_Comment by @AlexWaygood on 2024-04-16 10:21_

> The builtins hit seems like a false positive though — the existing annotation looks correct there. Something now isn't being inferred as being a builtin symbol because we're actually in builtins.pyi itself?

Oh! I missed that the error was _going away_ rather than being introduced! The typeshed hits both look like clear improvements, then 😃

---

_@AlexWaygood approved on 2024-04-16 10:23_

---

_@charliermarsh reviewed on 2024-04-16 17:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/definition.rs`:49 on 2024-04-16 17:43_

Good call, I moved it.

---

_@charliermarsh reviewed on 2024-04-16 17:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/definition.rs`:45 on 2024-04-16 17:43_

Good call.

---

_@charliermarsh reviewed on 2024-04-16 17:44_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:838 on 2024-04-16 17:44_

Good call.

---

_@charliermarsh reviewed on 2024-04-16 17:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/model.rs`:838 on 2024-04-16 17:45_

Actually, they're somewhat independent (like `Script` and `Path` are odd as variants because they're not in the same category of concepts).

---

_Comment by @codspeed-hq[bot] on 2024-04-16 17:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/script)

### Merging #10965 will **not alter performance**

<sub>Comparing <code>charlie/script</code> (df60061) with <code>main</code> (06b3e37)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Merged by @charliermarsh on 2024-04-18 01:42_

---

_Closed by @charliermarsh on 2024-04-18 01:42_

---

_Branch deleted on 2024-04-18 01:42_

---

_Comment by @fofoni on 2024-04-18 04:54_

Thank you all for handling this so quickly!!

---

_Comment by @charliermarsh on 2024-04-18 04:55_

No problem, sorry that it took a few days to merge -- I had to debug a performance regression :joy:

---
