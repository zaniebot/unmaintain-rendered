```yaml
number: 16475
title: Ignore trailing commas not working for function arguments
type: issue
state: closed
author: sparxastronomy
labels:
  - question
assignees: []
created_at: 2025-03-03T14:47:52Z
updated_at: 2025-03-03T15:11:46Z
url: https://github.com/astral-sh/ruff/issues/16475
synced_at: 2026-01-10T11:09:57Z
```

# Ignore trailing commas not working for function arguments

---

_Issue opened by @sparxastronomy on 2025-03-03 14:47_

### Summary

I have the following function block:
```
def compute_profiles(
        x: np.ndarray, 
        y: np.ndarray, 
        window_size: int, 
        type: str = "median", 
        return_xbins: bool = False
) -> tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
```

However ruff ignores the trailing commas, and formats it to  even though  `skip-magic-trailing-comma = false`
```
def compute_profiles(
    x: np.ndarray, y: np.ndarray, window_size: int, type: str = "median", return_xbins: bool = False
) -> tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]:
```

The problem only appears for functions, for regular arrays the problem does not appear. 


My configuration is:
```
[tool.ruff]
extend-include = ["*.ipynb"]
preview = true
line-length = 120
indent-width = 4

[tool.ruff.format]
preview = true
quote-style = "double"
indent-style = "space"
line-ending = "auto"
skip-magic-trailing-comma = false
docstring-code-format = true
docstring-code-line-length = "dynamic"

[tool.ruff.lint]
select = [
    "E", # pycodestyle (error)
    "F", # pyflakes
    "B", # bugbear
    "B9",
    "C4", # flake8-comprehensions
    "SIM", # flake8-simplify
    "I", # isort
    "UP", # pyupgrade
    "PIE", # flake8-pie
    "PGH", # pygrep-hooks
    "PYI", # flake8-pyi
    "RUF",
]
ignore = [
    # only relevant if you run a script with `python -0`,
    # which seems unlikely for any of the scripts in this repo
    "B011",
    # Leave it to the formatter to split long lines and
    # the judgement of all of us.
    "E501",
    "RUF052",
    "E201",
    "E241"
]

fixable = ["A", "B", "C", "D", "E", "F"]
unfixable = []

[tool.ruff.lint.pydocstyle]
convention = "google"
```


I am using the latest version of Ruff extension on VSCode. 

### Version

v2025.14.0 (VScode)

---

_Label `question` added by @MichaReiser on 2025-03-03 14:52_

---

_Comment by @MichaReiser on 2025-03-03 14:54_

Hi @sparxastronomy 

I don't see a trailing comma in your example code. The trailing comma must come after the last argument's default value:

```py
def compute_profiles(
        x: np.ndarray, 
        y: np.ndarray, 
        window_size: int, 
        type: str = "median", 
        return_xbins: bool = False,
) -> tuple[np.ndarray, np.ndarray, np.ndarray, np.ndarray]: ...
```

Ruff should then split the arguments across multiple lines when using `skip-magic-trailing-comma=false` (default)

https://play.ruff.rs/10014487-0683-40d5-9eed-968075bce9b5

---

_Comment by @sparxastronomy on 2025-03-03 15:11_

Thanks @MichaReiser for point out the missing commas. I thought the last comma was not needed.

---

_Closed by @sparxastronomy on 2025-03-03 15:11_

---
