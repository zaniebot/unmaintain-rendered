---
number: 6938
title: "Formatter: Multiline string followed by call expression"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-08-28T11:50:12Z
updated_at: 2023-09-02T08:23:07Z
url: https://github.com/astral-sh/ruff/issues/6938
synced_at: 2026-01-07T13:12:15-06:00
---

# Formatter: Multiline string followed by call expression

---

_Issue opened by @MichaReiser on 2023-08-28 11:50_

I create this issue to document our decision on call expression formatting that directly follows a multiline string.

This is how stable black formats a multiline string

```Python
sql = """
INSERT INTO {0}_pony (pink, weight) VALUES (1, 3.55);
INSERT INTO {0}_pony (pink, weight) VALUES (3, 5.0);
""".format(
    app_label
)
```

But Ruff collapses the `format` call if it fits on the line

```python
sql = """
INSERT INTO {0}_pony (pink, weight) VALUES (1, 3.55);
INSERT INTO {0}_pony (pink, weight) VALUES (3, 5.0);
""".format(app_label)
```

Ruff's formatting matches Black's preview [*improve multiline string handling*](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-multiline-string-handling) formatting. 

Ruff will only support the preview formatting because implementing the non-preview style requires specialised formatting that doesn't seem justified considering that this style will change soon anyway (and seems better IMO).

---

_Label `formatter` added by @MichaReiser on 2023-08-28 11:50_

---

_Closed by @MichaReiser on 2023-08-28 11:50_

---

_Referenced in [astral-sh/ruff#6069](../../astral-sh/ruff/issues/6069.md) on 2023-08-29 12:26_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-09-02 08:23_

---

_Referenced in [astral-sh/ruff#8388](../../astral-sh/ruff/issues/8388.md) on 2023-10-31 21:52_

---
