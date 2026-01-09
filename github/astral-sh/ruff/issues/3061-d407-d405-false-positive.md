---
number: 3061
title: "D407 / D405: false positive"
type: issue
state: closed
author: spaceone
labels:
  - question
assignees: []
created_at: 2023-02-20T15:21:34Z
updated_at: 2023-02-21T22:44:46Z
url: https://github.com/astral-sh/ruff/issues/3061
synced_at: 2026-01-07T13:12:14-06:00
---

# D407 / D405: false positive

---

_Issue opened by @spaceone on 2023-02-20 15:21_

```
D407 [*] Missing dashed underline after section ("TODO")
   |
 3 | / """
 4 | | foo.
 5 | |
 6 | | Follows <https://blah>.
 7 | |
 8 | | TODO:
 9 | | *        nicht beenden
10 | | *        unmaintained
11 | | """
   | |___^ D407
```
is not detected by `pydocstyle`.

---

_Renamed from "D407: false positive" to "D407 / D405: false positive" by @spaceone on 2023-02-20 15:22_

---

_Comment by @charliermarsh on 2023-02-20 17:30_

I get D405 and D407 on that docstring with pydocstyle:

```
foo.py:1 at module level:
        D100: Missing docstring in public module
foo.py:2 in public function `f`:
        D212: Multi-line docstring summary should start at the first line
foo.py:2 in public function `f`:
        D405: Section name should be properly capitalized ('Todo', not 'TODO')
foo.py:2 in public function `f`:
        D413: Missing blank line after last section ('Todo')
foo.py:2 in public function `f`:
        D407: Missing dashed underline after section ('Todo')
```

I think it's also "correct" in that TODO is an official Google docstring section header.

---

_Label `question` added by @charliermarsh on 2023-02-20 17:30_

---

_Closed by @charliermarsh on 2023-02-21 22:44_

---
