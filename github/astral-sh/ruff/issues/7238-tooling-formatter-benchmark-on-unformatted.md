---
number: 7238
title: "Tooling: Formatter benchmark on unformatted projects"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-09-08T08:10:11Z
updated_at: 2023-10-18T10:11:42Z
url: https://github.com/astral-sh/ruff/issues/7238
synced_at: 2026-01-07T13:12:15-06:00
---

# Tooling: Formatter benchmark on unformatted projects

---

_Issue opened by @MichaReiser on 2023-09-08 08:10_

Benchmark Ruff's formatter compatibility on *unblacked* projects by

* Format the project with Ruff
* Format the same project with Black
* Compare the outputs

---

_Label `formatter` added by @MichaReiser on 2023-09-08 08:10_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 08:10_

---

_Assigned to @konstin by @MichaReiser on 2023-09-27 13:29_

---

_Comment by @MichaReiser on 2023-10-18 08:57_

@konstin I believe you worked on this. Would you mind documenting your findings and closing the issue if there's nothing else to do?

---

_Comment by @konstin on 2023-10-18 10:11_

I checked cpython and pretix and filed issues for cases black looks better for `ruff format . && black .`

---

_Closed by @konstin on 2023-10-18 10:11_

---
