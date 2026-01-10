---
number: 7460
title: "Formatter instability: String normalization inserts whitespace"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-09-17T15:00:13Z
updated_at: 2023-09-18T10:28:17Z
url: https://github.com/astral-sh/ruff/issues/7460
synced_at: 2026-01-10T01:22:47Z
---

# Formatter instability: String normalization inserts whitespace

---

_Issue opened by @MichaReiser on 2023-09-17 15:00_

## Input

```python
value_text  = ''' value=""'''
```

## Format 1
Which gets normalized to which isn't the same as the input

```python
value_text = """ value="""""
```

## Format 2
Inserts a space between the implicit concatenated strings

```python
value_text = """ value=""" ""
```

The real issue is why the string gets normalized to `Format 1`, It should keep the single quotes.

Sourced by #7445 


---

_Label `bug` added by @MichaReiser on 2023-09-17 15:00_

---

_Label `formatter` added by @MichaReiser on 2023-09-17 15:00_

---

_Label `help wanted` added by @MichaReiser on 2023-09-17 15:00_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-17 15:00_

---

_Referenced in [astral-sh/ruff#7485](../../astral-sh/ruff/pulls/7485.md) on 2023-09-18 09:14_

---

_Closed by @konstin on 2023-09-18 10:28_

---
