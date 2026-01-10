```yaml
number: 10151
title: Bump version to v0.3.0
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: ruff-0.3
created_at: 2024-02-28T14:56:44Z
updated_at: 2024-02-29T15:05:21Z
url: https://github.com/astral-sh/ruff/pull/10151
synced_at: 2026-01-10T22:47:01Z
```

# Bump version to v0.3.0

---

_Pull request opened by @MichaReiser on 2024-02-28 14:56_

## Summary

Bump Ruff to v0.3

## Test Plan



---

_Converted to draft by @MichaReiser on 2024-02-28 14:56_

---

_Comment by @github-actions[bot] on 2024-02-28 15:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/e14a9bd41d8cd8ac52c5c958b735623fe0eae064/pandas/_libs/tslib.pyi#L21'>pandas/_libs/tslib.pyi:21:5:</a> PLR0917 Too many positional arguments (6/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_Comment by @Skylion007 on 2024-02-28 16:55_

We aren't promoting any rules from preview in this release?

---

_Comment by @charliermarsh on 2024-02-28 17:38_

I want to hold off on promoting the `pycodestyle` rules (since we just added a bunch of new ones, and they still have some issues). The one set I could see us promoting around the `refurb` rules. They're quite complex but we don't receive many issues around them.

---

_Comment by @MichaReiser on 2024-02-28 18:09_

This is not ready to merge but ready to get early feedback on the new documentation

---

_Marked ready for review by @MichaReiser on 2024-02-28 18:09_

---

_Comment by @Skylion007 on 2024-02-28 21:36_

@charliermarsh Thoughts on the PERF lint rules?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:610 on 2024-02-29 10:00_

We fixed this at some point

---

_@MichaReiser reviewed on 2024-02-29 10:00_

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:11 on 2024-02-29 11:51_

```suggestion
Previously, Ruff used one or two blank lines (or the number configured by `isort.lines-after-imports`) after imports in typing stub files (`.pyi` files).
```

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:12 on 2024-02-29 11:52_

```suggestion
The [typing style guide for stubs](https://typing.readthedocs.io/en/latest/source/stubs.html#style-guide) recommends using at most 1 blank line for grouping.
```

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:13 on 2024-02-29 11:53_

```suggestion
As of this release, `isort` now always uses one blank line after imports in stub files, the same as the formatter.
```

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:15 on 2024-02-29 11:54_

I assumed this would be related to the `build` package you can install from PyPI from the heading here

```suggestion
### Remove directories named `build` from the default exclusion list
```

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:17 on 2024-02-29 11:54_

```suggestion
Ruff maintains a list of directories and files that are excluded by default. This list now consists of the following patterns:
```

---

_Review comment by @AlexWaygood on `BREAKING_CHANGES.md`:47 on 2024-02-29 11:56_

```suggestion
Previously, the `build` directory was included in this list. However, the `build` directory tends to be a not-unpopular directory
name, and excluding it by default caused confusion. Ruff now no longer excludes `build` except if it is excluded by a `.gitignore` file
or because it is listed in `extend-exclude`.
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:11 on 2024-02-29 11:57_

```suggestion
- \[`pycodestyle`\] Don't warn about a single whitespace character before a comma in a tuple (`E203`) ([#10094](https://github.com/astral-sh/ruff/pull/10094))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:18 on 2024-02-29 11:57_

```suggestion
- \[`pylint`\] Ignore `sys.version` and `sys.platform` in `PLR1714` ([#10054](https://github.com/astral-sh/ruff/pull/10054))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:24 on 2024-02-29 11:58_

```suggestion
- \[`eradicate`\] Detect single-line code for `try:`, `except:`, etc. (`ERA001`) ([#10057](https://github.com/astral-sh/ruff/pull/10057))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:28 on 2024-02-29 11:59_

it's tautologically new if you're introducing it ;)

```suggestion
This release introduces the Ruff 2024.2 style, stabilizing the following changes:
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:49 on 2024-02-29 12:02_

```suggestion
- \[`pycodestyle`\] Mark fixes overlapping with a multiline string as unsafe for `W293` ([#10049](https://github.com/astral-sh/ruff/pull/10049))
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:51 on 2024-02-29 12:02_

is this relevant to users? Maybe this could be omitted

```suggestion
```

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:54 on 2024-02-29 12:04_

```suggestion
- \[`pylint`\] Delete entire statement, including semicolons when fixing `PLR0203` ([#10074](https://github.com/astral-sh/ruff/pull/10074))
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/README.md`:311 on 2024-02-29 12:06_

```suggestion
Black intended to ship a similar style change as part of the 2024 style that always removes the indent. It turned out that this change was too disruptive to justify the cases where it improved formatting. Ruff introduced the new heuristic of preserving the indent. We believe it's a good compromise that improves formatting but minimizes disruption for users.
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/README.md`:315 on 2024-02-29 12:06_

```suggestion
Black 24 and newer allows blank lines at the start of a block, where Ruff always removes them:
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/README.md`:328 on 2024-02-29 12:07_

```suggestion
Currently, we are concerned that allowing blank lines at the start of a block leads [to unintentional blank lines when refactoring or moving code](https://github.com/astral-sh/ruff/issues/8893#issuecomment-1867259744). However, we will consider adotping Black's formatting at a later point with an improved heuristic. The style change is tracked in [#9745](https://github.com/astral-sh/ruff/issues/9745).
```

---

_@AlexWaygood reviewed on 2024-02-29 12:09_

Very nice overall!

---

_@MichaReiser reviewed on 2024-02-29 12:16_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:51 on 2024-02-29 12:16_

I can rephrase it to explain the issue it fixes

---

_Review comment by @AlexWaygood on `CHANGELOG.md`:51 on 2024-02-29 12:16_

that would be great!

---

_@AlexWaygood reviewed on 2024-02-29 12:17_

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/README.md`:328 on 2024-02-29 12:39_

I think I'm to blame for this one...

```suggestion
Currently, we are concerned that allowing blank lines at the start of a block leads [to unintentional blank lines when refactoring or moving code](https://github.com/astral-sh/ruff/issues/8893#issuecomment-1867259744). However, we will consider adopting Black's formatting at a later point with an improved heuristic. The style change is tracked in [#9745](https://github.com/astral-sh/ruff/issues/9745).
```

---

_@AlexWaygood reviewed on 2024-02-29 12:39_

---

_@AlexWaygood approved on 2024-02-29 12:40_

---

_@MichaReiser reviewed on 2024-02-29 12:41_

---

_Review comment by @MichaReiser on `CHANGELOG.md`:54 on 2024-02-29 12:41_

I keep the `(rule)` for consistency with other changelog entries

---

_Renamed from "Ruff v0.3" to "Bump version to v0.3.0" by @MichaReiser on 2024-02-29 15:05_

---

_Merged by @MichaReiser on 2024-02-29 15:05_

---

_Closed by @MichaReiser on 2024-02-29 15:05_

---

_Branch deleted on 2024-02-29 15:05_

---
