```yaml
number: 12074
title: Docs is not updated to v0.5.0
type: issue
state: closed
author: yairp03
labels:
  - documentation
  - release
assignees: []
created_at: 2024-06-27T16:24:12Z
updated_at: 2024-06-28T11:29:05Z
url: https://github.com/astral-sh/ruff/issues/12074
synced_at: 2026-01-12T15:54:51Z
```

# Docs is not updated to v0.5.0

---

_@yairp03_

The docs weren't updated to comply with v0.5.0 changes.  
For example, there is no deprecated note on [E999](https://docs.astral.sh/ruff/rules/syntax-error/) and [RUF024](https://docs.astral.sh/ruff/rules/mutable-fromkeys-value/) is still in preview.

---

_Label `documentation` added by @AlexWaygood on 2024-06-27 16:35_

---

_Comment by @MichaReiser on 2024-06-27 16:37_

Thanks for reporting. I wasn't aware that we need to start the action to publish the documentation manually. It's now running and that should hopefully fix it. I'll update the contribution guidelines. 

https://github.com/astral-sh/ruff/actions/runs/9700308469/job/26771392459

---

_Comment by @zanieb on 2024-06-27 16:41_

I don't think that should need to be manual, presumably this is an oversight because docs are not published in uv. cc @charliermarsh 

---

_Label `release` added by @zanieb on 2024-06-27 16:41_

---

_Comment by @MichaReiser on 2024-06-27 16:41_

And it just finished and the docs are up to date.

---

_Assigned to @charliermarsh by @MichaReiser on 2024-06-27 16:49_

---

_Comment by @MichaReiser on 2024-06-27 16:49_

Assigning to you @charliermarsh to look into if this should be triggered automatically

---

_Comment by @charliermarsh on 2024-06-27 16:51_

Yup, will fix!

---

_Closed by @charliermarsh on 2024-06-28 11:29_

---
