```yaml
number: 196
title: "Syntax error doesn't cause ruff to exit with non-zero code"
type: issue
state: closed
author: Jackenmen
labels:
  - bug
assignees: []
created_at: 2022-09-15T01:51:46Z
updated_at: 2022-09-15T02:37:56Z
url: https://github.com/astral-sh/ruff/issues/196
synced_at: 2026-01-12T15:54:40Z
```

# Syntax error doesn't cause ruff to exit with non-zero code

---

_@Jackenmen_

For code with invalid syntax, e.g:
```py
def x():
```

The output is:
```
❯ ruff repro.py
[2022-09-15][03:50:47][ruff][ERROR] Failed to check repro.py: Got unexpected EOF at line 2 column 1
Found 0 error(s).

❯ echo $?            
0

❯ flake8 repro.py
repro.py:1:8: E999 SyntaxError: unexpected EOF while parsing

❯ echo $?            
1
```

---

_Label `bug` added by @charliermarsh on 2022-09-15 02:32_

---

_Closed by @charliermarsh on 2022-09-15 02:37_

---
