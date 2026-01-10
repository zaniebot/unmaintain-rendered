---
number: 4175
title: "Consider adding `D204` but for functions"
type: issue
state: closed
author: CobaltCause
labels:
  - question
assignees: []
created_at: 2023-05-01T23:43:14Z
updated_at: 2023-06-28T23:59:20Z
url: https://github.com/astral-sh/ruff/issues/4175
synced_at: 2026-01-10T01:22:43Z
---

# Consider adding `D204` but for functions

---

_Issue opened by @CobaltCause on 2023-05-01 23:43_

This would allow for enforcing uniformity between function and class docstrings so they both look like this:

```
class definition or function signature
    docstring

    body
```

Note the blank line between the docstring and body.

---

_Comment by @charliermarsh on 2023-05-02 01:02_

This is probably a result of the conventions defined in [PEP 257](https://peps.python.org/pep-0257/#multi-line-docstrings), which recommends a blank line after the class docstring to mirror the blank line between methods in a class, but doesn't make any explicit recommendation for function docstrings. It looks like Black lets you do either.

---

_Comment by @charliermarsh on 2023-05-02 01:10_

I think I'm gonna close in the interest of minimizing configuration complexity. There isn't really a good home for this right now, since ideally it'd be part of the `pydocstyle` rule set, but then we'd be adding rules that don't exist upstream, and users with `D` enabled would have to explicitly ignore it. That _can_ be fine, but there are tradeoffs involved, and here, I'm not sure that the upside is worth it.

---

_Label `question` added by @charliermarsh on 2023-05-02 01:10_

---

_Comment by @CobaltCause on 2023-05-02 01:47_

> users with `D` enabled would have to explicitly ignore it

The option I'm proposing would conflict with `D202` and I see `ruff` can already emit warnings for conflicts and automatically choose one, so that might be a possible migration path for this particular case.

---

_Closed by @charliermarsh on 2023-06-28 23:59_

---
