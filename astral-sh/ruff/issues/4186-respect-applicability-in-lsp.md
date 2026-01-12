```yaml
number: 4186
title: "Respect `Applicability` in LSP"
type: issue
state: closed
author: MichaReiser
labels: []
assignees: []
created_at: 2023-05-02T09:34:14Z
updated_at: 2023-11-10T05:29:24Z
url: https://github.com/astral-sh/ruff/issues/4186
synced_at: 2026-01-12T15:54:44Z
```

# Respect `Applicability` in LSP

---

_@MichaReiser_

This task is part of #4181 and it depends on #4183 and #4185

* Double check if we need to change the LSP fix command to use `--fix-unsafe` when applying a code fix. 
* Change the `fixAll` action handler to only apply safe fixes by default
* Introduce a new option to also run unsafe fixes as part of `fixAll`

---

_Comment by @zanieb on 2023-10-07 00:32_

Discussed more at https://github.com/astral-sh/ruff/issues/6962#issuecomment-1696845966

---

_Assigned to @zanieb by @zanieb on 2023-10-07 00:32_

---

_Comment by @dhruvmanila on 2023-10-12 05:56_

Additionally, would having `unsafe-fixes = true` in the config mean that the `fixAll` code action _includes_ them?

---

_Comment by @zanieb on 2023-10-12 13:37_

Yes, I think it should.

---

_Comment by @charliermarsh on 2023-11-10 05:29_

I think applicability is now respected in the LSP but @zanieb correct me if I'm wrong.

---

_Closed by @charliermarsh on 2023-11-10 05:29_

---
