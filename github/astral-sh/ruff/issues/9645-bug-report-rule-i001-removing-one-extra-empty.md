---
number: 9645
title: "Bug report : rule \"I001\" removing one extra empty line after imports"
type: issue
state: closed
author: Lenormju
labels:
  - isort
assignees: []
created_at: 2024-01-26T09:25:10Z
updated_at: 2024-05-08T16:08:09Z
url: https://github.com/astral-sh/ruff/issues/9645
synced_at: 2026-01-07T13:12:15-06:00
---

# Bug report : rule "I001" removing one extra empty line after imports

---

_Issue opened by @Lenormju on 2024-01-26 09:25_

<!--


* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
-->



# Actual

Code to reproduce :

```python
import math


PI = math.pi
```

Ruff output :

```sh
minimal_reproducible_example.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

Applying the fix removes one empty line between imports and variable declaration :

```diff
 import math

-                    
 PI = math.pi
```

# Expected

The Ruff error message is `I001 [*] Import block is un-sorted or un-formatted`.
Per [PEP 8](https://peps.python.org/pep-0008/) regarding [blank lines](https://peps.python.org/pep-0008/#blank-lines) :
> Surround top-level function and class definitions with two blank lines.

I don't think that the import block is un-formatted, there is just maybe an extraneous line after it.

I quickly surveyed [code from CPython's stdlib](https://github.com/python/cpython/tree/main/Lib) and there is both 1 or 2 spaces between the last line of imports and the first declaration after.

I ran Black (version `24.1.0`) on the same file and it did not remove a blank line.

# Context

Ruff version : `ruff 0.1.14`

Ruff settings :

* `pyproject.toml`
  ```toml
  [tool.ruff]
  select = ["I",]
  ```

Ruff command : `ruff check minimal_reproducible_example.py` (if `--isolated` the bug is not reproduced)

---

_Comment by @MichaReiser on 2024-01-26 10:04_

Thanks for the detailed error report. To consistently enforce two lines after imports, you can use `isort.lines-after-imports = 2'.

I'm unfamiliar with the logic used by `isort` when using `lines-after-imports = -1`. @charliermarsh should know the details.


---

_Label `isort` added by @MichaReiser on 2024-01-26 10:04_

---

_Comment by @Lenormju on 2024-01-26 10:42_

Indeed !
* `isort --check file.py` prints an error (`Imports are incorrectly sorted and/or formatted.`)
* `isort file.py` removes the line, like Ruff currently does
* `isort --lines-after-import 2 --check file.py` reports no error
* `isort --lines-after-import 1 --check file.py` removes the line

So the default behavior of Ruff matches the default behavior of isort.

If I specify [the value in the `[tool.ruff.lint.isort]` section](https://docs.astral.sh/ruff/settings/#isort-lines-after-imports) of the `pyproject.toml` :
* `lines-after-imports = -1` removes the line
* `lines-after-imports = 1` removes the line
* `lines-after-imports = 2` does _not_ remove the line

---

_Comment by @MichaReiser on 2024-01-27 16:47_

 @Lenormju I close this issue because what I understand from your response is that your problem is resolved. Let me know if that's not the case or if there's something we could improve. 

---

_Closed by @MichaReiser on 2024-01-27 16:47_

---

_Comment by @Tedpac on 2024-05-08 16:08_

In my case, I'm using the Visual Studio Code extensions for [Ruff](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff) and [isort](https://marketplace.visualstudio.com/items?itemName=ms-python.isort), and I solved this issue by adding `"isort.args": ["--lines-after-import", "2"]` to `settings.json`. It should be noted that my decision to use both extensions was not to solve this issue but a personal choice. ✌️

---
