```yaml
number: 16554
title: "Improve `uv init` error for invalid directory names"
type: pull_request
state: merged
author: terror
labels:
  - error messages
assignees: []
merged: true
base: main
head: invalid-dir-init-error
created_at: 2025-11-02T22:08:44Z
updated_at: 2025-11-03T00:26:52Z
url: https://github.com/astral-sh/uv/pull/16554
synced_at: 2026-01-12T16:12:19Z
```

# Improve `uv init` error for invalid directory names

---

_@terror_

Resolves https://github.com/astral-sh/uv/issues/16433

When `uv init` infers a project name from the working directory, directories with characters outside the PEP 503 rules produced the generic “Not a valid package or extra name” message that didn’t explain the source of the problem. This change intercepts that failure, reports whether the current or explicit target directory caused it, and tells the user to supply an explicit `--name`.

---

_@charliermarsh approved on 2025-11-03 00:26_

Excellent PR, thanks!

---

_Label `error messages` added by @charliermarsh on 2025-11-03 00:26_

---

_Merged by @charliermarsh on 2025-11-03 00:26_

---

_Closed by @charliermarsh on 2025-11-03 00:26_

---

_Branch deleted on 2025-11-03 00:26_

---
