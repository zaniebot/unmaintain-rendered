```yaml
number: 2971
title: Fix for F541 unescape f-string
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: f541-f-string-unescape
created_at: 2023-02-16T20:43:41Z
updated_at: 2023-02-17T22:56:26Z
url: https://github.com/astral-sh/ruff/pull/2971
synced_at: 2026-01-12T04:39:44Z
```

# Fix for F541 unescape f-string

---

_Pull request opened by @sbrugman on 2023-02-16 20:43_

Closes #2936

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pyflakes/F541.py`:37 on 2023-02-16 22:35_

What about cases like...

```py
>>> f"\{{x}}"
'\\{x}'
>>> f"{{a {{x}}"
'{a {x}'
>>> f"{{{{x}}}}"
'{{x}}'
```

Any others to consider? (These may all be handled appropriately.)

---

_@charliermarsh reviewed on 2023-02-16 22:35_

---

_@sbrugman reviewed on 2023-02-17 10:22_

---

_Review comment by @sbrugman on `crates/ruff/resources/test/fixtures/pyflakes/F541.py`:37 on 2023-02-17 10:22_

Interesting. The second and third case are handled correctly. The first gives a parsing error:
`Error: f-string: single '}' is not allowed at line 1 column 8` (while python accepts it). RustPython handles it correctly.

(`cargo dev print-ast ./crates/ruff/resources/test/fixtures/pyflakes/F541.py` to reproduce with line 43 uncommented)

---

_Merged by @charliermarsh on 2023-02-17 19:27_

---

_Closed by @charliermarsh on 2023-02-17 19:27_

---

_Branch deleted on 2023-02-17 22:56_

---
