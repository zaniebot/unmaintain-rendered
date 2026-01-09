---
number: 16629
title: "`ignore-one-line-docstrings` is not respected for `D401`"
type: issue
state: closed
author: tylerlaprade
labels:
  - question
assignees: []
created_at: 2025-03-11T15:23:13Z
updated_at: 2025-03-27T12:14:50Z
url: https://github.com/astral-sh/ruff/issues/16629
synced_at: 2026-01-07T13:12:16-06:00
---

# `ignore-one-line-docstrings` is not respected for `D401`

---

_Issue opened by @tylerlaprade on 2025-03-11 15:23_

### Summary

I'm unable to repro in the playground, but locally in both VSCode and the CLI I am getting warnings about one-line docstrings.

<img width="1497" alt="Image" src="https://github.com/user-attachments/assets/c567bd24-ae33-43d9-afe1-cb235a7a88bd" />

<img width="313" alt="Image" src="https://github.com/user-attachments/assets/37e22d73-c8e9-488e-a374-68f19a7e0883" />

### Version

ruff 0.9.10 (0dfa810e9 2025-03-07)

---

_Comment by @hofrob on 2025-03-11 17:11_

I assume [ignore-one-line-docstrings](https://docs.astral.sh/ruff/settings/#lint_pydoclint_ignore-one-line-docstrings) is only applicable for pydoclint rules. D401 is part of pydocstyle though.

---

_Label `question` added by @MichaReiser on 2025-03-12 08:56_

---

_Closed by @MichaReiser on 2025-03-27 12:14_

---
