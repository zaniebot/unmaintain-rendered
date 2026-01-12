```yaml
number: 14695
title: ANN401 induces a stack overflow on quoted quoted escape sequences
type: issue
state: open
author: dscorbett
labels:
  - bug
  - help wanted
  - parser
assignees: []
created_at: 2024-11-30T22:00:34Z
updated_at: 2024-12-01T09:19:21Z
url: https://github.com/astral-sh/ruff/issues/14695
synced_at: 2026-01-12T15:54:54Z
```

# ANN401 induces a stack overflow on quoted quoted escape sequences

---

_@dscorbett_

[`any-type` (ANN401)](https://docs.astral.sh/ruff/rules/any-type/) overflows the stack in Ruff 0.6.4 through Ruff 0.8.1 for a quoted annotation with an extra layer of quotation marks containing an escape sequence.

```console
$ cat ann401.py
def f(x: "'in\x74'"): pass

$ ruff check --isolated --select ANN401 ann401.py

thread 'main' has overflowed its stack
fatal runtime error: stack overflow
```

---

_Label `bug` added by @AlexWaygood on 2024-11-30 22:59_

---

_Label `help wanted` added by @AlexWaygood on 2024-11-30 22:59_

---

_Label `parser` added by @dylwil3 on 2024-12-01 09:19_

---
