---
number: 9508
title: No __prepare__ at is_known_dunder_method
type: issue
state: closed
author: arnaud-ma
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-01-14T03:33:27Z
updated_at: 2024-01-15T14:37:21Z
url: https://github.com/astral-sh/ruff/issues/9508
synced_at: 2026-01-10T01:22:49Z
---

# No __prepare__ at is_known_dunder_method

---

_Issue opened by @arnaud-ma on 2024-01-14 03:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

For exemple 
```python3
class A(type):
    @classmethod
    def __prepare__(cls, name, bases): ...
```
returns the error "PLW3201 Bad or misspelled dunder method name `__prepare__`". It appears that this is missing in https://github.com/astral-sh/ruff/blob/e8d7a6dfce3251a76afc754899034fad48376d99/crates/ruff_linter/src/rules/pylint/helpers.rs#L202-L210

We can verify that this is the last dunder method missing by doing a regex `\b__\w+__\b` on https://docs.python.org/3/reference/datamodel.html and looking at what is missing in the file. We obtain 
- `__annotations__`
- `__bases__`
- `__classcell__`
- `__closure__`
- `__code__`
- `__defaults__`
- `__file__`
- `__func__`
- `__future__`
- `__globals__`
- `__import__`
- `__kwdefaults__`
- `__match_args__`
- `__mro__`
- `__mro_entries__`
- `__name__`
- `__objclass__`
- `__prepare__`
- `__qualname__`
- `__self__`
- `__slots__`
- `__traceback__`
- `__type_params__`

It appears `__prepare__` is the only method here

---

_Comment by @zanieb on 2024-01-14 04:00_

Thanks! Not sure if this was excluded intentionally but I'm not sure why it would be.



---

_Label `bug` added by @zanieb on 2024-01-14 04:00_

---

_Label `good first issue` added by @zanieb on 2024-01-14 04:00_

---

_Comment by @arnaud-ma on 2024-01-14 04:59_

Well `__prepare__` is a bit of a special case that very few people use, this is the only method that only makes sense if it's in a metaclass so I can understand. 
Furthermore for the `PLW3201` as we can add it in the bad-dunder-method-name setting it's not really a big problem in itself. But I find it a bit strange that by default the linter acts as if the method does not exist at all.

---

_Comment by @takaya0 on 2024-01-14 15:05_

I'd like to work on this, please assign me :)

---

_Assigned to @takaya0 by @charliermarsh on 2024-01-14 15:10_

---

_Referenced in [astral-sh/ruff#9529](../../astral-sh/ruff/pulls/9529.md) on 2024-01-15 14:31_

---

_Closed by @charliermarsh on 2024-01-15 14:37_

---
