---
number: 9554
title: "`# noqa: I001` not respected but `# isort:skip` is"
type: issue
state: closed
author: monotkate
labels:
  - documentation
assignees: []
created_at: 2024-01-16T19:11:28Z
updated_at: 2024-01-16T20:19:34Z
url: https://github.com/astral-sh/ruff/issues/9554
synced_at: 2026-01-10T01:22:49Z
---

# `# noqa: I001` not respected but `# isort:skip` is

---

_Issue opened by @monotkate on 2024-01-16 19:11_

I'm working to move a codebase from isort from ruff's isort. There are a few lines we don't want moving around, that are currently labeled with `# noqa for side effects`. Ruff isort blew right through that comment, so I updated it to `# noqa: I001` and it continues to sort that line. Digging through issues I saw Ruff should also respect `# isort:skip` and discovered it does respect that. Config is pretty simple.

Sorts/errors anyway:
```
...
from tests.factories.user_insurance_factory import create_user_insurance

from app import main  # noqa: I001
...
```
Does not sort:
```
...
from tests.factories.user_insurance_factory import create_user_insurance

from app import main  # isort:skip
...
```

Using VSCode and the Ruff extension, it errors and will auto move the line.
Running `ruff check <filename>` it removes the line entirely (main is not used directly in the file)

Ruff version: `ruff 0.1.13`
Ruff config:
```
[lint]
select = [
    # pycodestyle
    "E",
    # Pyflakes
    "F",
    # isort - Import sort
    "I",
]

# Files excluded from just linting
exclude = [
    "**/__init__.py",
    "app/shared/models/**.pyi",
    "app/main.py",
    "venv/*",
]

[lint.isort]
force-single-line = true
force-sort-within-sections = true
order-by-type = true
single-line-exclusions = ["typing"]
known-local-folder = ["src", "tests", "scripts"]
```

---

_Comment by @charliermarsh on 2024-01-16 19:12_

I think the `# noqa: I001` needs to go on the last line of the entire import block -- it can't be applied to individual imports.

---

_Comment by @monotkate on 2024-01-16 19:19_

Oh interesting. So `isort: skip` is functionality I cannot replicate with `noqa: I001`? The skip seems to label a specific line as not an import according to isort docs so that it doesn't get moved by isort.

`from app import main  # noqa: I001` is the last line of the import block. There's code after it, but all the imports are before it.

---

_Comment by @charliermarsh on 2024-01-16 19:29_

Yeah, they're functionally a bit different from one another. `# noqa: I001` will turn off import sorting for the entire block of imports preceding it, while `# isort: skip` will turn off import sorting for that one import (roughly).


---

_Comment by @monotkate on 2024-01-16 19:33_

In theory, this should not being erroring then, is that correct?

<img width="456" alt="image" src="https://github.com/astral-sh/ruff/assets/11742296/0ea336dc-be10-40ee-a0c3-27ac22c4d338">


---

_Comment by @charliermarsh on 2024-01-16 19:34_

Sorry, I was mistaken, it needs to go on the _first_ line, not the last: https://play.ruff.rs/583348e6-e041-491b-b5cb-4f71b3d1d730

---

_Comment by @monotkate on 2024-01-16 19:39_

Gotcha, that helps me understand what I'm working with, and should cover my issue, thank you! Is there documentation on this somewhere that I missed? I couldn't find anything about how to use `noqa: I001` in the docs, and only found the `isort: skip` part digging through issues.

---

_Comment by @charliermarsh on 2024-01-16 19:43_

Sadly no, for the `# isort: skip` action comments we've kind of lazily deferred to the isort documentation. I'm going to add a note to the docs about `# noqa: I001` though -- we have a similar note for multiline strings which are also a little bit special. I'll link this issue to that PR.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-16 19:43_

---

_Label `documentation` added by @charliermarsh on 2024-01-16 19:43_

---

_Comment by @monotkate on 2024-01-16 19:44_

As a note: it's unexpected that you can't ignore single lines. I could use this for circular dependency issues so that one line gets pinned but the others are sorted. I'll use the isort skip for now, but it's a little odd to use another tool's commands. Thanks for the fast response and the future docs!

---

_Comment by @charliermarsh on 2024-01-16 19:46_

Would you be able to provide a concrete example of how you'd use it on a single line, and what you'd expect to happen?

---

_Comment by @monotkate on 2024-01-16 19:53_

Yep! This isn't my code specifically, so excuse me if I don't know all the details. I'm more familiar with how this results in TS rather than Python.

`from app import main` needs to be the first import so that certain things are imported in the correct order, or else the code results in circular dependencies that causes issues. Outside of that single line, idc what the order of the imports are, and would prefer them to be sorted according to isort.

I can add the isort skip to that line and it neatly ignores it in terms of all the other imports, resolving my use case.

Does that make sense?

---

_Referenced in [astral-sh/ruff#9555](../../astral-sh/ruff/pulls/9555.md) on 2024-01-16 20:16_

---

_Closed by @charliermarsh on 2024-01-16 20:19_

---

_Referenced in [astral-sh/ruff#16774](../../astral-sh/ruff/issues/16774.md) on 2025-03-16 12:37_

---
