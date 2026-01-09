---
number: 17438
title: Naming the new Python Type Checker
type: issue
state: closed
author: T-256
labels:
  - ty
assignees: []
created_at: 2025-04-17T01:02:15Z
updated_at: 2025-04-22T07:47:53Z
url: https://github.com/astral-sh/ruff/issues/17438
synced_at: 2026-01-07T13:12:16-06:00
---

# Naming the new Python Type Checker

---

_Issue opened by @T-256 on 2025-04-17 01:02_

As red-knot project going to alpha state, opened this discussion to make decision about naming of it.

Despite shared structures between Ruff and red-knot, it seems they're going to be separated apps (at least for alpha and beta releases) so it deserves to have a good name.

Here is my personal suggestion:

### `pyte`

Pros:

- short: it's 4 length word as same as `ruff` and `knot`.
- started with `pyt` which is half of with `python` word.
- inspired by `type` world by in-placing t with p.

Cons:

- it's already reserved in Pypi.
- it can be multiple pronounced: _pait_ OR _peet_.


This discussion created to gather more ideas, drop your suggestions about other names.

---

_Comment by @daviewales on 2025-04-17 01:14_

`rype`

Starts with `r` for 'rust'.
Ends with `ype` for 'type'.
4 characters.
Available on [PyPI](https://pypi.org/search/?q=rype).

---

_Label `red-knot` added by @AlexWaygood on 2025-04-17 08:05_

---

_Added to milestone `Red Knot Alpha` by @AlexWaygood on 2025-04-17 08:06_

---

_Comment by @lukeshingles on 2025-04-17 10:28_

Does it need a separate name? The ruff linter (ruff check) and ruff formatter (ruff format) don't. Would the command be something like "ruff typecheck"? Or it could be integrated into "ruff check" with new categories of lint error for type issues?

---

_Comment by @T-256 on 2025-04-17 22:37_

I like to have it as subcommand of ruff too, but as of current state we have executable called `knot` which has its own separated cli, configuration, lsp, diagnostics, playground, etc. So, I guess that's going to keep its current state in alpha and beta releases too.

---

_Locked by @MichaReiser on 2025-04-22 07:47_

---
