```yaml
number: 4414
title: "[Autofix error] SyntaxError from RUF010"
type: issue
state: closed
author: janosh
labels:
  - bug
assignees: []
created_at: 2023-05-13T15:32:55Z
updated_at: 2023-05-15T02:08:31Z
url: https://github.com/astral-sh/ruff/issues/4414
synced_at: 2026-01-10T11:09:47Z
```

# [Autofix error] SyntaxError from RUF010

---

_Issue opened by @janosh on 2023-05-13 15:32_

Applying `ruff --fix` (v0.0.267) to [this line](https://github.com/materialsproject/pymatgen/blob/8fded9b450fb1952d3d915adebff2e551d69c360/pymatgen/io/packmol.py#L217)

```py
file_contents += f"  number {str(d['number'])}\n"
```

causes

> error: Autofix introduced a syntax error. Reverting all changes.

The attempted fix is

> pymatgen/io/packmol.py:217:42: RUF010 Use conversion in f-string

Workaround is to manually change to

```py
file_contents += f"  number {d['number']!s}\n"
```

---

_Comment by @JonathanPlasse on 2023-05-13 16:36_

I can work on this.

---

_Label `bug` added by @charliermarsh on 2023-05-13 17:39_

---

_Closed by @charliermarsh on 2023-05-15 02:08_

---
