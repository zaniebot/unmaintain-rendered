```yaml
number: 4187
title: "Show `Applicability` in Diagnostics"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
assignees: []
created_at: 2023-05-02T09:38:51Z
updated_at: 2023-10-17T03:02:56Z
url: https://github.com/astral-sh/ruff/issues/4187
synced_at: 2026-01-12T15:54:44Z
```

# Show `Applicability` in Diagnostics

---

_@MichaReiser_

This task is part of #4181 and depends on #4183

I don't know if we have the necessary information because I believe we only generate fixes if `--fix` is present but it would be nice if the `TextEmitter` (and other emitters) would indicate whether a fix is safe or unsafe. E.g. label with "Safe fix" vs "Suggested Fix"

---

_Label `help wanted` added by @MichaReiser on 2023-05-02 09:39_

---

_Closed by @zanieb on 2023-10-17 03:02_

---
