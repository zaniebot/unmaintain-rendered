```yaml
number: 9766
title: "Invert order of checks in `zero-sleep-call`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/zero
created_at: 2024-02-01T23:25:03Z
updated_at: 2024-02-01T23:38:53Z
url: https://github.com/astral-sh/ruff/pull/9766
synced_at: 2026-01-10T22:57:09Z
```

# Invert order of checks in `zero-sleep-call`

---

_Pull request opened by @charliermarsh on 2024-02-01 23:25_

The other conditions are cheaper and should eliminate the vast majority of these checks.

---

_Label `internal` added by @charliermarsh on 2024-02-01 23:25_

---

_@zanieb approved on 2024-02-01 23:25_

---

_Comment by @zanieb on 2024-02-01 23:26_

Could be in a perf category 🤷‍♀️ 

---

_Label `internal` removed by @charliermarsh on 2024-02-01 23:26_

---

_Label `performance` added by @charliermarsh on 2024-02-01 23:26_

---

_Merged by @charliermarsh on 2024-02-01 23:30_

---

_Closed by @charliermarsh on 2024-02-01 23:30_

---

_Branch deleted on 2024-02-01 23:30_

---

_Comment by @github-actions[bot] on 2024-02-01 23:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The `show-source` option has been deprecated in favor of `output-format`'s "full" and "concise" variants. Please update your configuration to use `output-format = <full|concise>` instead.
warning: `RUF011` has been remapped to `B035`.
warning: `TCH006` has been remapped to `TCH010`.
ruff failed
  Cause: Selection of deprecated rule `ANN102` is not allowed when preview is enabled.
```

</p>
</details>




---
