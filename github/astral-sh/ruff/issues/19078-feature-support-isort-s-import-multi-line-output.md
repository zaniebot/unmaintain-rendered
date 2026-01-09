---
number: 19078
title: "Feature: Support iSort's import Multi Line Output Mode 2 - Hanging Indent"
type: issue
state: closed
author: Bengt
labels:
  - isort
  - wish
assignees: []
created_at: 2025-07-01T21:25:55Z
updated_at: 2025-07-07T13:01:11Z
url: https://github.com/astral-sh/ruff/issues/19078
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature: Support iSort's import Multi Line Output Mode 2 - Hanging Indent

---

_Issue opened by @Bengt on 2025-07-01 21:25_

### Summary

I want to optimize my ruff configuration to generate minimal diffs. This makes reviewing changesets easier as there is less scrolling distance, especially when handling codebases with many small files and therefore many imports. To get single-line diffs for single-line changes, I want to use one line per import:

```python
from utils import func1
from utils import func2
```

I can achieve that today by setting `force-single-line = true` in my PyProject.toml under `[tool.ruff.lint.isort]`. However, this breaks, when I have long distribution, package, module, or function names like so:

```python
from my_long_distribution_name.my_long_package_name.my_long_module_name import my_long_function_name
```

Ruff will format this like so, generating a 3-line diff:

```python
from my_long_distribution_name.my_long_package_name.my_long_module_name import (
    my_long_function_name,
)
```

This is a pity, as the diff would only need to be two lines if ruff supported isort's mode 2 "Hanging Indent":

```python
from my_long_distribution_name.my_long_package_name.my_long_module_name import \
    my_long_function_name
```

Note that implementing this is a bit tricky, as we need to adhere to the line length while inserting the backslash character. The above line is fine with ruff's default line length of 88 characters, as it is 81 characters long. If one were to set `line-length = 80` under `[tool.ruff]` in `pyproject.toml`, the correct solution would be to split the line before the `import`:

```python
from my_long_distribution_name.my_long_package_name.my_long_module_name \
    import my_long_function_name
```

If I were to set `line-length = 73`, we would even need to break the line at the dot operator before the module name:

```python
from my_long_distribution_name.my_long_package_name.\
    my_long_module_name import my_long_function_name
```


---

_Comment by @Bengt on 2025-07-01 21:29_

There is also an open request for more general support of isort's import multi line output mode: <https://github.com/astral-sh/ruff/issues/2600>

This is my case for supporting mode 2 in particular, as it combines nicely with ruff's preexisting `force-single-line` option.

---

_Comment by @Bengt on 2025-07-01 21:45_

As a workaround, one can deactivate ruff's import sorting rules and configure isort to do the same in `pyproject.toml`:

```toml
[tool.isort]
multi_line_output = 2
force_single_line = true

[tool.ruff.lint]
ignore = [
    "I001",
]
```

---

_Label `isort` added by @ntBre on 2025-07-01 22:32_

---

_Label `wish` added by @ntBre on 2025-07-01 22:34_

---

_Comment by @MichaReiser on 2025-07-07 13:01_

Your desired style, unfortunately, is incompatible with the formatter. It reformats

```py
from my_long_distribution_name.my_long_package_name.my_long_module_name import \
    my_long_function_name
```

to

```py
from my_long_distribution_name.my_long_package_name.my_long_module_name import (
    my_long_function_name,
)

```

This makes it very unlikely that we'll support this mode (tool compatibility is important to us).

I'll merge this into https://github.com/astral-sh/ruff/issues/2600 as it asks for a sub feature of it


---

_Closed by @MichaReiser on 2025-07-07 13:01_

---
