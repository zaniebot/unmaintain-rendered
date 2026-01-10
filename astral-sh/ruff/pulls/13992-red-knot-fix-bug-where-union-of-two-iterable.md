```yaml
number: 13992
title: "[red-knot] Fix bug where union of two iterable types was not recognised as iterable"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/iter-bug
created_at: 2024-10-30T11:40:43Z
updated_at: 2024-10-31T11:24:18Z
url: https://github.com/astral-sh/ruff/pull/13992
synced_at: 2026-01-10T20:59:37Z
```

# [red-knot] Fix bug where union of two iterable types was not recognised as iterable

---

_Pull request opened by @AlexWaygood on 2024-10-30 11:40_

Fixes #13990. My favourite kind of bugfix: more accuracy for less code!

---

_Review requested from @carljm by @AlexWaygood on 2024-10-30 11:40_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-30 11:40_

---

_Merged by @AlexWaygood on 2024-10-30 11:54_

---

_Closed by @AlexWaygood on 2024-10-30 11:54_

---

_Branch deleted on 2024-10-30 11:54_

---

_Comment by @github-actions[bot] on 2024-10-30 12:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 1, column 11
  |
1 | include = "pyproject.toml"
  |           ^^^^^^^^^^^^^^^^
invalid type: string "pyproject.toml", expected a sequence
```

</p>
</details>




---

_Label `red-knot` added by @AlexWaygood on 2024-10-30 12:07_

---

_@MichaReiser reviewed on 2024-10-30 12:31_

It would be nice to also have a test where one type is iterable and the other is not. E.g. iterator over `str | int`. 

This should work as expected today but I think the fix you pushed now would have regressed the diagnostic precision when our errors distinguish between a non-iterable and an iterator missing the `__next__` method. 



---

_Comment by @AlexWaygood on 2024-10-30 12:52_

> This should work as expected today

in fact, it does not... great catch!

---

_Comment by @carljm on 2024-10-30 15:57_

> in fact, it does not

Should we capture this in an issue?

---

_Comment by @AlexWaygood on 2024-10-30 16:05_

> > in fact, it does not
> 
> Should we capture this in an issue?

I am working on a fix. But if I do not succeed in putting up the fix, I will write up an issue.

---

_Comment by @AlexWaygood on 2024-10-31 11:24_

> > > in fact, it does not
> > 
> > 
> > Should we capture this in an issue?
> 
> I am working on a fix. But if I do not succeed in putting up the fix, I will write up an issue.

I did not succeed in putting up the fix! Here is the issue instead: https://github.com/astral-sh/ruff/issues/14012

---
