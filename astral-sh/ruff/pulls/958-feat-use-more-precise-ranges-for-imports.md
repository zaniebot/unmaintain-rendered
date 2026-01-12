```yaml
number: 958
title: "feat: use more precise ranges for imports"
type: pull_request
state: merged
author: relsunkaev
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-29T04:02:42Z
updated_at: 2022-11-30T00:37:11Z
url: https://github.com/astral-sh/ruff/pull/958
synced_at: 2026-01-12T05:48:46Z
```

# feat: use more precise ranges for imports

---

_Pull request opened by @relsunkaev on 2022-11-29 04:02_

Closes #946

- All import bindings except "*" now use range from `alias`
- Consolidated `fix` functions for plain and `from` imports
- Removed `ImportKind` enum (unused)
- Reporting unused `from` imports separately

---

_Comment by @charliermarsh on 2022-11-29 04:42_

Rad, love what I'm seeing in the PR summary + all the red in the diff! Look forward to reviewing.

---

_Comment by @charliermarsh on 2022-11-29 05:05_

(I'm on ET so I'll probably review in the morning.)

---

_Review requested from @charliermarsh by @charliermarsh on 2022-11-29 18:10_

---

_Comment by @charliermarsh on 2022-11-29 22:51_

(Got delayed today but will get to this tonight.)

---

_Comment by @charliermarsh on 2022-11-30 00:01_

This is a great PR -- no major feedback here. I'm really impressed, this is a tricky part of the code.

---

_Merged by @charliermarsh on 2022-11-30 00:01_

---

_Closed by @charliermarsh on 2022-11-30 00:01_

---

_Comment by @relsunkaev on 2022-11-30 00:37_

Thanks! The codebase is pretty clear even without much documentation.

---
