```yaml
number: 6581
title: "Formatter: don't break single-argument multi-line strings if the first line \"fits\""
type: issue
state: closed
author: charliermarsh
labels:
  - formatter
  - needs-decision
assignees: []
created_at: 2023-08-15T03:01:04Z
updated_at: 2023-09-02T08:21:57Z
url: https://github.com/astral-sh/ruff/issues/6581
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: don't break single-argument multi-line strings if the first line "fits"

---

_@charliermarsh_

See: https://github.com/psf/black/pull/1879/files.

In Preview, Black now reformats:

```python
textwrap.dedent(
    """
    This is a
    multiline string
"""
)
```

to:

```python
textwrap.dedent("""
    This is a
    multiline string
""")
```

https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AGlAIldAD2IimZxl1N_WmhEpla9DGFkUapmMYaVDChQFFzFY8lTc9V-psgZJEaxBwinapUZzdJvX8NXjM1PIFkv5tdgxiTStTWHDKc-RWzWjZvcHL8-afLVYsHq6y_3a6xN_lELxXHP-HR-0jzuepYl65gRWE-um2cLzTZWAycQpu0Bz_5igXnUT4SeJh8AAAAAAHz0sc5o1W6jAAGlAaYDAACwhaLdscRn-wIAAAAABFla

---

_Label `formatter` added by @charliermarsh on 2023-08-15 03:01_

---

_Comment by @charliermarsh on 2023-08-15 03:04_

(This is part of preview style, so perhaps not required for now.)

---

_Comment by @charliermarsh on 2023-08-15 03:10_

@MichaReiser -- do we have a general opinion on preview style? My assumptions:

- If we implement formatting that _does_ match preview style (but doesn't match Black's stable formatting), that's okay.
- However, preview style is currently a nice-to-have and not required to ship.


---

_Comment by @MichaReiser on 2023-08-15 05:09_

Preview style isn't a priority right now. We can implement it but we need to add a preview option first and gate the logic accordingly 

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-17 09:44_

---

_Label `needs-decision` added by @MichaReiser on 2023-08-22 15:07_

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 15:07_

---

_Comment by @MichaReiser on 2023-09-02 08:21_

Tracked as part of [#6935](https://github.com/astral-sh/ruff/issues/6935)

---

_Closed by @MichaReiser on 2023-09-02 08:21_

---
