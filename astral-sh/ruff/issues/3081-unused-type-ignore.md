---
number: 3081
title: unused-type-ignore
type: issue
state: open
author: janosh
labels:
  - rule
  - type-inference
assignees: []
created_at: 2023-02-21T03:05:54Z
updated_at: 2023-02-28T22:42:46Z
url: https://github.com/astral-sh/ruff/issues/3081
synced_at: 2026-01-10T01:22:41Z
---

# unused-type-ignore

---

_Issue opened by @janosh on 2023-02-21 03:05_

Very tall ask considering I don't see how to do this without depending on `mypy` but wanted to at least float the suggestion.

Similar to how useless `# noqa` directives are removed by [`RUF100`](https://beta.ruff.rs/docs/rules/#ruff-specific-rules-ruf), it would be cool to auto-remove `# type: ignore` on lines that no longer issue a `mypy` error.

---

_Label `rule` added by @charliermarsh on 2023-02-21 15:58_

---

_Comment by @charliermarsh on 2023-02-21 16:00_

Haha yeah we can't support this right now. But I'd like to support it in the future :eyes: I'll leave it open for now!

---

_Comment by @stinodego on 2023-02-21 19:06_

@janosh In case you didn't know, `mypy` can at least warn you when this happens (but doesn't autofix it for you):
https://mypy.readthedocs.io/en/stable/command_line.html#cmdoption-mypy-warn-unused-ignores

---

_Comment by @janosh on 2023-02-21 19:16_

@stinodego Thanks for bringing that up, should have mentioned it myself. I'm already using that flag. `mypy` is just painfully slow so if `ruff` could do it, would be a big QoL improvement.

---

_Comment by @twoertwein on 2023-02-22 18:29_

While I would like this feature, I think it is out of scope for ruff:

- per [PEP 484](https://peps.python.org/pep-0484/#compatibility-with-other-uses-of-function-annotations) `typing: ignore` is not mypy-specific but intended for all type checkers (I assume ruff doesn't want to implement all type checkers)
- mypy/pyright/... and the python type system are still changing, does ruff really want to keep up with this (sounds like a lot of work for little benefit given that type checkers themselves have this check already)? It would also mean having to know the version of mypy and reading its config file to know which rules are used.

---

_Label `type-inference` added by @charliermarsh on 2023-02-28 22:42_

---
