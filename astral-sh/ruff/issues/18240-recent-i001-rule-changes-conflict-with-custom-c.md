```yaml
number: 18240
title: "Recent `I001` rule changes conflict with custom C++ extensions"
type: issue
state: closed
author: JackAshwell11
labels:
  - needs-mre
assignees: []
created_at: 2025-05-21T11:43:17Z
updated_at: 2025-07-24T12:04:24Z
url: https://github.com/astral-sh/ruff/issues/18240
synced_at: 2026-01-10T11:09:58Z
```

# Recent `I001` rule changes conflict with custom C++ extensions

---

_Issue opened by @JackAshwell11 on 2025-05-21 11:43_

### Summary

If you had a module `foo` which internally used a C/C++ extension `bar` and had a dependency on a third-party pip package `abc`, a conflict could arise due to the recent rule changes with `I001` in #16565.

If the import statements were ordered like this in previous versions:
```py
import abc  # This is the third-party pip package

import bar  # This is the custom C/C++ extension
```
The recent rule change would rearrange these imports to being:
```py
import bar  # This is the custom C/C++ extension
import abc  # This is the third-party pip package
```
Which when run through other linters such as `pylint` would raise the error:
```
C0411: third party import "abc" should be placed before first party imports "bar"
```
As the custom `bar` package shouldn't be considered a third-party import like the `abc` package.

### Version

ruff 0.11.10

---

_Comment by @MichaReiser on 2025-05-21 11:45_

CC: @dylwil3 

---

_Comment by @dylwil3 on 2025-05-21 17:31_

Thanks for the report! I'm not super familiar with this situation - would it be possible to give a minimal reproducible example? Alternatively, even just a description of what the directory layout looks like in your situation would help.

---

_Comment by @JackAshwell11 on 2025-05-21 19:41_

So say we have the following directory structure:
```
ª   pyproject.toml
ª   uv.lock
+---bar
ª   ª   __init__.pyi
+---src
ª   +---foo
ª   ª   ª   __main__.py
ª   +---bar
ª   ª   ª   main.cpp
```
Where `project` is the main Python module and `src/foo/__main__.py` imports a few things from `bar` (which is the C/C++ extension). This C/C++ extension is compiled in the `src/bar` folder with added Python bindings so it can be interacted with.

In `src/foo/__main__.py`, we may have the code:
```py
import abc   # This is the third-party pip package

import bar  # This is the custom C/C++ extension

...
```
Showing that it imports the C/C++ extension `bar` and a third-party pip package `abc`. Running `ruff check . --select I001` will change `src/foo/__main__.py`'s code to:
```py
import bar  # This is the custom C/C++ extension
import abc   # This is the third-party pip package

...
```
As `ruff` thinks `bar` is a third-party package when it is not (hence the line separation in the original code snippet).

---

_Comment by @dylwil3 on 2025-05-21 23:06_

Could you share your ruff settings? I'm not able to reproduce this:

```console
❯ tree
.
├── bar
│   └── __init__.pyi
├── pyproject.toml
├── README.md
└── src
    ├── bar
    │   └── main.cpp
    └── foo
        └── __main__.py

5 directories, 5 files

❯ cat src/foo/__main__.py
import bizbuzz

import bar

❯ ruff check --no-cache --select I001 -v .
[2025-05-21][18:01:31][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'bizbuzz' as Known(ThirdParty) (NoMatch)
[2025-05-21][18:01:31][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'bar' as Known(FirstParty) (SourceMatch("/Users/dmbp/Documents/dev/foo"))
All checks passed!

❯ ruff check --no-cache --select I001 --preview -v .
[2025-05-21][18:01:37][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'bizbuzz' as Known(ThirdParty) (NoMatch)
[2025-05-21][18:01:37][ruff_linter::rules::isort::categorize][DEBUG] Categorized 'bar' as Known(FirstParty) (SourceMatch("/Users/dmbp/Documents/dev/foo"))
All checks passed!
```

(I deleted irrelevant debug logs above so if you do this yourself it'll be more verbose).

Note also that the changes in the PR you mentioned only apply in preview mode, so if you were invoking `ruff` without preview then there shouldn't have been any change from before and after that PR was merged.

Is there anything else that might have been relevant to help me re-create the situation where you encountered this behavior?

---

_Comment by @JackAshwell11 on 2025-05-22 09:44_

So I wasn't able to recreate this with a simplified example, but I think my config was just missing the `know-first-party` option:
```toml
[tool.ruff.lint.isort]
known-first-party = ["bar"]
```

---

_Comment by @ollie-bell on 2025-05-23 07:59_

> Note also that the changes in the PR you mentioned only apply in preview mode

FWIW the change works like a charm in my testing with preview mode on. Does it require a “breaking change” release for this to be brought out of preview mode? e.g. are we waiting for 0.12.0 instead of 0.11.X?

---

_Comment by @MichaReiser on 2025-05-23 13:18_

> FWIW the change works like a charm in my testing with preview mode on. Does it require a “breaking change” release for this to be brought out of preview mode? e.g. are we waiting for 0.12.0 instead of 0.11.X?

Yes, we're waiting for a new minor. We wanted to get some more signal before changing the behavior, and it's breaking as well. We plan to do a minor next month and I expect this change to be stabilizie as part of it.

---

_Label `needs-info` added by @MichaReiser on 2025-05-23 13:18_

---

_Label `needs-info` removed by @MichaReiser on 2025-05-23 13:18_

---

_Label `needs-mre` added by @MichaReiser on 2025-05-23 13:18_

---

_Closed by @MichaReiser on 2025-07-24 12:04_

---
