```yaml
number: 137
title: "Support `--extra` flag in `pip-compile`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-10-19T14:04:16Z
updated_at: 2023-10-31T19:44:41Z
url: https://github.com/astral-sh/uv/issues/137
synced_at: 2026-01-12T15:58:22Z
```

# Support `--extra` flag in `pip-compile`

---

_@charliermarsh_

_No description provided._

---

_Label `good first issue` added by @konstin on 2023-10-19 15:14_

---

_Label `enhancement` added by @charliermarsh on 2023-10-24 19:22_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-24 19:22_

---

_Label `good first issue` removed by @charliermarsh on 2023-10-24 19:31_

---

_Assigned to @zanieb by @zanieb on 2023-10-24 19:39_

---

_Comment by @konstin on 2023-10-26 14:43_

Relevant for that: https://peps.python.org/pep-0685/

I don't know if we got that right elsewhere fwiw, this is a rather new PEP

---

_Comment by @charliermarsh on 2023-10-26 15:14_

We probably want an `ExtraName` abstraction like `PackageName` for this.

---

_Comment by @zanieb on 2023-10-30 21:35_

Do we want support for `--all-extras` too?

---

_Comment by @charliermarsh on 2023-10-30 21:41_

Probably worth supporting.

---

_Comment by @zanieb on 2023-10-31 19:44_

Done in #239 

---

_Closed by @zanieb on 2023-10-31 19:44_

---
