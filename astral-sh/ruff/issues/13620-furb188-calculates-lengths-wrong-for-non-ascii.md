```yaml
number: 13620
title: FURB188 calculates lengths wrong for non-ASCII affixes
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-10-03T22:49:37Z
updated_at: 2024-10-07T14:13:30Z
url: https://github.com/astral-sh/ruff/issues/13620
synced_at: 2026-01-12T15:54:53Z
```

# FURB188 calculates lengths wrong for non-ASCII affixes

---

_@dscorbett_

[`slice-to-remove-prefix-or-suffix` (FURB188)](https://docs.astral.sh/ruff/rules/slice-to-remove-prefix-or-suffix/) determines string length by UTF-8 code units, as in Rust, whereas Python counts code points. This discrepancy causes problems for non-ASCII affixes. In the following, furb188.py has two parts: the first has a false positive and the second has a false negative.

```console
$ ruff --version
ruff 0.6.8
$ cat furb188.py
text = "řetězec"
if text.startswith("ř"):
    text = text[2:]
print(text)

text = "řetězec"
if text.startswith("ř"):
    text = text[1:]
print(text)
$ python furb188.py
tězec
etězec
$ ruff check --isolated --preview --target-version py39 --select FURB188 furb188.py --fix
Found 1 error (1 fixed, 0 remaining).
$ cat furb188.py
text = "řetězec"
text = text.removeprefix("ř")
print(text)

text = "řetězec"
if text.startswith("ř"):
    text = text[1:]
print(text)
$ python furb188.py
etězec
etězec
```

---

_Comment by @dylwil3 on 2024-10-04 04:47_

Thank you! I'll try to fix this shortly.

---

_Label `bug` added by @AlexWaygood on 2024-10-04 09:44_

---

_Closed by @MichaReiser on 2024-10-07 14:13_

---
