```yaml
number: 167
title: "Enforce import from `__all__` (at least locally)"
type: issue
state: open
author: samuelcolvin
labels:
  - rule
  - type-inference
assignees: []
created_at: 2022-09-12T10:29:52Z
updated_at: 2024-10-24T14:32:57Z
url: https://github.com/astral-sh/ruff/issues/167
synced_at: 2026-01-12T15:54:40Z
```

# Enforce import from `__all__` (at least locally)

---

_@samuelcolvin_

I understand that ruff can't check imports are valid in a general way, but it would be great if ruff could check local (relative) imports only import things listed in `__all__` where `__all__` is defined.

## Example of what I want

**foo.py**:

```py
__all__ = ('public_thing',)

def public_thing():
    pass

def private_thing():
    pass
``` 

**bar.py**:

```py
from .foo import public_thing # 👍

from .foo import private_thing # 👎 FXXX 'private_thing' not declared in __all__
```

---

This would only apply where `__all__` is defined in the module scope obviously.

Any chance ruff could be extended to support this? 

This get's fairly important in big packages. But I don't currently know of a tool which enforces `__all__` and I'm resisting the the impulse to build one myself. 


---

_Label `enhancement` added by @charliermarsh on 2022-09-12 13:29_

---

_Comment by @charliermarsh on 2022-09-16 17:13_

Do you know if Mypy enforces this?

---

_Comment by @samuelcolvin on 2022-09-16 17:28_

Nope.

It does some checks, but not full support.

I'll try to check and report back here.

---

_Comment by @charliermarsh on 2022-09-16 17:34_

Only if convenient, not a big deal :) This would be great to support, though obviously requires some significant changes to the model (since we're no longer checking files in isolation), so won't happen immediately.

---

_Comment by @samuelcolvin on 2022-09-16 19:56_

The problem is that according the the formal rules (and runtime behaviour), importing `private_thing` should be fine. All that `__all__` actually does is mean that `private_thing` wouldn't be imported if I used `from .foo import *`.

What I'm really looking for is a way to enforce (ideally at runtime as well as in static analysis) the equivalent of `export` in TypeScript/JS - e.g. a way to white list which attributes of a module can be accessed, but sadly (and IMHO very strangely) it seems to be impossible.

---

_Comment by @charliermarsh on 2022-09-16 20:51_

Ugh, yeah, I’d love to have something like ESM or module visibility modifiers in Python.

---

_Comment by @charliermarsh on 2022-09-16 20:52_

Mypy does have a “no implicit re-export” setting which helps a bit with the __all__ thing, IIRC.

---

_Comment by @samuelcolvin on 2022-09-16 21:34_

Agreed, problem is, even with that, if you put code in a private module, then re-export it through a public one, even with `__all__` set, pycharm tries to import from the private module.

In short, enforcing a public API/private logic divide is virtually impossible with python.

---

_Comment by @charliermarsh on 2022-12-09 15:15_

This is really similar (identical?) to #1107. I think this import / export enforcement is something I want to do soon (maybe after I improve config resolution).

---

_Comment by @smackesey on 2022-12-20 23:58_

`pyright` enforces something similar but with some tweaks-- I believe if `ruff` implements this it should follow pyright's publicity rules, which are also in a [document](https://github.com/python/typing/blob/master/docs/source/libraries.rst#library-interface-public-and-private-symbols) in the `python/typing` repo (though not in a PEP to my knowledge):

>If a py.typed module is present, a type checker will treat all modules within that package (i.e. all files that end in .py or .pyi) as importable unless the file name begins with an underscore. These modules comprise the supported interface for the library.
>
>Each module exposes a set of symbols. Some of these symbols are considered "private” — implementation details that are not part of the library’s interface. Type checkers can use the following rules to determine which symbols are visible outside of the package.
>
>- Symbols whose names begin with an underscore (but are not dunder names) are considered private.
>- Imported symbols are considered private by default. If they use the import A as A (a redundant module alias), from X import A as A (a redundant symbol alias), or from . import A forms, symbol A is not private unless the name begins with an underscore. If a file __init__.py uses form from .A import X, symbol A is treated likewise. If a wildcard import (of the form from X import *) is used, all symbols referenced by the wildcard are not private.
>- A module can expose an __all__ symbol at the module level that provides a list of names that are considered part of the interface. This overrides all other rules above, allowing imported symbols or symbols whose names begin with an underscore to be included in the interface.
> Local variables within a function (including nested functions) are always considered private.

Basically a symbol is public if it:

- is defined inside the module and doesn't have a leading `_`
- is a redundantly aliased import
- is listed in `__all__`

More generally, I'm curious whether ruff ever seeks to depend on a Python environment for import resolution info in the way a tool like `pyright` does?

My own opinion here is that `ruff` should probably forgo any rules dependent on import resolution and signature knowledge unless it wants to get fully into the type-checking game. Pylint implements pseudo typechecking and I mostly find it a source of confusion/false positives. Also, figuring out how to implement and configure import resolution is no joke and a significant source of confusion for users.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:21_

---

_Label `rule` added by @charliermarsh on 2022-12-31 18:21_

---

_Comment by @sanmai-NL on 2023-10-04 12:54_

Publicity rules are perhaps the work of a PR bureau, these things are often named visibility rules in this domain.

---

_Comment by @smackesey on 2023-10-04 14:44_

>Publicity rules are perhaps the work of a PR bureau, these things are often named visibility rules in this domain.

Take it up with the maintainers of python/typing: https://github.com/python/typing/blob/master/docs/source/libraries.rst#library-interface-public-and-private-symbols

---

_Comment by @sbdchd on 2024-01-07 03:05_

I was thinking of an additional feature of this rule where if a variable, type, function isn't referenced in the module and isn't in `__all__` then it would also be considered unused – then `ruff` would be able to automatically mark and delete the code

Like you can mark all your stuff with `_` but honestly not ideal, I think using `__all__` as import / export visibility would be nice

```python
# some_module.py
from dataclasses import dataclass

__all__ = ['does_something']


@dataclass
class Result: # unused
     ok: bool
     result: str

def does_something() -> bool: ...

def foo() -> None: ... # unused
```

---

_Comment by @rsxdalv on 2024-07-10 00:37_

Case in point - this broke torch and setuptools >=70.0.0 installations

---

_Label `multifile-analysis` added by @MichaReiser on 2024-07-10 07:30_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:32_

---
