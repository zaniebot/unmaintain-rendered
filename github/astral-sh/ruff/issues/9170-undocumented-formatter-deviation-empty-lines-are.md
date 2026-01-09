---
number: 9170
title: Undocumented formatter deviation - empty lines are removed from block beginning
type: issue
state: closed
author: jond01
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-12-17T16:01:17Z
updated_at: 2023-12-20T05:11:27Z
url: https://github.com/astral-sh/ruff/issues/9170
synced_at: 2026-01-07T13:12:15-06:00
---

# Undocumented formatter deviation - empty lines are removed from block beginning

---

_Issue opened by @jond01 on 2023-12-17 16:01_

* A minimal code snippet that reproduces the bug:
Original `.py` file:
```py
def f():

    pass

```
Black - unchanged.

Ruff:
```py
def f():
    pass

```

The same happens with if statements, for loops, class declarations, etc.

* The command you invoked: `ruff format`.
* The current Ruff settings: nothing relevant.
* The current Ruff version: 0.1.8

More examples are here:
https://github.com/mlrun/mlrun/pull/4748
* https://github.com/mlrun/mlrun/pull/4748/files#diff-277e10f57c688505214127056ff0b8b38c04197562f1fff844ddbd7700e69ec7
* https://github.com/mlrun/mlrun/pull/4748/files#diff-eab0b5a3c6ae8610645aa6cd8e98d6ada4b71820a550b0ea8c10588f62f018ae
* https://github.com/mlrun/mlrun/pull/4748/files#diff-85933aa74a2d66c3e4dcdf7a9ad8397f5a7971080d34ef1108296a7c6b69e7e3

---

_Label `bug` added by @MichaReiser on 2023-12-18 00:41_

---

_Label `formatter` added by @MichaReiser on 2023-12-18 00:41_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-12-18 00:41_

---

_Comment by @charliermarsh on 2023-12-20 05:00_

Are you using Black in preview mode? As far as I can tell, Black does indeed remove the blank line here: https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4AB7AE9dAD2IimZxl1N_WlbvK5V9Hvo0zZlkjkY7jWHIgmdR0rHlIb74KGjor5TXSOaGv8qKEVysQM7wuFOHgrCWK8Ui7FQA2zwGzHKOlacpv7iYIwAAAIh9HcwrvN0JAAFrfFA6PP8ftvN9AQAAAAAEWVo=

---

_Comment by @charliermarsh on 2023-12-20 05:11_

Confirming that this is part of preview, so it's a case of https://github.com/astral-sh/ruff/issues/8893. Merging into that issue, thanks!

---

_Closed by @charliermarsh on 2023-12-20 05:11_

---
