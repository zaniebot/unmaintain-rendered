---
number: 13525
title: Auto-disable ANN rules when a type-checker is detected
type: issue
state: closed
author: paduszyk
labels: []
assignees: []
created_at: 2024-09-26T13:47:26Z
updated_at: 2024-09-26T16:08:34Z
url: https://github.com/astral-sh/ruff/issues/13525
synced_at: 2026-01-07T13:12:15-06:00
---

# Auto-disable ANN rules when a type-checker is detected

---

_Issue opened by @paduszyk on 2024-09-26 13:47_

Ruff is a great linter and formatter, we all don't have any doubts 👍🏼 Thank you guys for maintaining this project!

However, Ruff also includes some rules related to static type-checking, for example: ANN. 

Could it be possible to set up Ruff to automatically turn off those rules if a type-checker is configured, let us say `mypy`? In such a scenario, extra "type-checking" with Ruff looks like a redundancy to me.

---

_Comment by @MichaReiser on 2024-09-26 16:02_

> Ruff is a great linter and formatter, we all don't have any doubts 👍🏼 Thank you guys for maintaining this project!

Thanks for the kind words

> Could it be possible to set up Ruff to automatically turn off those rules if a type-checker is configured, let us say mypy? In such a scenario, extra "type-checking" with Ruff looks like a redundancy to me.

I see the motivation for this but we very intentionally avoid reading configurations from other tools to avoid surprising behavior and incompatibilities (we don't know the other tool's version and ruff would break if the tool changes its configuration). 

> However, Ruff also includes some rules related to static type-checking, for example: ANN.

Some of those rules indeed overlap with what some type checkers do, but I consider their nature to be mostly linting (it enforces a specific style) rather than correct typing. I think it's best if users disable the rules manually if they know that their type checker of choice already covers them.


---

_Closed by @MichaReiser on 2024-09-26 16:02_

---

_Comment by @paduszyk on 2024-09-26 16:08_

OK. Thanks for an elaborate answer. 🙂

---
