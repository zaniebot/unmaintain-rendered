```yaml
number: 4597
title: "Treat Python version as a lower bound in `--universal`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/py
created_at: 2024-06-27T18:29:01Z
updated_at: 2024-06-27T18:41:47Z
url: https://github.com/astral-sh/uv/pull/4597
synced_at: 2026-01-10T13:48:28Z
```

# Treat Python version as a lower bound in `--universal`

---

_Pull request opened by @charliermarsh on 2024-06-27 18:29_

## Summary

Closes https://github.com/astral-sh/uv/issues/4591.


---

_Review requested from @zanieb by @charliermarsh on 2024-06-27 18:29_

---

_Review requested from @konstin by @charliermarsh on 2024-06-27 18:29_

---

_Label `bug` added by @charliermarsh on 2024-06-27 18:29_

---

_@konstin approved on 2024-06-27 18:31_

---

_Comment by @konstin on 2024-06-27 18:32_

Do we have an issue for considering `requires-python` when using `uv pip compile --universal pyproject.toml`?

---

_Comment by @charliermarsh on 2024-06-27 18:33_

We don't, but I'm undecided on whether we should... Probably? But it would mean that the `uv pip` API now reads from your "project" which it doesn't do today. (Maybe fine but blurs some boundaries.)

---

_Comment by @konstin on 2024-06-27 18:36_

I thought about discovering `pyproject.toml` but i think we shouldn't for that reason, but when we're already reading `project.dependencies` i think we can also read `project.requires-python`. Not high-priority though.

---

_Comment by @zanieb on 2024-06-27 18:41_

I agree with that logic :)

---

_@zanieb approved on 2024-06-27 18:41_

---

_Merged by @charliermarsh on 2024-06-27 18:41_

---

_Closed by @charliermarsh on 2024-06-27 18:41_

---

_Branch deleted on 2024-06-27 18:41_

---
