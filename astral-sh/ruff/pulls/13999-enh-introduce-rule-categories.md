```yaml
number: 13999
title: "Enh: introduce Rule Categories"
type: pull_request
state: closed
author: sbrugman
labels: []
assignees: []
draft: true
base: main
head: rule-categories
created_at: 2024-10-30T13:59:38Z
updated_at: 2025-05-18T16:32:57Z
url: https://github.com/astral-sh/ruff/pull/13999
synced_at: 2026-01-10T18:51:01Z
```

# Enh: introduce Rule Categories

---

_Pull request opened by @sbrugman on 2024-10-30 13:59_

PR to move the needle on the grouping of rules as in the original issue #1774. 
Groups are distinct from the re-categorisation of rules (see context)! - which I’d also like to help tackle if possible

Categorisation is hard to undo and needs some executive decisions.
Let’s thus not have the discussion here, but on discord/the existing issues.

Context:
- Focusses on the effect of the change rather than on the pattern that is detected, e.g. performance, bugs, style
- Thus different angle at subsetting rule, additional to “linter” (which is currently the origin, and after the re-grouping is similar to “checks” in Pylint)
- Since categories group the motivations behind a change (i.e. the “Why is this bad section”), they can be used to document the general reasoning behind them and simplify rule documentation (e.g. why should developers write idiomatic code)
- Taking subsets of the rules in this way has proven effective, e.g. in clippy, but is not sufficient (e.g. it does not permit for subsetting of all “Docstring” or “Django” rules).
- Introducing categories is a preparation for re-grouping of rules (e.g. under “imports” the category should distinguish between “unused imports” and “isort”)
- Mixing both abstractions will not result in a single category per rule like we have now with grouping per origin (e.g. “performance” and “django” can go together). 

Implementation:
- Implementation is non-breaking, the category attribute is added on top of the existing functionality
- Rules can be incrementally assigned to categories. When uncategorised, the behaviour is identical as before.
- Implemented as one category per rule. A many-to-many should also be considered (e.g. some rules are both more idiomatic and more performant).

Known limitations: 
- Category assignment of 800+ rules will likely have a couple of misclassifications. This should not be a problem, rules can be re-assigned. As long as the category definitions are deterministic and objective.
- Sometimes rule effect is more readable and more performant, or a syntax upgrade allows for faster code  => so need to choose the most fitting. Which category would the user expect?
- Rule selection via combinations of categories/linters are not possible yet, this is worth considering as follow-up.


---

_Comment by @github-actions[bot] on 2024-10-30 14:19_

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

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

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

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
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

_Closed by @MichaReiser on 2025-05-18 16:32_

---
