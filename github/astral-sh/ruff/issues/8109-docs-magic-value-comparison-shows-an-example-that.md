---
number: 8109
title: "Docs: `magic-value-comparison` shows an example that is not flagged"
type: issue
state: closed
author: bersbersbers
labels:
  - documentation
assignees: []
created_at: 2023-10-21T18:05:41Z
updated_at: 2023-10-22T05:15:49Z
url: https://github.com/astral-sh/ruff/issues/8109
synced_at: 2026-01-07T13:12:15-06:00
---

# Docs: `magic-value-comparison` shows an example that is not flagged

---

_Issue opened by @bersbersbers on 2023-10-21 18:05_

The code example at https://docs.astral.sh/ruff/rules/magic-value-comparison/ shows an example that is not flagged.

Here's a demo and an example that is actually flagged:
https://play.ruff.rs/0cd75dbb-5452-4b44-ac7b-ef1cc92e6d6e

---

_Referenced in [microsoft/pyright#6216](../../microsoft/pyright/issues/6216.md) on 2023-10-21 18:08_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-21 19:31_

---

_Label `documentation` added by @charliermarsh on 2023-10-21 19:31_

---

_Comment by @charliermarsh on 2023-10-21 19:36_

Thanks, will fix...

---

_Referenced in [astral-sh/ruff#8111](../../astral-sh/ruff/pulls/8111.md) on 2023-10-21 22:56_

---

_Closed by @charliermarsh on 2023-10-21 23:05_

---

_Comment by @bersbersbers on 2023-10-22 05:15_

I haven't checked, but I wouldn't be surprised if the new code was flagged by https://docs.astral.sh/ruff/rules/superfluous-else-return/ now ;)

---
