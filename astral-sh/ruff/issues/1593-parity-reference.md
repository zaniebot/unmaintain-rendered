---
number: 1593
title: Parity Reference
type: issue
state: closed
author: danieleades
labels:
  - documentation
assignees: []
created_at: 2023-01-03T11:21:38Z
updated_at: 2023-01-04T22:07:45Z
url: https://github.com/astral-sh/ruff/issues/1593
synced_at: 2026-01-10T01:22:39Z
---

# Parity Reference

---

_Issue opened by @danieleades on 2023-01-03 11:21_

The readme is pretty comprehensive in showing the supported lints, in relation to their corresponding flake8-plugins. I'm more interested in the inverse.

That is, I have a codebase that makes extensive use of flake8 plugins for linting. I would like a reference of which plugins can be *completely* replaced by ruff so that I can incrementally remove them where they overlap.

Is there some way to parse this?

For example, under the readme section for flake8-bugbear I see a list of all of the bugbear lints supported by Ruff. Short of manually cross-referencing against the bugbear docs, is there some way for me to check whether i can remove the bugbear plugin entirely by using ruff?

---

_Comment by @danieleades on 2023-01-03 11:52_

Another example- when replacing isort with ruff (at least in my repo) the package isn't automatically detected by ruff as a 'known first party' module, so this config needs to be added

---

_Comment by @bluetech on 2023-01-03 11:55_

For me https://github.com/charliermarsh/ruff#how-does-ruff-compare-to-flake8 answered this question -- the ones with `X/Y` are incomplete, the rest are complete.

---

_Comment by @danieleades on 2023-01-03 12:09_

> For me https://github.com/charliermarsh/ruff#how-does-ruff-compare-to-flake8 answered this question -- the ones with `X/Y` are incomplete, the rest are complete.

it comes close, though if we confine the dicussion only to the examples i give above-

- ruff doesn't implement all of bugbear's 'opinionated' lints
- the fact that isort infers the root package and ruff does not is not documented

---

_Label `documentation` added by @charliermarsh on 2023-01-03 18:38_

---

_Comment by @charliermarsh on 2023-01-04 16:02_

Thanks @danieleades. I'll add a section to note a few of those incompatibilities.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-04 16:02_

---

_Comment by @charliermarsh on 2023-01-04 21:01_

> Another example- when replacing isort with ruff (at least in my repo) the package isn't automatically detected by ruff as a 'known first party' module, so this config needs to be added

If you post your project structure, I'm happy to help with the configuration. Ruff typically _can_ infer the root, unless you're using namespace packages.


---

_Referenced in [astral-sh/ruff#1639](../../astral-sh/ruff/pulls/1639.md) on 2023-01-04 21:05_

---

_Closed by @charliermarsh on 2023-01-04 21:05_

---

_Comment by @danieleades on 2023-01-04 22:07_

> > Another example- when replacing isort with ruff (at least in my repo) the package isn't automatically detected by ruff as a 'known first party' module, so this config needs to be added
> 
> If you post your project structure, I'm happy to help with the configuration. Ruff typically _can_ infer the root, unless you're using namespace packages.
> 

Sure - dont think I'm doing anything particularly strange

https://github.com/danieleades/sphinx-graph

By the way, it only affected the 'tests' directory. Isort and ruff didn't agree on how to group the imports in tests/ unless ruff had the additional config.

---

_Referenced in [astral-sh/ruff#14651](../../astral-sh/ruff/issues/14651.md) on 2024-11-28 06:26_

---
