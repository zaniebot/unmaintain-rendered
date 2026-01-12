```yaml
number: 4897
title: "F601: Duplicated keys with different parenthesisation fixed incorrectly"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-06-06T08:45:18Z
updated_at: 2023-09-21T01:28:13Z
url: https://github.com/astral-sh/ruff/issues/4897
synced_at: 2026-01-12T15:54:45Z
```

# F601: Duplicated keys with different parenthesisation fixed incorrectly

---

_@addisoncrump_

This is a case which I believe is potentially indicative of other bugs with the F601 fix.

Duplicated keys are merged by Ruff in the case of equal values, but the method by which this is done leads to a strange error in the case of different parenthesisation:

```py
t={"x":"test123", "x":("test123")}
```

And the applied diff:

```
Fixed source has a syntax error where the source document does not. This is a bug in one of the generated fixes:
<filename>:1:17: E999 SyntaxError: unexpected token ')'
  |
1 | t={"x":"test123")}
  |                 ^ E999
  |


Last generated fixes:
<filename>:1:19: F601 [*] Dictionary key literal `"x"` repeated
  |
1 | t={"x":"test123", "x":("test123")}
  |                   ^^^ F601
  |
  = help: Remove repeated key literal `"x"`

ℹ Suggested fix
1   |-t={"x":"test123", "x":("test123")}
  1 |+t={"x":"test123")}


Source with applied fixes:
t={"x":"test123")}
```

Discovered by #4822.

---

_Label `bug` added by @charliermarsh on 2023-06-06 14:27_

---

_Label `autofix` added by @charliermarsh on 2023-06-06 14:27_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-21 01:05_

---

_Closed by @charliermarsh on 2023-09-21 01:28_

---
