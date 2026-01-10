```yaml
number: 6271
title: "Formatter: overlong values without breakpoint are never parenthesized"
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-08-02T12:23:37Z
updated_at: 2023-08-24T12:09:27Z
url: https://github.com/astral-sh/ruff/issues/6271
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: overlong values without breakpoint are never parenthesized

---

_Issue opened by @konstin on 2023-08-02 12:23_

Overlong values in e.g. assignment position without sometimes be parenthesized, this seems to depend on whether parenthesizing actually help too not be overlong anymore. The first step of fixing this issue is figuring out the exact criteria for what is indented and what isn't.

input and our output:
```python
x = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
x_aaaa = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
y_aaaa = a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
z_aaaa = a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
x = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
x_aaaa = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
y_aaaa = "a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
z_aaaa = "a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
```

black:
```python
x = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
x_aaaa = (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
)
y_aaaa = (
    a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
)
z_aaaa = (
    a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
)
x = "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
x_aaaa = (
    "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
)
y_aaaa = (
    "a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
)
z_aaaa = "a.aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa"
```


---

_Label `bug` added by @konstin on 2023-08-02 12:23_

---

_Label `formatter` added by @konstin on 2023-08-02 12:23_

---

_Renamed from "Formatter: overlong attribute chains should be wrapped in parentheses" to "Formatter: overlong value in assignment position without breakpoint are wrongly formatted" by @konstin on 2023-08-02 12:28_

---

_Renamed from "Formatter: overlong value in assignment position without breakpoint are wrongly formatted" to "Formatter: overlong values in assignment position without breakpoint are wrongly formatted" by @konstin on 2023-08-02 12:29_

---

_Label `help wanted` added by @konstin on 2023-08-02 12:31_

---

_Label `help wanted` removed by @MichaReiser on 2023-08-03 06:01_

---

_Comment by @MichaReiser on 2023-08-03 06:05_

This behavior isn't just specific to assignments but applies in general (I haven't understood the details yet). 

I believe [this](https://github.com/psf/black/blob/01b8d3d4095ebdb91d0d39012a517931625c63cb/src/black/linegen.py#L1572-L1578) is the relevant logic in black. Where it splits the line by adding optional parentheses, but only keeps them if all lines now fit. 

I have a [branch](https://github.com/astral-sh/ruff/compare/main...random-tries-to-format-strings) where I tried to get to the bottom of it but I haven't yet found out the right rules that apply (or messed up the Printer implementation)

This is the same as https://github.com/astral-sh/ruff/issues/6059.

---

_Renamed from "Formatter: overlong values in assignment position without breakpoint are wrongly formatted" to "Formatter: overlong values without breakpoint are parenthesized" by @MichaReiser on 2023-08-03 06:06_

---

_Renamed from "Formatter: overlong values without breakpoint are parenthesized" to "Formatter: overlong values without breakpoint are never parenthesized" by @MichaReiser on 2023-08-03 06:06_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-16 07:09_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-08-17 09:42_

---

_Closed by @MichaReiser on 2023-08-24 12:09_

---
