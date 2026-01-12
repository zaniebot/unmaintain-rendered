```yaml
number: 12023
title: "E203: false positive in f-string using formatting"
type: issue
state: closed
author: JaRoSchm
labels:
  - bug
assignees: []
created_at: 2024-06-25T06:55:10Z
updated_at: 2024-06-25T09:30:32Z
url: https://github.com/astral-sh/ruff/issues/12023
synced_at: 2026-01-12T15:54:51Z
```

# E203: false positive in f-string using formatting

---

_@JaRoSchm_

Hi,

this example raises E203 (ruff 0.4.10) although it clearly shouldn't in my opinion:
```python
x = 3.14159
print(f"{x = :.2f}")
```

Output of `ruff check --select E203 --preview`:
```
test.py:2:13: E203 [*] Whitespace before ':'
  |
1 | x = 3.14159
2 | print(f"{x = :.2f}")
  |             ^ E203
  |
  = help: Remove whitespace before ':'
```

However, removing the whitespace changes the output from `x = 3.14` to `x =3.14`.

---

_Comment by @dhruvmanila on 2024-06-25 08:23_

Yeah, I agree. I remember updating the rules to avoid checking inside f-strings when I added support for it, I guess I must've missed this one. Thank you for reporting!

---

_Label `bug` added by @dhruvmanila on 2024-06-25 08:23_

---

_Assigned to @dhruvmanila by @dhruvmanila on 2024-06-25 08:23_

---

_Closed by @dhruvmanila on 2024-06-25 09:30_

---
