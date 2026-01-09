---
number: 14439
title: Possible error when trying to fix with ICN001
type: issue
state: closed
author: tpgillam
labels:
  - bug
  - configuration
  - fixes
assignees: []
created_at: 2024-11-18T17:45:15Z
updated_at: 2024-11-21T15:54:51Z
url: https://github.com/astral-sh/ruff/issues/14439
synced_at: 2026-01-07T13:12:16-06:00
---

# Possible error when trying to fix with ICN001

---

_Issue opened by @tpgillam on 2024-11-18 17:45_

I obtain errors with [ICN001](https://docs.astral.sh/ruff/rules/unconventional-import-alias/) when trying to apply autofix (in 'unsafe' mode) that should ideally convert the import from `from A1.A2.AN import B` to `import A1.A2.AN`.

For example, consider the following simple source file `moo.py`:
```python
from math import sin

sin(42)
```

And then the following configuration for ruff  in `pyproject.toml`
```toml
[project]
name = "moo"
version = "0.1.0"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "ruff>=0.7.4",
]

[tool.ruff.lint]
select = ["ICN001"]

[tool.ruff.lint.flake8-import-conventions.aliases]
"math.sin" = "math.sin"
```

I get the following error:
```console
> uv run ruff check --fix --unsafe-fixes moo.py
error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `moo.py`, the rule codes ICN001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

moo.py:1:18: ICN001 `math.sin` should be imported as `math.sin`
  |
1 | from math import sin
  |                  ^^^ ICN001
2 |
3 | sin(42)
  |
  = help: Alias `math.sin` to `math.sin`

Found 1 error.
[*] 1 fixable with the --fix option.
```

I'm not 100% sure that I'm not abusing the intention of this rule, since here I'm trying to use the rule to enforce the _absence_ of an alias, rather than a conventional alias :)

---

_Comment by @dylwil3 on 2024-11-18 18:00_

Interesting... I think this is not quite what ICN001 was designed for. It's not possible to import (and then alias) a function like `import mymodule.myfunction`, which is what your user configuration is asking ICN001 to do.

Were you maybe looking for something like [`ICN003`](https://docs.astral.sh/ruff/rules/banned-import-from/)?

---

_Comment by @dylwil3 on 2024-11-18 22:46_

I'm also not super sold on whether to count this as a bug, since the user has required Ruff to insert the syntax error. It would be like writing a required import in the required imports linter setting that had a syntax error in it. 

This is especially difficult to detect in advance because one has to resolve the module you're trying to import in order to tell whether you're attempting to alias a submodule or a function.

---

_Comment by @dscorbett on 2024-11-18 23:52_

This should count as a bug because something of the form `from x import y as x.y` is a syntax error regardless of whether `x.y` is a submodule or a function, so it can be detected in advance. Here is an example with a submodule:
```console
$ echo 'from collections import abc' | ruff check --config 'lint.flake8-import-conventions.aliases = {"collections.abc" = "collections.abc"}' --select ICN001 - --unsafe-fixes --fix --isolated

error: Fix introduced a syntax error. Reverting all changes.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BFix%20error%5D

...quoting the contents of `-`, the rule codes ICN001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

from collections import abc
-:1:25: ICN001 `collections.abc` should be imported as `collections.abc`
  |
1 | from collections import abc
  |                         ^^^ ICN001
  |
  = help: Alias `collections.abc` to `collections.abc`

Found 1 error.
[*] 1 fixable with the --fix option.
```
Compare I002: if the user configures it to insert a syntax error, Ruff reports it as a user error instead of a generic “This indicates a bug in Ruff” error:
```console
$ echo 1 | ruff check --config 'lint.isort.required-imports = ["from math import sin as math.sin"]' --select I002 - --fix --isolated
error: invalid value 'lint.isort.required-imports = ["from math import sin as math.sin"]' for '--config <CONFIG_OPTION>'

  tip: A `--config` flag must either be a path to a `.toml` configuration file
       or a TOML `<KEY> = <VALUE>` pair overriding a specific configuration
       option

Could not parse the supplied argument as a `ruff.toml` configuration option:

Expected ',', found '.' at byte range 28..29
in `lint`

For more information, try '--help'.
```

---

_Comment by @dylwil3 on 2024-11-19 00:25_

Ah thanks for the investigation! I was mistakenly assuming that we were trying to do `import a.b as ...` and that's where the trouble originated.

We should certainly correct this!

As for detecting syntax errors like `import math.sin`: what I was trying to say is that while Ruff can detect the syntax error at the point of configuration in the example you give because it is detectable at the level of the grammar, I don't think it can detect `import math.sin` since it would have to know that `sin` was a function and not a submodule.

---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-19 03:04_

---

_Label `bug` added by @dhruvmanila on 2024-11-19 03:13_

---

_Label `fixes` added by @dylwil3 on 2024-11-19 06:07_

---

_Label `configuration` added by @dylwil3 on 2024-11-19 06:07_

---

_Comment by @tpgillam on 2024-11-19 10:29_

Just a little additional context about our objective:

We have internal modules such that there exists a function `a.b.c.moo_func`. And there is also `d.moo_func`. So we want to enforce our convention that, at all sites, we always write:
```python
import a.b.c
import d

a.b.c.moo_func(42)
d.moo_func(42)
```

But there might exist `a.b.c.Something` that we are happy to let people import as `Something` due to less ambiguity, so ICN003 would be a little too restrictive in this case.

We use the ICN001 rule to enforce this -- and it does seem (perhaps coincidentally!) to do a good job of detecting what we want. It's just that the autofix doesn't work :)

---

_Referenced in [astral-sh/ruff#14477](../../astral-sh/ruff/pulls/14477.md) on 2024-11-20 05:01_

---

_Comment by @dylwil3 on 2024-11-20 05:09_

The good news is that I can resolve the panic by surfacing an error to the user upon loading the configuration.

The bad news is that this breaks @tpgillam 's "hack" 😞 . Unfortunately I think I do have to prefer fixing the panic over preserving the hack, because the latter is truly not how the rule was intended to work.

If ICN003 does not help, perhaps you can open another issue describing a lint rule or configuration option that would make sense for your use case and we can consider it there? Apologies.

---

_Comment by @tpgillam on 2024-11-20 09:37_

No apologies required! Correctness is clearly preferable, I agree.

---

_Closed by @MichaReiser on 2024-11-21 15:54_

---
