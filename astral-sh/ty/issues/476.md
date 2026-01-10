```yaml
number: 476
title: "`disallow_untyped_defs` similar behavior"
type: issue
state: closed
author: pmareke
labels:
  - question
assignees: []
created_at: 2025-05-21T19:36:10Z
updated_at: 2025-05-21T20:36:34Z
url: https://github.com/astral-sh/ty/issues/476
synced_at: 2026-01-10T02:34:10Z
```

# `disallow_untyped_defs` similar behavior

---

_Issue opened by @pmareke on 2025-05-21 19:36_

### Question

First of all thanks for this amazing tool, even though it's not production ready it's quite impressive how it fast it is.

One thing that I'm missing (sorry if it's already talked, but I didn't find any reference to it) it's the possibility to have a similar behavior to MYPY's `disallow_untyped_defs`.

Do you have it in your radar? Or it's something that ty will not support?

As I said, thanks for your job. Not just **ty**, but also **uv** and **ruff**.

### Version

_No response_

---

_Label `question` added by @pmareke on 2025-05-21 19:36_

---

_Comment by @carljm on 2025-05-21 19:42_

Our opinion is that such a rule is out of scope for a _typechecker_, but is a great opt-in rule to provide in a _linter_. For some rules this might require a type-aware linter (which we ultimately aim to provide), but `disallow-untyped-defs` is a much simpler rule that doesn't even require type information to provide. In fact it's already provided by Ruff! See ANN001, ANN002, and ANN003 here: https://docs.astral.sh/ruff/rules/#flake8-annotations-ann

---

_Comment by @pmareke on 2025-05-21 20:36_

Thanks so much @carljm, it has all the sense.

I was using **ruff**, bur just as a formatter. Definitely I'll take a look to it as a linter too.

---

_Closed by @pmareke on 2025-05-21 20:36_

---
