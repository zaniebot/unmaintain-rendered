```yaml
number: 12686
title: Is it possible for ruff to preserve the multi-line list comprehension for readability?
type: issue
state: closed
author: jd-solanki
labels: []
assignees: []
created_at: 2024-08-05T11:43:17Z
updated_at: 2024-08-05T12:44:41Z
url: https://github.com/astral-sh/ruff/issues/12686
synced_at: 2026-01-12T15:54:52Z
```

# Is it possible for ruff to preserve the multi-line list comprehension for readability?

---

_@jd-solanki_

Hi 👋🏻 

Big fan of Ruff & Sister projects ❤️ 

Can we preserve the multi-line list comprehension for readability?

```py
    return [
        data
        for data in sku_data
        if data.date >= today.replace(month=today.month - duration_in_months)
    ]
```

With ruff it's get formatted in single line:
```py
return [data for data in sku_data if data.date >= today.replace(month=today.month - duration_in_months)]
```

My ruff config:
```toml
# Docs: https://docs.astral.sh/ruff/settings/
select = ["ALL"]
fix = true
target-version = "py312"
line-length = 120

ignore = [
    "D101",   # Missing docstring in public class
    "D104",   # Missing docstring in public package
    "D100",   # Missing docstring in public module
    "ANN201", # Missing return type annotation for public function `get_db`
    "D103",   # Missing docstring in public function
    "INP001", # File `X` is part of an implicit namespace package. Add an `__init__.py`.
    "D102",   # Missing docstring in public method
    "D107",   # Missing docstring in `__init__`
    "ANN101", # Missing type annotation for `self` in method
    "D106",   # Missing docstring in public nested class
    "ERA001", # Found commented-out code
    "T201",   # `print` found

    # Comments
    "TD002", # Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
    "TD003", # Missing issue link on the line following this TODO

    # FastAPI only
    "B008", # Do not perform function call `Depends` in argument defaults
]
[extend-per-file-ignores]
"tests/**" = ["S101", "PLR2004"]

[format]
skip-magic-trailing-comma = false
```

---

_Comment by @MichaReiser on 2024-08-05 12:44_

Thanks for the kind words and reporting the improvement. 

This has come up before. I'll merge the issue into https://github.com/astral-sh/ruff/issues/11753

---

_Closed by @MichaReiser on 2024-08-05 12:44_

---
