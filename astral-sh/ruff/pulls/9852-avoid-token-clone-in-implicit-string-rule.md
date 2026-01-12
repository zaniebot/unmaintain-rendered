```yaml
number: 9852
title: Avoid token clone in implicit string rule
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/token-clone
created_at: 2024-02-06T03:14:34Z
updated_at: 2024-02-06T03:28:05Z
url: https://github.com/astral-sh/ruff/pull/9852
synced_at: 2026-01-12T15:55:30Z
```

# Avoid token clone in implicit string rule

---

_@charliermarsh_

_No description provided._

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:116 on 2024-02-06 03:18_

Is the implicit clone because of tuple_windows? The clone shouldn't e a problem here as far as I understand because the iterator items are references and not owned Tokens

---

_@MichaReiser reviewed on 2024-02-06 03:18_

---

_@charliermarsh reviewed on 2024-02-06 03:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:116 on 2024-02-06 03:24_

Ah right I was misreading. Was just trying to understand why these rules are SO expensive.

---

_Closed by @charliermarsh on 2024-02-06 03:24_

---

_@charliermarsh reviewed on 2024-02-06 03:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:116 on 2024-02-06 03:24_

(Meant this to be a draft, just wanted the benchmarks.)

---

_Comment by @github-actions[bot] on 2024-02-06 03:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
