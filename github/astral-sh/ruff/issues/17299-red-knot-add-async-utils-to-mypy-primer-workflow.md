---
number: 17299
title: "[red-knot] Add `async-utils` to mypy_primer workflow"
type: issue
state: closed
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
created_at: 2025-04-08T18:17:33Z
updated_at: 2025-04-09T07:43:23Z
url: https://github.com/astral-sh/ruff/issues/17299
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Add `async-utils` to mypy_primer workflow

---

_Issue opened by @AlexWaygood on 2025-04-08 18:17_

Mypy_primer hasn't had much to say about @dcreager's recent generics work, because we've been doing new-style generics first, and not very many projects "in the wild" use new-style generics yet (they're only valid syntax on Python 3.12+). We should consider adding https://github.com/mikeshardmind/async-utils to our mypy_primer workflow, as it makes use of PEP-695 generics, so will give us valuable signal on how well our implementation is progressing.

This involves either making a PR to https://github.com/hauntsaninja/mypy_primer or our Astral fork (https://github.com/astral-sh/mypy_primer) to add the project to `project.py`. Then, once that's done, we need to update the workflow here to explicitly select the project during the `mypy_primer` CI job: https://github.com/astral-sh/ruff/blob/058439d5d3f65db515d69359c99838d60776f45e/.github/workflows/mypy_primer.yaml#L71

(cc. @sharkdp, who introduced our primer workflow to CI)

---

_Label `ci` added by @AlexWaygood on 2025-04-08 18:17_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-08 18:17_

---

_Referenced in [astral-sh/mypy_primer#1](../../astral-sh/mypy_primer/pulls/1.md) on 2025-04-08 18:41_

---

_Referenced in [astral-sh/ruff#17303](../../astral-sh/ruff/pulls/17303.md) on 2025-04-09 07:36_

---

_Closed by @sharkdp on 2025-04-09 07:43_

---

_Referenced in [hauntsaninja/mypy_primer#167](../../hauntsaninja/mypy_primer/issues/167.md) on 2025-05-04 22:03_

---
