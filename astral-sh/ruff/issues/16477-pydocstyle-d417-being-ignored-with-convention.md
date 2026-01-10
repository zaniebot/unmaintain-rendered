---
number: 16477
title: "[pydocstyle]: d417 being ignored with convention=google"
type: issue
state: closed
author: sjhw
labels:
  - question
  - great writeup
assignees: []
created_at: 2025-03-03T18:21:54Z
updated_at: 2025-03-04T17:49:53Z
url: https://github.com/astral-sh/ruff/issues/16477
synced_at: 2026-01-10T01:22:57Z
---

# [pydocstyle]: d417 being ignored with convention=google

---

_Issue opened by @sjhw on 2025-03-03 18:21_

Hello ruff team! Thank you for making such an incredible tool, I've found my python project management experience has skyrocketed in ease since incorporating this into my organization's main Python repo.

I read from #15065 that `convention=google` is the only way that D417 is supported, but I have enabled this convention and am still struggling to get the check to work as intended.

I'm a junior dev so this is my first time really diving into Python project management and style enforcement, so it's entirely possible I may just be missing something obvious in the way that the pydocstyle checks are working, or maybe even what D417 truly is, in which case I apologize for the inconvenience and appreciate any insight you could provide. Thank you for your time!

## Summary
When using Ruff to enforce pydocstyle checks, I cannot get the check to identify that a function has undocumented params. It seems to pass the check no matter what. I hope to use this as a pre-commit check to prevent new code additions from being committed without properly having docstrings with explanations for all function arguments.

## Example
My `pyproject.toml` contains the following config:
```toml
[tool.ruff]
line-length = 79 # 88
fix = true
show-fixes = true
lint.select = [
    "I",
    "D",  # enables pydocstyle checks
]

[tool.ruff.lint.pydocstyle]
convention = "google"
```

Here is a module, `mystery_module.py`
```python
"""Test module for ruff pydocstyle checks."""


def mystery_function(mystery_int: int):
    """Return a mysterious number."""
    mystery_int += 1
    return mystery_int

```

When checking this module with Ruff, I get the following message in the console:
```terminal
> ruff check mystery_module.py 
All checks passed!
```

When I delete the docstring in `mystery_function` altogether, Ruff is correctly able to identify the consequential D103 violation resulting from a public function with a missing docstring:
```python
"""Test module for ruff pydocstyle checks."""

import numpy as np


def mystery_function(mystery_int: int):
    mystery_int += 1
    return mystery_int
```

```terminal
> ruff check mystery_module.py 
mystery_module.py:6:5: D103 Missing docstring in public function
  |
6 | def mystery_function(mystery_int: int):
  |     ^^^^^^^^^^^^^^^^ D103
7 |     mystery_int += 1
8 |     return mystery_int
  |

Found 1 error.
```


When running with `--isolated`, the check correctly identifies unused import Numpy (which I intentionally added to ensure that Ruff is otherwise working as intended)
```terminal
> ruff check mystery_module.py --isolated
mystery_module.py:2:17: F401 [*] `numpy` imported but unused
  |
1 | """Test module for ruff pydocstyle checks."""
2 | import numpy as np
  |                 ^^ F401
  |
  = help: Remove unused import: `numpy`

Found 1 error.
[*] 1 fixable with the `--fix` option.
```

## Expected
Because the function `mystery_function` uses an argument `mystery_int` that isn't properly documented, I want Ruff to be able to flag this as a D417 violation and prevent the commit from being made, which isn't happening currently.

### Version

0.9.9

---

_Label `great writeup` added by @MichaReiser on 2025-03-04 09:56_

---

_Label `question` added by @MichaReiser on 2025-03-04 10:14_

---

_Comment by @MichaReiser on 2025-03-04 10:17_

Thanks for the kind words and the great write up! 

`D417` only flags missing arguments if the docstring has an `Args` (or `Other args`, `keyword args`...) section. It doesn't flag docstrings that have no documented arguments. 

I think what you want is the stricter https://github.com/astral-sh/ruff/pull/13280 that flags all functions regardless if they have an `Args` section or not if any of the parameters are undocumented. 

I do think that we could improve our documentation on D417, it took me a while to understand what the issue was

---

_Referenced in [astral-sh/ruff#16494](../../astral-sh/ruff/pulls/16494.md) on 2025-03-04 10:21_

---

_Comment by @sjhw on 2025-03-04 14:26_

Thank you so much for your response Micha! The stricter #13280 is indeed exactly what I was looking for here, it seems I was simply under a misunderstanding as to what `D417` truly was checking for. Thank you very much!

---

_Closed by @sjhw on 2025-03-04 14:26_

---
