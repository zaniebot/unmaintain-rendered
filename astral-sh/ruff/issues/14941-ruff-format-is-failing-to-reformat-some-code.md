```yaml
number: 14941
title: "`ruff format` is failing  to reformat some code"
type: issue
state: closed
author: samuelcolvin
labels:
  - question
assignees: []
created_at: 2024-12-12T18:31:55Z
updated_at: 2024-12-12T18:44:40Z
url: https://github.com/astral-sh/ruff/issues/14941
synced_at: 2026-01-10T11:09:56Z
```

# `ruff format` is failing  to reformat some code

---

_Issue opened by @samuelcolvin on 2024-12-12 18:31_

I think I might actually found a bug in Ruff! Do I get a prize???

See [this code](https://gist.github.com/samuelcolvin/3d2b490d140defae08286bf1bdfab9a1).

Trying to format it with `uv run ruff format ruff_format_broken.py` is not reformatting the weirdly formatted dicts on line:
* 214
* 241
* 266

I've tried ruff 0.8.0 and 0.8.3.

config:

```toml
[tool.ruff.lint]
extend-select = [
    "Q",
    "RUF100",
    "C90",
    "UP",
    "I",
    "D",
]
flake8-quotes = { inline-quotes = "single", multiline-quotes = "double" }
isort = { combine-as-imports = true, known-first-party = ["pydantic_ai"] }
mccabe = { max-complexity = 15 }
ignore = [
    "D100", # ignore missing docstring in module
    "D102", # ignore missing docstring in public method
    "D104", # ignore missing docstring in public package
    "D105", # ignore missing docstring in magic methods
    "D107", # ignore missing docstring in __init__ methods
]

[tool.ruff.lint.pydocstyle]
convention = "google"

[tool.ruff.format]
# don't format python in docstrings, pytest-examples takes care of it
docstring-code-format = false
quote-style = "single"

[tool.ruff.lint.per-file-ignores]
"tests/**/*.py" = ["D"]
"docs/**/*.py" = ["D"]
"pydantic_ai_examples/**/*.py" = ["D101", "D103"]
```

---

_Label `question` added by @MichaReiser on 2024-12-12 18:38_

---

_Comment by @MichaReiser on 2024-12-12 18:38_

I'm sorry. I have to disappoint you :( At least the formatter is working as expected. There's an argument that RUF028 should flag the suppression comment on line 203 because it doesn't enable any formatting. 

The suppression comments need to be at the same level or the `fmt: on` has no effect.

```py
# fmt: off
async def google_style_docstring_no_body(
    foo: int, bar: Annotated[str, Field(description='from fields')]
) -> str:  # pragma: no cover
    """
    Args:
        foo: The foo thing.
        bar: The bar thing.
    """
    return f'{foo} {bar}'
# fmt: on
```



---

_Comment by @samuelcolvin on 2024-12-12 18:44_

🤦 🤦 🤦 🤦 

thanks. maybe one day...


---

_Closed by @samuelcolvin on 2024-12-12 18:44_

---
