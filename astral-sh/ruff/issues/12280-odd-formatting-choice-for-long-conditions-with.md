```yaml
number: 12280
title: Odd formatting choice for long conditions with comments in list/dict comprehensions
type: issue
state: closed
author: goodspark
labels:
  - bug
  - formatter
assignees: []
created_at: 2024-07-10T17:56:31Z
updated_at: 2024-07-11T20:38:13Z
url: https://github.com/astral-sh/ruff/issues/12280
synced_at: 2026-01-12T15:54:51Z
```

# Odd formatting choice for long conditions with comments in list/dict comprehensions

---

_@goodspark_

Previously used search terms: format for loop if comment

Code:
```py
x = [1, 2, 3]
y = [
    a
    for a in x 
    if (
        # asdasd
        "askldaklsdnmklasmdlkasmdlkasmdlkasmdasd" != "as,mdnaskldmlkasdmlaksdmlkasdlkasdm"
        and "zxcm,.nzxclm,zxnckmnzxckmnzxczxc" != "zxcasdasdlmnasdlknaslkdnmlaskdm"
    )
]
```

Comparison of Black (previous) vs Ruff (new) outputs:
```diff
 y = [
     a
     for a in x
-    if (
+    if
+    (
         # asdasd
         "askldaklsdnmklasmdlkasmdlkasmdlkasmdasd"
         != "as,mdnaskldmlkasdmlaksdmlkasdlkasdm"
```

<details>
<summary>Ruff's output</summary>

```py
x = [1, 2, 3]
y = [
    a
    for a in x
    if
    (
        # asdasd
        "askldaklsdnmklasmdlkasmdlkasmdlkasmdasd"
        != "as,mdnaskldmlkasdmlaksdmlkasdlkasdm"
        and "zxcm,.nzxclm,zxnckmnzxckmnzxczxc" != "zxcasdasdlmnasdlknaslkdnmlaskdm"
    )
]
```
</details>

<details>
<summary>Black's output</summary>

```py
x = [1, 2, 3]
y = [
    a
    for a in x
    if (
        # asdasd
        "askldaklsdnmklasmdlkasmdlkasmdlkasmdasd"
        != "as,mdnaskldmlkasdmlaksdmlkasdlkasdm"
        and "zxcm,.nzxclm,zxnckmnzxckmnzxczxc" != "zxcasdasdlmnasdlknaslkdnmlaskdm"
    )
]
```
</details>

If I move the comment out of the inner parentheses of the loop condition, the outputs match:
```py
    for a in x 
    # asdasd
    if (
```

Command: `ruff format`

Ruff settings: None for formatting, but `target-python` is given at the project level, if that matters.
```toml
[project]
target-python = "==3.10"
```
Python version: 3.10
Ruff version: 0.5.0
Black version: 24.3.0

---

_Label `formatter` added by @MichaReiser on 2024-07-10 19:59_

---

_Label `bug` added by @MichaReiser on 2024-07-11 06:26_

---

_Closed by @MichaReiser on 2024-07-11 20:38_

---
