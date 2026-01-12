```yaml
number: 1119
title: Missing attribute access checking for Pydantic BaseModel
type: issue
state: closed
author: jackhodkinson
labels: []
assignees: []
created_at: 2025-09-03T10:06:55Z
updated_at: 2025-09-03T13:16:42Z
url: https://github.com/astral-sh/ty/issues/1119
synced_at: 2026-01-12T15:54:24Z
```

# Missing attribute access checking for Pydantic BaseModel

---

_@jackhodkinson_

# Missing attribute access checking for Pydantic BaseModel

ty doesn't detect invalid attribute access on Pydantic models, while pyright correctly reports an error.

## Reproduction

```python
from pydantic import BaseModel

class Example(BaseModel):
    name: str
    age: int

def main():
    examples: list[Example] = [
        Example(name="John", age=30),
    ]
    print(examples[0].incorrect)  # Should error: attribute "incorrect" doesn't exist
```

## Current behavior

```bash
$ uv run ty check main.py
All checks passed!
```

## Expected behavior

```bash
$ uv run pyright main.py
  /path/to/main.py:11:23 - error: Cannot access attribute "incorrect" for class "Example"
    Attribute "incorrect" is unknown (reportAttributeAccessIssue)
```

## Environment

- ty version: 0.0.1a19
- Python: 3.13.3
- pydantic: 2.11.7

---

_Comment by @sharkdp on 2025-09-03 10:41_

Thank you for reporting this.

This is related to the fact that we infer `list[Unknown]` for `examples` here (despite the annotation... because you're accessing it from the same scope, so we use the inferred local type, instead of the declared type).

If you change your example to the following, you'll notice that ty emits an error now:
```py
example = Example(name="John", age=30),
print(example.incorrect)  # error: unresolved-attribute
```

Related tickets:
* https://github.com/astral-sh/ty/issues/543
* https://github.com/astral-sh/ty/issues/168
* https://github.com/astral-sh/ty/issues/136

---

_Closed by @sharkdp on 2025-09-03 10:41_

---

_Comment by @godsplanhub on 2025-09-03 13:16_

Also ty does not autocomplete or autosuggest field values
if you were to start typing name it would help with autocomplete with the correct name of the fields that the model has.Any workaround on this.This would really be useful if Example(naMe="John",age=30,unknown="Result") ty would notify on unknown field value naMe and unknown.
Am on neovim and i have setup ty ruff and jedi-language-server .The same behavior i achieved it with mypy and it was really helpful.

---
