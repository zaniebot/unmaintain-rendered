---
number: 8102
title: "Additional Type Hints like `pandas-stubs` for `uv run mypy`"
type: issue
state: closed
author: anzhi0708
labels: []
assignees: []
created_at: 2024-10-10T17:24:38Z
updated_at: 2024-10-11T00:41:24Z
url: https://github.com/astral-sh/uv/issues/8102
synced_at: 2026-01-10T01:24:24Z
---

# Additional Type Hints like `pandas-stubs` for `uv run mypy`

---

_Issue opened by @anzhi0708 on 2024-10-10 17:24_

Let's take `pandas-stubs` as an example. If I don't install it, `mypy` will indicate that some pandas types are unrecognized. 

However, I want to know how to add these additional type hints in a `uv` project and ensure that the tools managed by `uv` can recognize the existence of these type annotations. 
When I use `uv add pandas-stubs`, it is indeed added to the dependencies of my current project, but running `uv run mypy` or `uvx mypy` seems unable to recognize it. 
How should I use type checking within a `uv` environment?

```console
anjiwong@AnjideMacBook-Air ~/d/addr_dup_checker_for_brox (main)> uv tree
Resolved 14 packages in 18ms
addr-dup-checker v0.1.0
├── openpyxl v3.1.5
│   └── et-xmlfile v1.1.0
├── pandas v2.2.3
│   ├── numpy v2.1.2
│   ├── python-dateutil v2.9.0.post0
│   │   └── six v1.16.0
│   ├── pytz v2024.2
│   └── tzdata v2024.2
├── pandas-stubs v2.2.3.241009
│   ├── numpy v2.1.2
│   └── types-pytz v2024.2.0.20241003
├── pyfiglet v1.0.2
└── thefuzz v0.22.1
    └── rapidfuzz v3.10.0
anjiwong@AnjideMacBook-Air ~/d/addr_dup_checker_for_brox (main)> uvx mypy ./main.py
src/order_parser.py:4: error: Library stubs not installed for "pandas"  [import-untyped]
src/order_parser.py:6: error: Library stubs not installed for "pandas.core.groupby"  [import-untyped]
src/duplicate_checker.py:7: error: Library stubs not installed for "pandas"  [import-untyped]
src/duplicate_checker.py:8: error: Cannot find implementation or library stub for module named "thefuzz"  [import-not-found]
main.py:7: error: Library stubs not installed for "pandas"  [import-untyped]
main.py:7: note: Hint: "python3 -m pip install pandas-stubs"
main.py:7: note: (or run "mypy --install-types" to install all missing stub packages)
main.py:7: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
main.py:10: error: Cannot find implementation or library stub for module named "pyfiglet"  [import-not-found]
main.py:214: note: By default the bodies of untyped functions are not checked, consider using --check-untyped-defs  [annotation-unchecked]
main.py:229: note: By default the bodies of untyped functions are not checked, consider using --check-untyped-defs  [annotation-unchecked]
Found 6 errors in 3 files (checked 1 source file)
anjiwong@AnjideMacBook-Air ~/d/addr_dup_checker_for_brox (main) [1]>
```

---

_Comment by @Ravencentric on 2024-10-10 21:31_

In your snippet, you're using `uvx mypy` which is an alias for `uv tool run` (which makes an isolated environment). I think what you're looking for is `uv run mypy ./main.py`.

---

_Comment by @anzhi0708 on 2024-10-11 00:38_

> In your snippet, you're using `uvx mypy` which is an alias for `uv tool run` (which makes an isolated environment). I think what you're looking for is `uv run mypy ./main.py`.

I think it's still not working...

```console
 src  uv tree
Resolved 17 packages in 1ms
addr-dup-checker v0.1.0
├── mypy v1.11.2
│   ├── mypy-extensions v1.0.0
│   └── typing-extensions v4.12.2
├── openpyxl v3.1.5
│   └── et-xmlfile v1.1.0
├── pandas v2.2.3
│   ├── numpy v2.1.2
│   ├── python-dateutil v2.9.0.post0
│   │   └── six v1.16.0
│   ├── pytz v2024.2
│   └── tzdata v2024.2
├── pandas-stubs v2.2.3.241009
│   ├── numpy v2.1.2
│   └── types-pytz v2024.2.0.20241003
├── pyfiglet v1.0.2
└── thefuzz v0.22.1
    └── rapidfuzz v3.10.0
 src  uv run mypy .\order_parser.py
order_parser.py:4: error: Library stubs not installed for "pandas.api.typing"  [import-untyped]
order_parser.py:4: note: Hint: "python3 -m pip install pandas-stubs"
order_parser.py:4: note: (or run "mypy --install-types" to install all missing stub packages)
order_parser.py:4: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
order_parser.py:78: error: Incompatible return value type (got "DataFrameGroupBy[tuple[Any, ...]]", expected "list[tuple[int, DataFrame]]")  [return-value]
Found 2 errors in 1 file (checked 1 source file)
```

---

_Comment by @anzhi0708 on 2024-10-11 00:41_

> In your snippet, you're using `uvx mypy` which is an alias for `uv tool run` (which makes an isolated environment). I think what you're looking for is `uv run mypy ./main.py`.

OK.. Problem solved by replacing import statement to
```python
# from pandas.api.typing import DataFrameGroupBy
from pandas.core.groupby import DataFrameGroupBy
```

Thanks for your reply!

---

_Closed by @anzhi0708 on 2024-10-11 00:41_

---
