---
number: 6462
title: "Docs request: how `target-version` can support `>=` syntax"
type: issue
state: closed
author: jamesbraza
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-08-09T21:18:46Z
updated_at: 2023-08-10T17:11:39Z
url: https://github.com/astral-sh/ruff/issues/6462
synced_at: 2026-01-07T13:12:15-06:00
---

# Docs request: how `target-version` can support `>=` syntax

---

_Issue opened by @jamesbraza on 2023-08-09 21:18_

I support Python>=3.10 and use `ruff==0.0.284`.  I am wondering how to specify `>=3.10` using `target-version`.

Can we add to the [`target-version`](https://beta.ruff.rs/docs/settings/#target-version) docs how to specify `>=` relations?  Or alternately, can we document that one should set `target-version` to be the lowest version in the supported range?

---

_Comment by @zanieb on 2023-08-09 21:23_

Hey!

`target-version` is your _minimum_ compatible version so you'd use `3.10`. We also infer this setting from the "requires-python" `pyproject.toml` setting.

The documentation already states

> The minimum Python version to target

How would you propose clarifying it?

---

_Label `documentation` added by @zanieb on 2023-08-09 21:23_

---

_Comment by @jamesbraza on 2023-08-09 21:27_

Okay gotchu, my eyes must have filtered out "minimum" text.  I propose adding an example, something like:

> The minimum Python version to target, e.g., when considering automatic code upgrades, like rewriting type annotations. For example, to represent supporting Python >= 3.10, specify `target-version = "py310"`.

---

_Comment by @zanieb on 2023-08-09 21:34_

Sure! I'd be happy to review a pull request adding that.

---

_Label `good first issue` added by @zanieb on 2023-08-09 21:34_

---

_Referenced in [astral-sh/ruff#6482](../../astral-sh/ruff/pulls/6482.md) on 2023-08-10 16:31_

---

_Closed by @zanieb on 2023-08-10 17:11_

---
