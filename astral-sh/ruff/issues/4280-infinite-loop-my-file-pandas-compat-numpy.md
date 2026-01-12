```yaml
number: 4280
title: "[Infinite loop] My File: pandas\\compat\\numpy\\function.py"
type: issue
state: closed
author: FishAlchemist
labels:
  - bug
assignees: []
created_at: 2023-05-08T12:05:21Z
updated_at: 2023-05-08T19:33:31Z
url: https://github.com/astral-sh/ruff/issues/4280
synced_at: 2026-01-12T15:54:44Z
```

# [Infinite loop] My File: pandas\compat\numpy\function.py

---

_@FishAlchemist_

## Version(ruff --version)
**ruff 0.0.265**
## Command(Powershell)
```powershell
ruff check --select=ALL .\function.py
```
**If you add --isolated , there will be no problem about Infinite loop**
## Terminal Content
```powershell
warning: `one-blank-line-before-class` (D203) and `no-blank-line-before-class` (D211) are incompatible. Ignoring `one-blank-line-before-class`.
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.

error: Failed to converge after 100 iterations.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `function.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

function.py:1:1: D205 1 blank line required between summary line and description
function.py:49:7: D101 Missing docstring in public class
function.py:50:9: D107 Missing docstring in `__init__`
function.py:51:9: ANN101 Missing type annotation for `self` in method
function.py:52:9: ANN001 Missing type annotation for function argument `defaults`
function.py:53:9: ANN001 Missing type annotation for function argument `fname`
function.py:55:9: ANN001 Missing type annotation for function argument `max_fname_arg_count`
function.py:62:9: PLR0913 Too many arguments to function call (6 > 5)
function.py:62:9: D102 Missing docstring in public method
function.py:63:9: ANN101 Missing type annotation for `self` in method
function.py:64:9: ANN001 Missing type annotation for function argument `args`
function.py:65:9: ANN001 Missing type annotation for function argument `kwargs`
function.py:66:9: ANN001 Missing type annotation for function argument `fname`
function.py:67:9: ANN001 Missing type annotation for function argument `max_fname_arg_count`
function.py:103:5: D103 Missing docstring in public function
function.py:103:51: ANN001 Missing type annotation for function argument `args`
function.py:111:64: ANN001 Missing type annotation for function argument `args`
function.py:111:70: ANN001 Missing type annotation for function argument `kwargs`
function.py:112:5: D205 1 blank line required between summary line and description
function.py:122:64: ANN001 Missing type annotation for function argument `args`
function.py:122:70: ANN001 Missing type annotation for function argument `kwargs`
function.py:123:5: D205 1 blank line required between summary line and description
function.py:154:67: ANN001 Missing type annotation for function argument `args`
function.py:154:73: ANN001 Missing type annotation for function argument `kwargs`
function.py:155:5: D205 1 blank line required between summary line and description
function.py:176:44: ANN001 Missing type annotation for function argument `args`
function.py:176:50: ANN001 Missing type annotation for function argument `kwargs`
function.py:181:46: ANN001 Missing type annotation for function argument `args`
function.py:181:52: ANN001 Missing type annotation for function argument `kwargs`
function.py:188:5: D205 1 blank line required between summary line and description
function.py:216:35: FBT001 Boolean positional arg in function definition
function.py:216:49: ANN001 Missing type annotation for function argument `args`
function.py:216:55: ANN001 Missing type annotation for function argument `kwargs`
function.py:216:63: ANN001 Missing type annotation for function argument `name`
function.py:217:5: D205 1 blank line required between summary line and description
function.py:320:64: ANN001 Missing type annotation for function argument `args`
function.py:320:70: ANN001 Missing type annotation for function argument `kwargs`
function.py:321:5: D205 1 blank line required between summary line and description
function.py:339:38: ANN001 Missing type annotation for function argument `args`
function.py:339:44: ANN001 Missing type annotation for function argument `kwargs`
function.py:339:52: ANN001 Missing type annotation for function argument `allowed`
function.py:340:5: D205 1 blank line required between summary line and description
function.py:340:5: D403 First word of the first line should be capitalized: `'args'` -> `'args'`
function.py:350:89: E501 Line too long (96 > 88 characters)
function.py:359:42: ANN001 Missing type annotation for function argument `args`
function.py:359:48: ANN001 Missing type annotation for function argument `kwargs`
function.py:360:5: D205 1 blank line required between summary line and description
function.py:360:5: D403 First word of the first line should be capitalized: `'args'` -> `'args'`
function.py:365:89: E501 Line too long (104 > 88 characters)
function.py:373:5: D417 Missing argument descriptions in the docstring: `axis`, `ndim`
function.py:374:5: D205 1 blank line required between summary line and description
function.py:403:5: D103 Missing docstring in public function
function.py:403:19: ANN001 Missing type annotation for function argument `fname`
function.py:403:26: ANN001 Missing type annotation for function argument `args`
function.py:403:32: ANN001 Missing type annotation for function argument `kwargs`
Found 256 errors (201 fixed, 55 remaining).
```
### Command(Powershell): --isolated
```powershell
ruff check --select=ALL --isolated .\function.py
```
[full_terminal_content_isolated.txt](https://github.com/charliermarsh/ruff/files/11420830/full_terminal_content_isolated.txt)
## Python Code
**Please change the file extension from txt to py**
[function.txt](https://github.com/charliermarsh/ruff/files/11420869/function.txt)
## pyproject.toml
**Please change the file extension from txt to toml**
[pyproject.txt](https://github.com/charliermarsh/ruff/files/11420870/pyproject.txt)




---

_Renamed from "[Infinite loop] pandas\compat\numpy\function.py" to "[Infinite loop] My File: pandas\compat\numpy\function.py" by @FishAlchemist on 2023-05-08 12:08_

---

_Comment by @MichaReiser on 2023-05-08 12:35_

This is an issue with the rule D403. It fails to stabalize if no uppercase variant exists for the first character of the first word. 

We should either not flag the rule if no uppercase version of the first character of the first word exists, or not provide an autofix. 

```
Failed to converge after 10 iterations. This likely indicates a bug in the implementation of the fix. Last diagnostics:
D403-test.py:340:5: D403 [*] First word of the first line should be capitalized: `'args'` -> `'args'`
    |
340 |   def validate_groupby_func(name: str, args, kwargs, allowed=None) -> None:
341 |       """'args' and 'kwargs' should be empty, except for allowed kwargs because all
    |  _____^
342 | |     of their necessary parameters are explicitly listed in the function
343 | |     signature.
344 | |     """
    | |_______^ D403
345 |       if allowed is None:
346 |           allowed = []
    |
    = help: Capitalize `'args'` to `'args'`

ℹ Suggested fix

D403-test.py:360:5: D403 [*] First word of the first line should be capitalized: `'args'` -> `'args'`
    |
360 |   def validate_resampler_func(method: str, args, kwargs) -> None:
361 |       """'args' and 'kwargs' should be empty because all of their necessary
    |  _____^
362 | |     parameters are explicitly listed in the function signature.
363 | |     """
    | |_______^ D403
364 |       if len(args) + len(kwargs) > 0:
365 |           if method in RESAMPLER_NUMPY_OPS:
    |
    = help: Capitalize `'args'` to `'args'`

```

---

_Label `bug` added by @MichaReiser on 2023-05-08 12:36_

---

_Comment by @dhruvmanila on 2023-05-08 13:27_

> We should either not flag the rule if no uppercase version of the first character of the first word exists, or not provide an autofix.

I think the former makes sense!

---

_Closed by @charliermarsh on 2023-05-08 19:33_

---
