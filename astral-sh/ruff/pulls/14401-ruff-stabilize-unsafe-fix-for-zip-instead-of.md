```yaml
number: 14401
title: "[`ruff`] Stabilize unsafe fix for `zip-instead-of-pairwise (RUF007)`"
type: pull_request
state: merged
author: dylwil3
labels: []
assignees: []
merged: true
base: ruff-0.8
head: stab-ruf007
created_at: 2024-11-17T16:41:57Z
updated_at: 2024-11-18T11:40:44Z
url: https://github.com/astral-sh/ruff/pull/14401
synced_at: 2026-01-12T15:55:47Z
```

# [`ruff`] Stabilize unsafe fix for `zip-instead-of-pairwise (RUF007)`

---

_@dylwil3_

This PR stabilizes the unsafe fix for [zip-instead-of-pairwise (RUF007)](https://docs.astral.sh/ruff/rules/zip-instead-of-pairwise/#zip-instead-of-pairwise-ruf007), which replaces the use of zip with that of itertools.pairwise and has been available under preview since version 0.5.7.

There are no open issues regarding RUF007 at the time of this writing.


---

_Label `do-not-merge` added by @dylwil3 on 2024-11-17 16:41_

---

_Comment by @github-actions[bot] on 2024-11-17 16:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/pypa/setuptools">pypa/setuptools</a> (error)</summary>
<p>

```
ruff failed
  Cause: Failed to parse /home/runner/work/ruff/ruff/checkouts/pypa:setuptools/ruff.toml
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
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
  Cause: TOML parse error at line 8, column 1
  |
8 | [lint]
  | ^^^^^^
Unknown rule selector: `UP027`
```

</p>
</details>




---

_@AlexWaygood approved on 2024-11-17 17:11_

Nice, thank you!

I think you can remove the `do-not-merge` label and merge this into the `ruff-0.8` branch now, actually. https://github.com/astral-sh/ruff/pull/14385 and https://github.com/astral-sh/ruff/pull/14384 have the `do-not-merge` label because they'll "ruin" the ecosystem check for other PRs if we land them now (see conversation in https://github.com/astral-sh/ruff/pull/14384#issuecomment-2480758746). Ruff won't even run on certain ecosystem projects once those PRs have landed, because their pyproject.toml files will fail to be parsed with the rules removed :/

---

_Label `do-not-merge` removed by @dylwil3 on 2024-11-17 17:26_

---

_Merged by @dylwil3 on 2024-11-17 17:27_

---

_Closed by @dylwil3 on 2024-11-17 17:27_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 11:40_

---
