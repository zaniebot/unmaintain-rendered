---
number: 15830
title: C420 false negative at the start of input for builtin static values
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - preview
assignees: []
created_at: 2025-01-30T16:29:26Z
updated_at: 2025-01-30T21:49:15Z
url: https://github.com/astral-sh/ruff/issues/15830
synced_at: 2026-01-07T13:12:16-06:00
---

# C420 false negative at the start of input for builtin static values

---

_Issue opened by @dscorbett on 2025-01-30 16:29_

### Description

[`unnecessary-dict-comprehension-for-iterable` (C420)](https://docs.astral.sh/ruff/rules/unnecessary-dict-comprehension-for-iterable/) has a false negative in Ruff 0.9.3 when the static value is a builtin and the comprehension starts at the very beginning of the input.
```console
$ printf '{x: NotImplemented for x in "XY"}\n' | ruff --isolated check --preview --select C420 - --output-format=concise
All checks passed!
```

If the value is not a builtin, the rule works.
```console
$ printf '{x: 1 for x in "XY"}\n' | ruff --isolated check --preview --select C420 - --output-format=concise --fix -q
dict.fromkeys("XY", 1)

$ printf '{x: None for x in "XY"}\n' | ruff --isolated check --preview --select C420 - --output-format=concise --fix -q
dict.fromkeys("XY")
```

If even a single character appears before the comprehension, the rule works.
```console
$ printf '\n{x: NotImplemented for x in "XY"}\n' | ruff --isolated check --preview --select C420 - --output-format=concise --fix -q

dict.fromkeys("XY", NotImplemented)

$ printf '({x: NotImplemented for x in "XY"})\n' | ruff --isolated check --preview --select C420 - --output-format=concise --fix -q
(dict.fromkeys("XY", NotImplemented))
```

---

_Label `bug` added by @dylwil3 on 2025-01-30 21:21_

---

_Label `preview` added by @dylwil3 on 2025-01-30 21:24_

---

_Referenced in [astral-sh/ruff#15837](../../astral-sh/ruff/pulls/15837.md) on 2025-01-30 21:30_

---

_Comment by @dylwil3 on 2025-01-30 21:31_

What a weird one... but I think I figured it out 😄 

---

_Closed by @dylwil3 on 2025-01-30 21:49_

---

_Closed by @dylwil3 on 2025-01-30 21:49_

---
