---
number: 10465
title: Long SPDX-FileCopyrightText lines cause rule E501 violations
type: issue
state: closed
author: damonlynch
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-03-18T22:08:32Z
updated_at: 2024-03-19T19:57:04Z
url: https://github.com/astral-sh/ruff/issues/10465
synced_at: 2026-01-07T13:12:15-06:00
---

# Long SPDX-FileCopyrightText lines cause rule E501 violations

---

_Issue opened by @damonlynch on 2024-03-18 22:08_

First, let me say thank you for a great project. Keep up the great work!

A lengthy single-line `SPDX-FileCopyrightText` statement in the source code header can unfortunately easily violate rule E501.

For example, this innocuous line is 89 characters:
`# SPDX-FileCopyrightText: Copyright 2012-2015 Jim Easterbrook <jim@jim-easterbrook.me.uk>`

It is unclear to me how to direct ruff to ignore this error on a per-line basis, because being a machine-read string, obviously the line cannot end with `# noqa: E501`.

My guess is that ruff should by default ignore rule E501 violations when dealing with SPDX headers (and potentially any other machine-read source code headers that adhere to a rigid specification).

Please note the specifications explicitly recommend not using multi-line statements, so that's not a viable workaround. 

In example above, to keep it at 88 characters or below, it appears the only current workaround is to use `©️` in place of the text `Copyright`, but that will not be sufficient in all circumstances.

**Relevant Links**
[SPDX specification](https://spdx.github.io/spdx-spec/v2.3/file-tags/)
[REUSE specification](https://reuse.software/spec/)

---

_Comment by @charliermarsh on 2024-03-19 00:55_

Yeah I think we should consider disabling E501 enforcement for SPDX headers.

---

_Label `bug` added by @charliermarsh on 2024-03-19 00:55_

---

_Label `help wanted` added by @MichaReiser on 2024-03-19 08:52_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-19 15:10_

---

_Referenced in [astral-sh/ruff#10481](../../astral-sh/ruff/pulls/10481.md) on 2024-03-19 18:36_

---

_Closed by @charliermarsh on 2024-03-19 19:57_

---
