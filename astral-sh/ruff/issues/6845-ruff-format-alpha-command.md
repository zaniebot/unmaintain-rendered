```yaml
number: 6845
title: "ruff `format` alpha command"
type: issue
state: closed
author: konstin
labels:
  - formatter
assignees: []
created_at: 2023-08-24T08:21:15Z
updated_at: 2023-08-28T15:25:48Z
url: https://github.com/astral-sh/ruff/issues/6845
synced_at: 2026-01-10T11:09:49Z
```

# ruff `format` alpha command

---

_Issue opened by @konstin on 2023-08-24 08:21_

We would like gather some feedback on the current state of the formatter. We plan to add a beta `ruff format` command that works on stdin (esp. for lsp integration), single files and an entire project (for now, with the same includes/excludes as ruff itself). This command will lack a lot of options and features we want in the final version, the only additional configuration we'll respect is ruff's `line-length`. This command is intended to be tried out and not yet adopted into production projects.

---

_Assigned to @konstin by @konstin on 2023-08-24 08:21_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-24 08:22_

---

_Renamed from "ruff `format` beta command" to "ruff `format` alpha command" by @MichaReiser on 2023-08-24 08:30_

---

_Label `formatter` added by @MichaReiser on 2023-08-24 08:31_

---

_Comment by @MichaReiser on 2023-08-24 09:12_

Related issue https://github.com/astral-sh/ruff/issues/6611

---

_Comment by @MichaReiser on 2023-08-28 12:27_

@charliermarsh do you know if this is completed?

---

_Comment by @MichaReiser on 2023-08-28 12:28_

I guess we could polish the command by

* Removing unsupported options from the help text
* Print the number of checked and formatted files (with time spent?)

---

_Comment by @charliermarsh on 2023-08-28 14:20_

Yeah I’m gonna polish this off today.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-08-28 14:20_

---

_Unassigned @konstin by @charliermarsh on 2023-08-28 14:20_

---

_Comment by @charliermarsh on 2023-08-28 15:25_

Closing for now!

---

_Closed by @charliermarsh on 2023-08-28 15:25_

---
