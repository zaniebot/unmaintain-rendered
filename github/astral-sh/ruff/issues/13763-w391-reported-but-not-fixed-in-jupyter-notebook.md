---
number: 13763
title: W391 reported but not fixed in Jupyter notebook
type: issue
state: closed
author: DimitriPapadopoulos
labels:
  - bug
  - notebook
assignees: []
created_at: 2024-10-15T14:44:58Z
updated_at: 2025-01-09T16:50:42Z
url: https://github.com/astral-sh/ruff/issues/13763
synced_at: 2026-01-07T13:12:15-06:00
---

# W391 reported but not fixed in Jupyter notebook

---

_Issue opened by @DimitriPapadopoulos on 2024-10-15 14:44_

Ruff emits a **W391** error, a spurious one as far as I can see, and `--fix` doesn't modify the file:
```console
$ ruff check --isolated --no-cache --extend-select W --preview --fix file.ipynb
Found 1 error (1 fixed, 0 remaining).
$ 
$ ruff check --isolated --no-cache --extend-select W --preview --fix file.ipynb
Found 1 error (1 fixed, 0 remaining).
$ 
```
The issue is associated to this line in the Jupyter Notebook:
```json
   "source": []
```

* List of keywords you searched for before creating this issue: "W391"
* A minimal code snippet that reproduces the bug.
  <details>
  <summary>code snippet</summary>

  ```json
  {
   "cells": [
    {
     "cell_type": "code",
     "execution_count": null,
     "metadata": {},
     "outputs": [
      {
       "name": "stdout",
       "output_type": "stream",
       "text": [
        "True\n"
       ]
      }
     ],
     "source": [
      "True"
     ]
    },
    {
     "cell_type": "code",
     "execution_count": null,
     "metadata": {},
     "outputs": [],
     "source": []
    }
   ],
   "metadata": {
    "kernelspec": {
     "display_name": "Python 3 (ipykernel)",
     "language": "python",
     "name": "python3"
    }
   },
   "nbformat": 4,
   "nbformat_minor": 2
  }
  
   ```
  </details>
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
  ```
   ruff check --isolated --no-cache --extend-select W --preview --fix file.ipynb
   ```
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
  ```
  ruff 0.6.9
  ```

---

_Renamed from "W391 not fixed in Jupyter notebook" to "W391 reported but not fixed in Jupyter notebook" by @DimitriPapadopoulos on 2024-10-15 14:50_

---

_Label `bug` added by @dhruvmanila on 2024-10-16 05:54_

---

_Label `notebook` added by @dhruvmanila on 2024-10-16 05:54_

---

_Comment by @dhruvmanila on 2024-10-16 05:56_

Yeah, I think we should limit this rule to individual cells and not for the concatenated content.

---

_Comment by @dhruvmanila on 2024-10-16 05:59_

The challenge here is that the rule looks at the tokens, so we'd need to basically run the `too_many_newlines_at_end_of_file` function for the tokens corresponding to each cell. I think that can be done by using the `CellOffsets`. We'd need to keep an iterator of cell offsets and loop over all the tokens of the file in reverse order and reset some state to indicate that we're at the end of the cell. This would be similar to what's being done for blank line rules by the `LinePreprocessor`.

---

_Referenced in [astral-sh/ruff#15308](../../astral-sh/ruff/pulls/15308.md) on 2025-01-06 23:21_

---

_Closed by @dylwil3 on 2025-01-09 16:50_

---

_Closed by @dylwil3 on 2025-01-09 16:50_

---
