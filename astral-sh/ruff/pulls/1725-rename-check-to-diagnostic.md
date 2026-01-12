```yaml
number: 1725
title: "Rename `Check` to `Diagnostic`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/diagnostic
created_at: 2023-01-07T22:59:53Z
updated_at: 2023-01-08T22:46:24Z
url: https://github.com/astral-sh/ruff/pull/1725
synced_at: 2026-01-12T15:55:06Z
```

# Rename `Check` to `Diagnostic`

---

_@charliermarsh_

Along with:

- `CheckKind` -> `DiagnosticKind`
- `CheckCode` -> `RuleCode`
- `CheckCodePrefix` -> `RuleCodePrefix`

---

_Comment by @charliermarsh on 2023-01-07 23:00_

\cc @not-my-profile 

---

_Comment by @not-my-profile on 2023-01-08 03:30_

I think we should probably rather go for `RuleCode` and `RuleCodePrefix` ... I think that makes more sense.

These codes/prefixes are mainly used to enable or disable rules (a user-facing concept). Whereas Diagnostic IMO is just an implementation detail. So I think it makes more sense to name these enums after the user-facing concept since "rule code" and "rule code prefix" will probably show up in the user documentation to replace our current "check code" / "error code" references.

---

_Comment by @charliermarsh on 2023-01-08 04:14_

Yeah, that makes sense. The fact that it appears in the docs is probably a good signal there too. (I actually briefly considered just `Code` and `CodePrefix` but those are probably too vague.)

---

_Merged by @charliermarsh on 2023-01-08 22:46_

---

_Closed by @charliermarsh on 2023-01-08 22:46_

---

_Branch deleted on 2023-01-08 22:46_

---
