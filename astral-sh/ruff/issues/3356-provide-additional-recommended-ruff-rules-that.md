---
number: 3356
title: Provide additional recommended ruff rules that points at others
type: issue
state: closed
author: qarmin
labels:
  - configuration
  - core
assignees: []
created_at: 2023-03-06T07:04:11Z
updated_at: 2023-03-08T22:21:57Z
url: https://github.com/astral-sh/ruff/issues/3356
synced_at: 2026-01-10T01:22:42Z
---

# Provide additional recommended ruff rules that points at others

---

_Issue opened by @qarmin on 2023-03-06 07:04_

Currently ruff support ~500 rules in ~50 categories.

I tried to create a set of rules that would be as generic as possible, but it didn't work out very well for me(mostly probably because I only use Python for a few small projects).

I am currently using a hand-picked mix of F, E, SIM, A, B rules

I think rule sets pointing to other style rules like:

- Ruff minimal (RM)
- Ruff minimal autofixable (RMA)
- Ruff recommended (RR)
- Ruff recommended autofixable (RRA)
- Ruff advanced (RA)
- Ruff advanced autofixable (RAA)

would be useful, would work well without the need for configuration

---

_Comment by @charliermarsh on 2023-03-06 13:20_

Yeah I'd really like to support ESLint-like "presets" for this. It needs scoping-out...

In the meantime, maybe I'll mention that if _I'm_ looking for a minimal setup, I tend to use something like this in my own projects:

```toml
[tool.ruff]
select = ["E", "F", "B", "UP"]
```

---

_Label `configuration` added by @charliermarsh on 2023-03-06 16:13_

---

_Label `core` added by @charliermarsh on 2023-03-06 16:13_

---

_Comment by @charliermarsh on 2023-03-06 17:52_

Created a Discussion around this: https://github.com/charliermarsh/ruff/discussions/3363

---

_Comment by @charliermarsh on 2023-03-08 22:21_

(Just closing in favor of the linked Discussion.)

---

_Closed by @charliermarsh on 2023-03-08 22:21_

---
