```yaml
number: 16266
title: Playground counts each non-BMP code point as two code points
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - playground
assignees: []
created_at: 2025-02-19T21:04:12Z
updated_at: 2025-09-24T08:08:01Z
url: https://github.com/astral-sh/ruff/issues/16266
synced_at: 2026-01-10T11:09:57Z
```

# Playground counts each non-BMP code point as two code points

---

_Issue opened by @dscorbett on 2025-02-19 21:04_

### Description

The playground counts each non-BMP code point as two code points when applying fixes, leaving characters there that should have been replaced as part of the fix. [Here is an example.](https://play.ruff.rs/8aef5a51-fa9e-4d87-9634-537659f3c65a) The input is:
```python
dict(𠤪=1)
```
Applying the quick fix for [`unnecessary-collection-call` (C408)](https://docs.astral.sh/ruff/rules/unnecessary-collection-call/) incorrectly produces:
```python
{"𠤪": 1})
```
On the command line, though, Ruff 0.9.6 correctly fixes it to:
```python
{"𠤪": 1}
```

---

_Label `playground` added by @MichaReiser on 2025-02-19 21:12_

---

_Label `bug` added by @MichaReiser on 2025-02-19 21:12_

---

_Closed by @MichaReiser on 2025-09-24 08:08_

---
