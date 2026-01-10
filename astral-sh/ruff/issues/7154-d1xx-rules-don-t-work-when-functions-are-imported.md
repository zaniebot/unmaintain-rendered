---
number: 7154
title: "D1XX rules don't work when functions are imported into a public file"
type: issue
state: open
author: AKuederle
labels:
  - type-inference
assignees: []
created_at: 2023-09-05T13:15:55Z
updated_at: 2024-10-24T14:32:59Z
url: https://github.com/astral-sh/ruff/issues/7154
synced_at: 2026-01-10T01:22:46Z
---

# D1XX rules don't work when functions are imported into a public file

---

_Issue opened by @AKuederle on 2023-09-05 13:15_

The D1XX rules check if docstrings for public functions/classes exist. However, I think they are missing a common pattern of defining "public objects". Namely, defining them in "private" (aka leading underscore) files and then importing them into a top-level `__init__`.

```python
# mypackage/__init__.py
from mypackage._methods implement my_function

__all__ = ["my_function"]
```

```python
# mypackage/_methods.py

def my_function():
   pass
```

In this case, I would expect ruff to identify `my_function` as a public function, as it is imported in a public module (and even added to `__all__`).


---

_Comment by @MichaReiser on 2023-09-05 20:45_

Thanks for reporting this common pattern that ruff doesn't support today (to my knowledge)

I'm unfamiliar with the implementation but I assume that this isn't working as expected because Ruff only supports single-file analysis today and your example requires aggregating knowing about the import and `__all__` export of `my_function` in the `__init__.py` file while lining the `_methods.py` file.

---

_Label `multifile-analysis` added by @MichaReiser on 2023-09-05 20:45_

---

_Comment by @AKuederle on 2023-09-06 06:07_

Thus makes sense! And I understand that this would be a considerable effort
then to fix this...

I think a potential workaround would be to make it configurable, if files
with a leading underscore should lead to all objects inside to be
considered non-public. For a project structure like I outlined above,
having ruff treat the `_methods` file "just" like a normal file would make
these rules at least usable for these types of setups

On Tue, 5 Sept 2023, 22:45 Micha Reiser, ***@***.***> wrote:

> I'm unfamiliar with the implementation but I assume that this isn't
> working as expected because Ruff only supports single-file analysis today
> and your example requires aggregating knowing about the import and __all__
> export of my_function in the __init__.py file while lining the _methods.py
> file.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/ruff/issues/7154#issuecomment-1707287776>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ACSHBOZGJWSFW2MGJXW4EYTXY6FOBANCNFSM6AAAAAA4LY3NGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Referenced in [astral-sh/ruff#9561](../../astral-sh/ruff/issues/9561.md) on 2024-03-21 13:29_

---

_Comment by @KRunchPL on 2024-05-12 16:22_

I fully agree with the previous comment. I follow this pattern in many projects.

Moreover, the configuration to disable treating `_files` as private (and disabling D1XX rules in them) would be not only a workaround. It is not uncommon in bigger projects, requiring docstring for "internally public" entities (not starting with `_`, but being in `_` module or package).

With this flag it would be still pretty easy to disable those rules for specific file(s), while without the support there is currently no way to enable it when needed. Thus it would make the tool much more flexible.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:32_

---
