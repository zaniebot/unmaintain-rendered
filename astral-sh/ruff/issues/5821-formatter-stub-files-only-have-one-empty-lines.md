```yaml
number: 5821
title: "Formatter: stub files only have one empty lines between top level items"
type: issue
state: closed
author: konstin
labels:
  - good first issue
  - formatter
  - help wanted
assignees: []
created_at: 2023-07-17T06:47:08Z
updated_at: 2023-08-22T14:20:18Z
url: https://github.com/astral-sh/ruff/issues/5821
synced_at: 2026-01-10T11:09:48Z
```

# Formatter: stub files only have one empty lines between top level items

---

_Issue opened by @konstin on 2023-07-17 06:47_

Black only formats one empty line between top level items in stub files (.pyi), while it uses two empty lines in regular python files. When single line classes, method and functions of the same kind that are stubbed out with ellipses follow each they don't get any empty lines.

**scratch.py**

```python
class A:
    def __init__(self):
        pass


class B:
    def __init__(self):
        pass


def foo():
    pass


class Del(expr_context):
    ...


class Load(expr_context):
    ...


class Store(expr_context):
    ...
```

**scratch.pyi**

```python
class A:
    def __init__(self):
        pass

class B:
    def __init__(self):
        pass

def foo():
    pass

class Del(expr_context): ...
class Load(expr_context): ...
class Store(expr_context): ...
```

The relevant code is below, which doesn't have access to whether it's in a stub file or not yet.

https://github.com/astral-sh/ruff/blob/f5f8eb31ed94ee99ce02f7113a5253d1bdb27a69/crates/ruff_python_formatter/src/statement/suite.rs#L72

---

_Label `formatter` added by @konstin on 2023-07-17 06:47_

---

_Comment by @MichaReiser on 2023-07-17 07:34_

Note: This requires knowing the file type during formatting by passing it to the format context.

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-07-31 16:16_

---

_Label `good first issue` added by @konstin on 2023-08-10 08:43_

---

_Label `help wanted` added by @konstin on 2023-08-10 08:43_

---

_Comment by @konstin on 2023-08-10 08:45_

`PyFormatOptions` now has a `source_type` field

---

_Closed by @MichaReiser on 2023-08-15 07:33_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-08-22 14:20_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:20_

---
