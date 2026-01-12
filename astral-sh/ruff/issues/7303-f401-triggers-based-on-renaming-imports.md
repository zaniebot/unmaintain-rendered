```yaml
number: 7303
title: F401 triggers based on renaming imports
type: issue
state: closed
author: henryiii
labels:
  - question
assignees: []
created_at: 2023-09-12T14:26:52Z
updated_at: 2023-09-12T15:38:36Z
url: https://github.com/astral-sh/ruff/issues/7303
synced_at: 2026-01-12T15:54:47Z
```

# F401 triggers based on renaming imports

---

_@henryiii_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff's F401 triggers if you import something _and_ rename it, but not if you rename it but keep the name the same.

For example, this file doesn't use any of it's imports:

```python
from . import clients as clients
from .config import config as config
from .config_training import config as config_training # E: F401 [*] `.config_training.config` imported but unused
from .thing import other as foobar # E: F401 [*] `.thing.other` imported but unused
from . import x as y # E: F401 [*] `.x` imported but unused
from a import b as c # E: F401 [*] `a.b` imported but unused
```

Which I'm running in VI via ALE using the LSP to get the comments, but I can also run it manually:

```console
$ ruff check --isolated --select=F401 example.py
example.py:3:40: F401 [*] `.config_training.config` imported but unused
example.py:4:29: F401 [*] `.thing.other` imported but unused
example.py:5:20: F401 [*] `.x` imported but unused
example.py:6:20: F401 [*] `a.b` imported but unused
Found 4 errors.
[*] 4 potentially fixable with the --fix option.
```

Notice that the error doesn't trigger if you `from a import b as b`, but only if you use `from a import b as c`. This was from an `__init__.py` file that didn't set `__all__`, which [isn't a good pattern](https://learn.scientific-python.org/development/patterns/exports/), and it goes away as it should if you set `__all__`). But I think it should be consistent. I'm pretty sure the problem is that `from a import b as b` is being treated as `from a import b; b = b` which "uses" `b`, making it not unused. That's a guess.



---

_Comment by @zanieb on 2023-09-12 14:38_

We allow `from a import b as b` because it's respected by type checkers as a way to "export" the name similar to `__all__`. See the [Pyright documentation section](https://github.com/microsoft/pyright/blob/ada1ea63e8360fe3bd203bff3b0069c34d142817/docs/typed-libraries.md#library-interface) that discusses redundant aliases.

Some related discussion at https://github.com/astral-sh/ruff/issues/5697

---

_Label `question` added by @zanieb on 2023-09-12 14:38_

---

_Comment by @Avasam on 2023-09-12 14:51_

To me, this looks correct and intended. Whilst both import examples lead to **unused** symbol in the **current** module, both also lead to symbols that **exist** and can be imported by other modules.
`from example import clients` and `from example import y` are both valid at runtime.
To keep public interfaces sane, type checkers (and some import linters like pycln, isort, etc.) have a bit of heuristic to define what is private and public, and the form `import X as X` is how you explicitly flag an import as an intended re-export without having to specify `__all__` (which affects star imports at runtime).

---

_Comment by @Avasam on 2023-09-12 14:56_

That being said, mixing explicit re-exports and `__all__` isn't a great pattern as you've mentioned, and maybe a rule dedicated to that might satisfy your needs and expectations?

---

_Comment by @henryiii on 2023-09-12 14:57_

Okay, I see why it happens, then, but the code that someone originally was confused about:

```python
from .config import config as config
from .config_training import config as config_training
```

still seems surprising - you can't "reexport" the second `config`, since it can't keep the original name. The current form obviously was trying to reexport, but if it has to be renamed, that doesn't work then.

I'll just suggest that they use `__all__`, explicit is better than implicit. And makes the imports look less odd. :)

---

_Closed by @henryiii on 2023-09-12 14:57_

---

_Comment by @henryiii on 2023-09-12 14:57_

Thanks, by the way!

---

_Comment by @henryiii on 2023-09-12 15:38_

> maybe a rule dedicated to that might satisfy your needs and expectations?

I was thinking the best solution would be to not do the explicit reexports, but move entirely to `__all__`.

I wouldn't mind having a rule that enforced all files (that don't start with an underscore, anyway) have an `__all__`, and all non-`__init__.py` files have a `def __dir__()` too (actually, I already enforce exactly this in pytest for scikit-build-core). But that's pretty specific to following this pattern.

---
