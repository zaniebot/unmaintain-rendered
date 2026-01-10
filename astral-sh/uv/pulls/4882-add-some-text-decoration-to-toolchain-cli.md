```yaml
number: 4882
title: Add some text decoration to toolchain CLI
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/toolchain-cli
created_at: 2024-07-08T01:19:24Z
updated_at: 2024-07-08T14:03:40Z
url: https://github.com/astral-sh/uv/pull/4882
synced_at: 2026-01-10T13:42:52Z
```

# Add some text decoration to toolchain CLI

---

_Pull request opened by @charliermarsh on 2024-07-08 01:19_

## Summary

Attempts to make the CLI output a little more consistent with the `pip` interface. I opted to make the Python versions, requests, and filenames blue, and the keys green, but open to opinions on that. (We use blue for filenames elsewhere.)

Closes #4813.
Closes https://github.com/astral-sh/uv/issues/4814.

![Screenshot 2024-07-07 at 9 18 48 PM](https://github.com/astral-sh/uv/assets/1309177/8518b559-196b-4cd0-bc16-8e79e66460bb)


---

_Review requested from @zanieb by @charliermarsh on 2024-07-08 01:19_

---

_Review requested from @konstin by @charliermarsh on 2024-07-08 01:19_

---

_Label `cli` added by @charliermarsh on 2024-07-08 01:19_

---

_Label `preview` added by @charliermarsh on 2024-07-08 01:19_

---

_@charliermarsh reviewed on 2024-07-08 01:19_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:77 on 2024-07-08 01:19_

I also tried bold for these but it felt too loud.

---

_@charliermarsh reviewed on 2024-07-08 01:41_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:77 on 2024-07-08 01:41_

Maybe yellow is more subtle.

---

_Comment by @charliermarsh on 2024-07-08 01:44_

Other inspiration here could be Cargo which bolds + colors the operation rather than the argument:

![Screenshot 2024-07-07 at 9 44 31 PM](https://github.com/astral-sh/uv/assets/1309177/3ba94f9f-7aa1-45c2-9d11-d4c81a12a810)


---

_@zanieb approved on 2024-07-08 14:02_

Let's merge as an incremental improvement and we can futz more later?

---

_Merged by @charliermarsh on 2024-07-08 14:03_

---

_Closed by @charliermarsh on 2024-07-08 14:03_

---

_Branch deleted on 2024-07-08 14:03_

---
