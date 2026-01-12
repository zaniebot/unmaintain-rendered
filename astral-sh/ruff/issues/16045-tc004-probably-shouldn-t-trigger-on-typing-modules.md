```yaml
number: 16045
title: "TC004 probably shouldn't trigger on typing-modules"
type: issue
state: closed
author: mikeshardmind
labels:
  - bug
  - preview
assignees: []
created_at: 2025-02-08T22:08:58Z
updated_at: 2025-02-09T16:27:08Z
url: https://github.com/astral-sh/ruff/issues/16045
synced_at: 2026-01-12T15:54:55Z
```

# TC004 probably shouldn't trigger on typing-modules

---

_@mikeshardmind_

### Description

relevant parts of config:

```toml
[tool.ruff]

line-length = 90
target-version = "py312"
preview = true

[tool.ruff.format]
line-ending = "lf"

[tool.ruff.lint]
select = [
    "A", "ANN", "ASYNC", "B", "BLE", "C4", "COM", "D", "DOC", "DTZ", "E",
    "EM", "ERA", "F", "FA", "FBT", "FURB", "G", "I", "INP", "ISC", "NPY",
    "PD", "PERF", "PGH", "PIE", "PLC", "PLE", "PLR", "PLW", "PTH", "PYI",
    "Q", "Q003", "RET", "RSE", "RUF", "S", "SIM", "SLOT", "T20", "TC", "TID",
    "TRY", "UP", "YTT"
]

typing-modules = ["async_utils._typings"]
```

contents of `async_utils._typings`

```py
# License and context of encountering this issue available here
# https://github.com/mikeshardmind/async-utils/blob/c84b9a71f8caf5a5dcfafc675eff7dec60bd4162/src/async_utils/_typings.py#L27-L44
from __future__ import annotations

TYPE_CHECKING = False
if TYPE_CHECKING:
    from typing import Any, Literal, Never, Self
else:

    def __getattr__(name: str):
        if name in {"Any", "Literal", "Never", "Self"}:
            import typing

            return getattr(typing, name)

        msg = f"module {__name__!r} has no attribute {name!r}"
        raise AttributeError(msg)


__all__ = ["TYPE_CHECKING", "Any", "Literal", "Never", "Self"]
```

errors:

```
Error: src/async_utils/_typings.py:31:24: TC004 Move import `typing.Any` out of type-checking block. Import is used for more than type hinting.
Error: src/async_utils/_typings.py:31:29: TC004 Move import `typing.Literal` out of type-checking block. Import is used for more than type hinting.
Error: src/async_utils/_typings.py:31:38: TC004 Move import `typing.Never` out of type-checking block. Import is used for more than type hinting.
Error: src/async_utils/_typings.py:31:45: TC004 Move import `typing.Self` out of type-checking block. Import is used for more than type hinting.
```

Not sure there's a good way to handle this, this is intentionally slightly dynamic use, as throughout the rest of the module, use of typing is done with something like:

```py
from __future__ import annotations  # remove this on 3.14+

from . import _typings as t

def errors() -> t.Never:
    ...
```

Which preserves runtime introspectability, but can result in typing never being imported if that introspection capability is never used.

Checking that a corresponding use exists in else block here might be more trouble than it is worth, and it may be better to just exempt any file marked as part of `typing-modules` from this. For now I'm going to disable the rule.

---

_Comment by @ntBre on 2025-02-09 02:07_

Thanks for the report! This looks like a change/regression in 0.9.5 (possibly related to #15719?) I was having trouble reproducing it locally but realized my global ruff install was on 0.9.3 😆 

I can actually reproduce with just `--select TC004` and `--preview` on 0.9.5:

```shell
 cat <<'EOF' | uvx ruff@0.9.5 check --select TC004 --no-cache --preview --isolated -
from __future__ import annotations

TYPE_CHECKING = False
if TYPE_CHECKING:
    from typing import Any, Literal, Never, Self
else:

    def __getattr__(name: str):
        if name in {"Any", "Literal", "Never", "Self"}:
            import typing

            return getattr(typing, name)

        msg = f"module {__name__!r} has no attribute {name!r}"
        raise AttributeError(msg)


__all__ = ["TYPE_CHECKING", "Any", "Literal", "Never", "Self"]
EOF
```

@AlexWaygood might have more insight into the intended or desired behavior here.

---

_Label `bug` added by @ntBre on 2025-02-09 02:07_

---

_Label `preview` added by @ntBre on 2025-02-09 02:07_

---

_Comment by @AlexWaygood on 2025-02-09 12:18_

I suppose we _could_ automatically switch off this rule for all modules listed in the `typing-modules` setting. I'm not sure if that's what all users would naturally expect, though. If you find the rule otherwise useful, but you want to suppress all diagnostics from the rule in this specific file, you can do that by putting `# ruff: noqa: TC004` anywhere in the file (docs at https://docs.astral.sh/ruff/linter/#error-suppression), or by using either the [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) setting or the [`extend-per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_extend-per-file-ignores) setting.

@Daverball, what do you think of automatically switching the rule off for all files listed in the `typing-modules` setting?

---

_Comment by @MichaReiser on 2025-02-09 12:30_

This is probably a dumb question but is there a specific reason why the typing module uses `.py` over `.pyi`?

---

_Comment by @AlexWaygood on 2025-02-09 12:34_

> This is probably a dumb question but is there a specific reason why the typing module uses `.py` over `.pyi`?

The idea of the typing module here is to reduce the import time of the app or library at runtime by only lazily importing the `typing` module "on demand". On more recent Python versions, the typing module is actually faster to import than lots of other popular stdlib modules such as `inspect` or `dataclasses`, but it has a historical reputation as being very slow to import, so this kind of thing is reasonably common. (It's also one of the easier modules to do this kind of thing with; it's harder to use this kind of shim module to delay the import of `dataclasses`, for example.)

This wouldn't work if the typing module was a `.pyi` file because `.pyi` files are never executed at runtime; they're just for type checkers. So a `.pyi` file would have no effect on the import time of the app or library.

---

_Label `needs-decision` added by @AlexWaygood on 2025-02-09 12:59_

---

_Comment by @Daverball on 2025-02-09 13:08_

The main culprit here is `__all__`, it creates a runtime reference to these symbols and the `else` clause is too dynamic for ruff to understand that these symbols are actually available at runtime.

This is such a specialized case, I think it makes more sense to just `noqa: TC004` the imports, anything else is a hack at best.

---

_Comment by @Daverball on 2025-02-09 13:14_

Another solution is to disable `TC004` if there is a module-level `__getattr__` and the only runtime reference is in `__all__`, since realistically we can't know at that point if a symbol is available at runtime or not.

---

_Comment by @AlexWaygood on 2025-02-09 13:15_

> Another solution is to disable `TC004` if there is a module-level `__getattr__` and the only runtime reference is in `__all__`, since realistically we can't know at that point if a symbol is available at runtime or not.

Oh, that seems like a reasonable improvement to me!

---

_Label `needs-decision` removed by @AlexWaygood on 2025-02-09 16:15_

---

_Closed by @AlexWaygood on 2025-02-09 16:27_

---
