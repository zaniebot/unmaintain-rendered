---
number: 12110
title: RUF025 duplicates unimplemented rule C420
type: issue
state: closed
author: tjkuson
labels:
  - rule
assignees: []
created_at: 2024-06-30T12:02:40Z
updated_at: 2024-08-12T10:43:13Z
url: https://github.com/astral-sh/ruff/issues/12110
synced_at: 2026-01-07T13:12:15-06:00
---

# RUF025 duplicates unimplemented rule C420

---

_Issue opened by @tjkuson on 2024-06-30 12:02_

[unnecessary-dict-comprehension-for-iterable (RUF025)](https://docs.astral.sh/ruff/rules/unnecessary-dict-comprehension-for-iterable/) now exists as a flake8-comprehensions rule (https://github.com/adamchainz/flake8-comprehensions/pull/553).

Although C420 has not been implemented yet, it would make sense (and seems to match what has been done before) to merge the rules in favour of the flake8 plugin. Although the rule request was made to Ruff first (https://github.com/astral-sh/ruff/issues/9592, https://github.com/adamchainz/flake8-comprehensions/issues/552), my understanding is that Ruff favours strong coupling to flake8.

search terms: RUF025, flake8-comprehensions, duplicate, duplication

Related to https://github.com/astral-sh/ruff/issues/2714

---

_Comment by @charliermarsh on 2024-06-30 14:47_

Cool, makes sense to me to redirect it to C420! Thanks for doing that.

---

_Label `rule` added by @charliermarsh on 2024-06-30 14:47_

---

_Added to milestone `v0.6` by @MichaReiser on 2024-06-30 15:56_

---

_Referenced in [astral-sh/ruff#12322](../../astral-sh/ruff/issues/12322.md) on 2024-07-14 19:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-26 14:27_

---

_Referenced in [astral-sh/ruff#12533](../../astral-sh/ruff/pulls/12533.md) on 2024-07-26 14:36_

---

_Closed by @MichaReiser on 2024-08-12 10:43_

---
