```yaml
number: 764
title: "[panic] ty crashes with UnboundIterAndGetitemError on Iterable types during type checking"
type: issue
state: closed
author: SoraKukyo
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-07-04T10:54:41Z
updated_at: 2025-07-25T10:16:06Z
url: https://github.com/astral-sh/ty/issues/764
synced_at: 2026-01-10T02:06:24Z
```

# [panic] ty crashes with UnboundIterAndGetitemError on Iterable types during type checking

---

_Issue opened by @SoraKukyo on 2025-07-04 10:54_

### Bug report: `ty` panics during type checking on Iterable-related code

**Description:**

When running `ty check` as part of pre-commit hooks on my project, I encounter a panic error related to `try_iterate()` failing on a type assignable to `Iterable`. The error message suggests that `ty` tries to iterate over a type but fails due to an unimplemented method.

**Reproduction details:**

- Running pre-commit with the `ty` hook
- `ty` version: (unspecified, latest pre-release)
- OS: Windows x86_64
- Project path: `D:\personal\freelance\MyProject\run_scrapyd.py` *(replaced from original)*

**Error output snippet:**
```log
error[panic]: Panicked at crates\ty_python_semantic\src\types\call\bind.rs:989:70 when checking D:\personal\freelance\MyProject\run_scrapyd.py: try_iterate() should not fail on a type assignable to Iterable: UnboundIterAndGetitemError { dunder_getitem_error: MethodNotAvailable }
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: windows x86_64
info: Args: ["...\ty.exe", "check", "projects/MyProject/scrapy.cfg", "projects/MyProject/MyProject/spiders/dummy.py", "projects/MyProject/MyProject/myscript.py", "run_scrapyd.py"]
info: run with RUST_BACKTRACE=1 environment variable to show the full backtrace information
info: query stacktrace:
0: infer_expression_types(Id(4b84))
1: infer_definition_types(Id(15e5a))
```

**Additional notes:**

- This error started appearing recently, with no relevant code changes.
- The panic breaks the entire pre-commit run.
- The warning `ty is pre-release software and not ready for production use` is shown.
- I have confirmed the same issue occurs both locally and in CI (GitHub Actions).

Please let me know if you need any more logs or reproduction steps.

---

Thanks for the awesome project! Hope this helps improve stability.

---

_Label `bug` added by @AlexWaygood on 2025-07-04 11:05_

---

_Label `fatal` added by @AlexWaygood on 2025-07-04 11:05_

---

_Comment by @AlexWaygood on 2025-07-04 11:08_

Thanks! It'll be hard to debug this without a minimal reproducible example, unfortunately -- are you able to share a self-contained snippet of code that reproduces the panic when you run ty on it?

---

_Comment by @SoraKukyo on 2025-07-04 11:17_

Thanks for the quick reply!
 The panic occurs when running:
`uv run pre-commit run --all-files` 
with my current pre-commit setup (see below). The failure consistently happens on the file `run_scrapyd.py` which contains this  code:
```python

import os

from twisted.internet import reactor
from twisted.web import server

from scrapyd import get_application

if __name__ == "__main__":
    os.environ["SCRAPYD_CONFIG"] = "scrapyd/scrapyd.conf"
    application = get_application()
    site = server.Site(application)
    reactor.listenTCP(6800, site)  # type: ignore[unresolved-attribute]
    reactor.run()  # type: ignore[unresolved-attribute]
```

The error is related to try_iterate() failing on a type assignable to Iterable, specifically pointing to
```css
UnboundIterAndGetitemError { dunder_getitem_error: MethodNotAvailable }
```

his happens both locally and in CI (GitHub Actions).

Here is the relevant portion of my `.pre-commit-config.yaml`:
```yaml
repos:
  - repo: https://github.com/astral-sh/uv-pre-commit
    rev: 0.6.6
    hooks:
      - id: uv-lock
  # ... other hooks omitted for brevity ...
  - repo: local
    hooks:
      - id: ty
        name: type checking
        entry: uvx ty check
        language: system
```

If you need me to isolate this further or provide any other details, please let me know.

FYI: 2 days ago, everything was working fine!

---

_Comment by @AlexWaygood on 2025-07-04 16:14_

> FYI: 2 days ago, everything was working fine!

yeah, the regression was caused by https://github.com/astral-sh/ruff/pull/18987, which was released as part of ty 0.0.1-alpha-13, which we released 2 days ago :-)

---

_Comment by @AlexWaygood on 2025-07-04 16:17_

> ```py
> import os
> 
> from twisted.internet import reactor
> from twisted.web import server
> 
> from scrapyd import get_application
> 
> if __name__ == "__main__":
>     os.environ["SCRAPYD_CONFIG"] = "scrapyd/scrapyd.conf"
>     application = get_application()
>     site = server.Site(application)
>     reactor.listenTCP(6800, site)  # type: ignore[unresolved-attribute]
>     reactor.run()  # type: ignore[unresolved-attribute]
> ```

Thanks, I can reproduce if I save this snippet as `foo.py` and then run `uv run --no-project --with=scrapyd --with=twisted cargo run -p ty check foo.py`

---

_Comment by @AlexWaygood on 2025-07-04 16:21_

A smaller repro is:

```py
from twisted.web import server

server.Site()
```

---

_Comment by @AlexWaygood on 2025-07-04 16:42_

Even smaller: run `uv run --no-project --with=zope cargo run -p ty check foo.py` with this saved as `foo.py`:

```py
from zope.interface import implementer

implementer()
```

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-07-04 16:43_

---

_Comment by @AlexWaygood on 2025-07-05 18:51_

And here, finally, is a minimal repro with no third-party dependencies:

```py
from unresolved_module import SomethingUnknown

class Foo(SomethingUnknown): ...

tuple(Foo)
```

So it looks like the issue is with classes that have `Any`/`Unknown` in their MRO.

---

_Closed by @AlexWaygood on 2025-07-25 10:16_

---
