```yaml
number: 9475
title: Bump version to v0.1.12
type: pull_request
state: merged
author: charliermarsh
labels:
  - release
assignees: []
merged: true
base: main
head: charlie/bump
created_at: 2024-01-11T18:36:37Z
updated_at: 2024-01-11T22:26:57Z
url: https://github.com/astral-sh/ruff/pull/9475
synced_at: 2026-01-12T15:55:29Z
```

# Bump version to v0.1.12

---

_@charliermarsh_

_No description provided._

---

_Comment by @github-actions[bot] on 2024-01-11 18:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -4 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -4 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/52cb549f443f09727448251cfee53227c6bca9d2/pandas/core/internals/construction.py#L161'>pandas/core/internals/construction.py:161:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/52cb549f443f09727448251cfee53227c6bca9d2/pandas/core/internals/construction.py#L237'>pandas/core/internals/construction.py:237:5:</a> PLR0917 Too many positional arguments (6/5)
- <a href='https://github.com/pandas-dev/pandas/blob/52cb549f443f09727448251cfee53227c6bca9d2/pandas/core/internals/construction.py#L441'>pandas/core/internals/construction.py:441:9:</a> PLC0415 `import` should be at the top-level of a file
- <a href='https://github.com/pandas-dev/pandas/blob/52cb549f443f09727448251cfee53227c6bca9d2/pandas/core/internals/construction.py#L784'>pandas/core/internals/construction.py:784:5:</a> PLC0415 `import` should be at the top-level of a file
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 2 | 0 | 2 | 0 | 0 |
| PLC0415 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @zanieb by @charliermarsh on 2024-01-11 19:50_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-01-11 19:50_

---

_Marked ready for review by @charliermarsh on 2024-01-11 19:50_

---

_Comment by @codspeed-hq[bot] on 2024-01-11 19:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/bump)

### Merging #9475 will **degrade performances by 4.35%**

<sub>Comparing <code>charlie/bump</code> (97702d9) with <code>main</code> (eb4ed24)</sub>



### Summary

`❌ 1` regressions
`✅ 29` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/bump)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/bump` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `parser[numpy/ctypeslib.py]` | 12.2 ms | 12.8 ms | -4.35% |


---

_Review comment by @AlexWaygood on `CHANGELOG.md`:7 on 2024-01-11 19:57_

```suggestion
- Formatter: Hug multiline-strings in preview style ([#9243](https://github.com/astral-sh/ruff/pull/9243))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:19 on 2024-01-11 19:58_

```suggestion
- [\`flake8-boolean-trap\`] Allow Boolean positional arguments in setters ([#9429](https://github.com/astral-sh/ruff/pull/9429))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:21 on 2024-01-11 20:01_

Looks like these are both changes to RUF011 -- is it correct to classify them as flake8-bugbear?

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:15 on 2024-01-11 20:02_

```suggestion
- [\`ruff\`] Add `parenthesize-chained-operators` (`RUF021`) to enforce parentheses in `a or b and c` ([#9440](https://github.com/astral-sh/ruff/pull/9440))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:26 on 2024-01-11 20:02_

```suggestion
- [\`ruff\`] Add fix for `parenthesize-chained-operators` (`RUF021`) ([#9449](https://github.com/astral-sh/ruff/pull/9449))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:47 on 2024-01-11 20:04_

```suggestion
- [\`flake8-pyi\`] Fix false negative for `unused-private-protocol` (`PYI046`) with unused generic protocols ([#9405](https://github.com/astral-sh/ruff/pull/9405))
```

---

_@AlexWaygood reviewed on 2024-01-11 20:04_

A few small suggestions ;)

---

_@charliermarsh reviewed on 2024-01-11 20:31_

---

_Review comment by @charliermarsh on `CHANGELOG.md`:21 on 2024-01-11 20:31_

Oops, thanks. This rule was moved to flake8-bugbear, but that PR hasn't actually merged yet, my mistake!

---

_Comment by @charliermarsh on 2024-01-11 20:31_

Thanks @AlexWaygood, sloppy work on my part, appreciate it :)

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:37 on 2024-01-11 20:38_

```suggestion
- \[`ruff`\] Allow `Hashable = None` in type annotations (`RUF013`) ([#9442](https://github.com/astral-sh/ruff/pull/9442))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:39 on 2024-01-11 20:39_

```suggestion
- [`pydocstyle`] Disambiguate argument descriptors from section headers ([#9427](https://github.com/astral-sh/ruff/pull/9427))
```

---

_@zanieb approved on 2024-01-11 20:41_

<3

---

_@AlexWaygood approved on 2024-01-11 20:41_

Looks great! Hyped to use some of these in my projects

---

_Label `release` added by @charliermarsh on 2024-01-11 22:07_

---

_Merged by @charliermarsh on 2024-01-11 22:13_

---

_Closed by @charliermarsh on 2024-01-11 22:13_

---

_Branch deleted on 2024-01-11 22:13_

---
