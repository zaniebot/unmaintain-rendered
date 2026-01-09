---
number: 17649
title: Linter and formatter conflicts with docstrings
type: issue
state: closed
author: Frakko
labels: []
assignees: []
created_at: 2025-04-26T21:21:55Z
updated_at: 2025-04-26T21:23:35Z
url: https://github.com/astral-sh/ruff/issues/17649
synced_at: 2026-01-07T13:12:16-06:00
---

# Linter and formatter conflicts with docstrings

---

_Issue opened by @Frakko on 2025-04-26 21:21_

### Summary

Hello, I'm having a few conflict issues between the ruff formatter and the ruff linter when it comes to docstrings.

I will start by giving a few examples of how the *formatter* handles docstrings based on my experience with it:
1. A docstring placed right under a *class* declaration cannot have a blank line above (between docstring and declaration), but a blank line below is enforced.
2. A docstring placed right under a *function* declaration cannot have a blank line above, but a blank line below is arbitrary (meaning it's kept the way I manually structure it).
3. A docstring placed in the middle of a code block (e.g. in between the variable declarations) blank lines above *and* below are arbitrary (kept the way I manually structure it).

This (more or less) follows the guidelines in [PEP257](https://peps.python.org/pep-0257/) which is my preferred way of formatting my code.

### Version

_No response_

---

_Closed by @Frakko on 2025-04-26 21:22_

---

_Comment by @Frakko on 2025-04-26 21:23_

(apologies, mis-clicked. didn't mean to submit, can be deleted)

---
