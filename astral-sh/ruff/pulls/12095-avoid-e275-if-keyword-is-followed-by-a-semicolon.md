```yaml
number: 12095
title: "Avoid `E275` if keyword is followed by a semicolon"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/autofix-bug
created_at: 2024-06-28T15:05:06Z
updated_at: 2024-06-28T15:21:37Z
url: https://github.com/astral-sh/ruff/pull/12095
synced_at: 2026-01-12T15:55:40Z
```

# Avoid `E275` if keyword is followed by a semicolon

---

_@dhruvmanila_

fixes: #12094 

---

_Label `bug` added by @dhruvmanila on 2024-06-28 15:05_

---

_@charliermarsh approved on 2024-06-28 15:09_

---

_Comment by @github-actions[bot] on 2024-06-28 15:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 1 project error; 48 projects unchanged)

<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/milvus-io/pymilvus/blob/99ad63e98c80bf06fbe1bdfd56de37bc41340522/examples/example_bulkinsert_json.py#L209'>examples/example_bulkinsert_json.py:209:13:</a> E275 [*] Missing whitespace after keyword
- <a href='https://github.com/milvus-io/pymilvus/blob/99ad63e98c80bf06fbe1bdfd56de37bc41340522/examples/example_bulkinsert_numpy.py#L211'>examples/example_bulkinsert_numpy.py:211:13:</a> E275 [*] Missing whitespace after keyword
</pre>

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E275 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Merged by @dhruvmanila on 2024-06-28 15:21_

---

_Closed by @dhruvmanila on 2024-06-28 15:21_

---

_Branch deleted on 2024-06-28 15:21_

---
