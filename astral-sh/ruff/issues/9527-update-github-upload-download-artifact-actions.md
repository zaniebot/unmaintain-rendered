```yaml
number: 9527
title: Update GitHub Upload/Download artifact actions
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - ci
assignees: []
created_at: 2024-01-15T09:35:16Z
updated_at: 2024-02-29T17:57:08Z
url: https://github.com/astral-sh/ruff/issues/9527
synced_at: 2026-01-10T11:09:51Z
```

# Update GitHub Upload/Download artifact actions

---

_Issue opened by @MichaReiser on 2024-01-15 09:35_

GitHub updated the upload/download artifact actions in a non-backward compatible way (the main issue is that artifacts now need a unique name). 

We must update our release workflow to use unique names to update the upload/download artifact actions.

See GitHub's [Migration guide](https://github.com/actions/download-artifact/blob/main/docs/MIGRATION.md).

---

_Label `help wanted` added by @MichaReiser on 2024-01-15 09:35_

---

_Label `ci` added by @MichaReiser on 2024-01-15 09:35_

---

_Comment by @MichaReiser on 2024-01-15 09:36_

You can see the errors in the build logs of https://github.com/astral-sh/ruff/pull/9526 

---

_Closed by @MichaReiser on 2024-02-29 17:57_

---
