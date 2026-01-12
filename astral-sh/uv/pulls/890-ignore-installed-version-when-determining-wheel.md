```yaml
number: 890
title: Ignore installed version when determining wheel compatibility
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/target
created_at: 2024-01-12T02:19:06Z
updated_at: 2024-01-12T13:57:01Z
url: https://github.com/astral-sh/uv/pull/890
synced_at: 2026-01-12T16:04:15Z
```

# Ignore installed version when determining wheel compatibility

---

_@charliermarsh_

_No description provided._

---

_Comment by @charliermarsh on 2024-01-12 02:19_

@konstin - Do you mind testing this branch for me on your machine?

---

_Label `bug` added by @charliermarsh on 2024-01-12 02:19_

---

_Review requested from @konstin by @charliermarsh on 2024-01-12 02:25_

---

_Comment by @konstin on 2024-01-12 09:21_

I had tried that, the problem is that the tags are only for installed and don't get patched ([pip-compile](https://github.com/astral-sh/puffin/blob/55f2be72e2c81ccc9b3514de6e2afe10bbff9bbe/crates/puffin-cli/src/commands/pip_compile.rs#L127-L132))

---

_Comment by @charliermarsh on 2024-01-12 13:47_

@konstin - Ok, I think I understand. More involved but probably still fixable. Should I merge this anyway? Seems correct.

---

_Comment by @konstin on 2024-01-12 13:47_

Yeah this looks like a strict improvement.

I can take over the tags if you want since i have the test setup.

---

_@konstin approved on 2024-01-12 13:47_

---

_Merged by @charliermarsh on 2024-01-12 13:57_

---

_Closed by @charliermarsh on 2024-01-12 13:57_

---

_Branch deleted on 2024-01-12 13:57_

---
