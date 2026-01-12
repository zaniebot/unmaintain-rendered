```yaml
number: 9653
title: Detect automagic-like assignments in notebooks
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/auto
created_at: 2024-01-26T16:27:50Z
updated_at: 2024-01-29T13:05:07Z
url: https://github.com/astral-sh/ruff/pull/9653
synced_at: 2026-01-12T15:55:29Z
```

# Detect automagic-like assignments in notebooks

---

_@charliermarsh_

## Summary

Given a statement like `colors = 6`, we currently treat the cell as an automagic (since `colors` is an automagic) -- i.e., we assume it's equivalent to `%colors = 6`. This PR adds some additional detection whereby if the statement is an _assignment_, we avoid treating it as such. I audited the list of automagics, and I believe this is safe for all of them.

Closes https://github.com/astral-sh/ruff/issues/8526.

Closes https://github.com/astral-sh/ruff/issues/9648.

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-01-26 16:27_

---

_Label `bug` added by @charliermarsh on 2024-01-26 16:27_

---

_Comment by @github-actions[bot] on 2024-01-26 16:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>

<pre>
- examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 14:1:1: F821 Undefined name `df`
- examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 4:14:8: F401 [*] `requests` imported but unused
- examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 4:16:17: F401 [*] `bs4.BeautifulSoup` imported but unused
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 2 | 0 | 2 | 0 | 0 |
| F821 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -3 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (+0 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
- examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 14:1:1: F821 Undefined name `df`
- examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 4:14:8: F401 [*] `requests` imported but unused
- examples/llms/RAG/question-generation-retrieval-evaluation.ipynb:cell 4:16:17: F401 [*] `bs4.BeautifulSoup` imported but unused
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| F401 | 2 | 0 | 2 | 0 | 0 |
| F821 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-01-26 19:06_

Ecosystem checks are correct (i.e., they were false positives before the change).

---

_Review comment by @dhruvmanila on `crates/ruff_notebook/src/cell.rs`:170 on 2024-01-29 07:50_

I think the correct wording would be that "The assignment operators can never follow an automagic [..]" because `foo = !pwd` is valid and `foo` will be the output of `pwd` command.

---

_@dhruvmanila approved on 2024-01-29 07:50_

---

_Merged by @charliermarsh on 2024-01-29 12:55_

---

_Closed by @charliermarsh on 2024-01-29 12:55_

---

_Branch deleted on 2024-01-29 12:55_

---
