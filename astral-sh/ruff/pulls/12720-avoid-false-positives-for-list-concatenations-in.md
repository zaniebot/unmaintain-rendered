```yaml
number: 12720
title: Avoid false-positives for list concatenations in SQL construction
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/s
created_at: 2024-08-06T20:12:27Z
updated_at: 2024-08-06T20:26:07Z
url: https://github.com/astral-sh/ruff/pull/12720
synced_at: 2026-01-10T21:47:02Z
```

# Avoid false-positives for list concatenations in SQL construction

---

_Pull request opened by @charliermarsh on 2024-08-06 20:12_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12710.


---

_Label `bug` added by @charliermarsh on 2024-08-06 20:12_

---

_Merged by @charliermarsh on 2024-08-06 20:26_

---

_Closed by @charliermarsh on 2024-08-06 20:26_

---

_Comment by @github-actions[bot] on 2024-08-06 20:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -3 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -3 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
- <a href='https://github.com/python/typeshed/blob/8a7f09e3511f3a1d04281c60167b8dcc3b78938b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L2165'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:2165:5:</a> F821 Undefined name `DESCR`
- <a href='https://github.com/python/typeshed/blob/8a7f09e3511f3a1d04281c60167b8dcc3b78938b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L2177'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:2177:27:</a> F821 Undefined name `UnpackResponse`
- <a href='https://github.com/python/typeshed/blob/8a7f09e3511f3a1d04281c60167b8dcc3b78938b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L2181'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:2181:5:</a> PYI021 Docstrings should not be included in stubs
+ <a href='https://github.com/python/typeshed/blob/8a7f09e3511f3a1d04281c60167b8dcc3b78938b/stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi#L2198'>stubs/tensorflow/tensorflow/compiler/xla/xla_pb2.pyi:2198:5:</a> PYI021 Docstrings should not be included in stubs
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI021 | 2 | 1 | 1 | 0 | 0 |
| F821 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Branch deleted on 2024-08-06 20:26_

---
