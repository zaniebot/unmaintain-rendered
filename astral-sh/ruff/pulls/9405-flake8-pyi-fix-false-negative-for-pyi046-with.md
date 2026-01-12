```yaml
number: 9405
title: "[flake8-pyi] Fix false negative for PYI046 with unused generic protocols"
type: pull_request
state: merged
author: AlexWaygood
labels: []
assignees: []
merged: true
base: main
head: pyi046
created_at: 2024-01-05T18:34:43Z
updated_at: 2024-01-05T18:56:20Z
url: https://github.com/astral-sh/ruff/pull/9405
synced_at: 2026-01-12T15:55:28Z
```

# [flake8-pyi] Fix false negative for PYI046 with unused generic protocols

---

_@AlexWaygood_

I just fixed this false negative in flake8-pyi (https://github.com/PyCQA/flake8-pyi/pull/460), and then realised ruff has the exact same bug! Luckily it's a very easy fix.

(The bug is that unused protocols go undetected if they're generic.)

---

_Converted to draft by @AlexWaygood on 2024-01-05 18:38_

---

_Marked ready for review by @AlexWaygood on 2024-01-05 18:39_

---

_Comment by @github-actions[bot] on 2024-01-05 18:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select PYI</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/a7c5d8bc14597c187457606aca22b61ed357aba4/stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi#L32'>stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi:32:7:</a> PYI046 Private protocol `_HasTagAndGet` is never used
+ <a href='https://github.com/python/typeshed/blob/a7c5d8bc14597c187457606aca22b61ed357aba4/stubs/protobuf/google/protobuf/internal/type_checkers.pyi#L5'>stubs/protobuf/google/protobuf/internal/type_checkers.pyi:5:7:</a> PYI046 Private protocol `_ValueChecker` is never used
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI046 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select PYI</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/a7c5d8bc14597c187457606aca22b61ed357aba4/stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi#L32'>stubs/openpyxl/openpyxl/xml/_functions_overloads.pyi:32:7:</a> PYI046 Private protocol `_HasTagAndGet` is never used
+ <a href='https://github.com/python/typeshed/blob/a7c5d8bc14597c187457606aca22b61ed357aba4/stubs/protobuf/google/protobuf/internal/type_checkers.pyi#L5'>stubs/protobuf/google/protobuf/internal/type_checkers.pyi:5:7:</a> PYI046 Private protocol `_ValueChecker` is never used
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI046 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_@zanieb approved on 2024-01-05 18:55_

Thanks!

---

_Merged by @zanieb on 2024-01-05 18:56_

---

_Closed by @zanieb on 2024-01-05 18:56_

---

_Branch deleted on 2024-01-05 18:56_

---
