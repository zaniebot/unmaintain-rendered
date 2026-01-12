```yaml
number: 4881
title: Add user-facing output to indicate PEP 723 script
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/pep
created_at: 2024-07-08T00:49:39Z
updated_at: 2024-07-08T13:58:20Z
url: https://github.com/astral-sh/uv/pull/4881
synced_at: 2026-01-12T16:06:30Z
```

# Add user-facing output to indicate PEP 723 script

---

_@charliermarsh_

_No description provided._

---

_Label `cli` added by @charliermarsh on 2024-07-08 01:24_

---

_Label `preview` added by @charliermarsh on 2024-07-08 01:24_

---

_@zanieb reviewed on 2024-07-08 01:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:71 on 2024-07-08 01:49_

Can we refer to "inline script metadata" instead of the PEP? I think it'd be good to normalize the term and discuss things as the user-facing concept.

---

_@charliermarsh reviewed on 2024-07-08 01:52_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:71 on 2024-07-08 01:52_

Ok

---

_@zanieb reviewed on 2024-07-08 01:59_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/run.rs`:71 on 2024-07-08 01:59_

A bit of a nit, but we've already read it right? And the user passed us this script name already? Should we just say "Using inline script metadata for requirements" (a little unsure about "requirements")

---

_@zanieb approved on 2024-07-08 01:59_

---

_Merged by @charliermarsh on 2024-07-08 02:05_

---

_Closed by @charliermarsh on 2024-07-08 02:05_

---

_Branch deleted on 2024-07-08 02:05_

---

_@charliermarsh reviewed on 2024-07-08 13:58_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/run.rs`:71 on 2024-07-08 13:58_

I would prefer "Using inline script metadata for: {}"

---
