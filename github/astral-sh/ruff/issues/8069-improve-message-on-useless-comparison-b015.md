---
number: 8069
title: "Improve message on \"useless-comparison\" (B015)"
type: issue
state: closed
author: inoa-jboliveira
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2023-10-19T17:20:33Z
updated_at: 2023-10-28T11:18:03Z
url: https://github.com/astral-sh/ruff/issues/8069
synced_at: 2026-01-07T13:12:15-06:00
---

# Improve message on "useless-comparison" (B015)

---

_Issue opened by @inoa-jboliveira on 2023-10-19 17:20_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
ruff version 0.1.0 (and likely any older version)

Currently using B015 we get the following message:

    B015 Pointless comparison. This comparison does nothing but waste CPU instructions. Either prepend `assert` or remove it.

This message is very unhelpful because no ones writes `foo = bar` in a line and cares about wasting CPU instructions. It just sounds like this is one of those annoying fixes that won't catch any bugs.

The actual use case for this test should be when someone types `==` by accident and wanted to type `=` instead.

like `data['success'] == False` instead of `data['success'] = False`

This message should be something in the lines:

    B015 Pointless comparison. This comparison does nothing. Did you mean to assign a value? Otherwise prepend `assert` or remove it.



---

_Label `documentation` added by @charliermarsh on 2023-10-20 21:18_

---

_Comment by @charliermarsh on 2023-10-20 21:18_

Agree, this could be improved.

---

_Comment by @zanieb on 2023-10-20 21:20_

The existing message does have the befit of being really silly :D

I like your proposal!

---

_Label `good first issue` added by @zanieb on 2023-10-20 21:20_

---

_Referenced in [astral-sh/ruff#8295](../../astral-sh/ruff/pulls/8295.md) on 2023-10-28 03:49_

---

_Closed by @charliermarsh on 2023-10-28 11:18_

---
