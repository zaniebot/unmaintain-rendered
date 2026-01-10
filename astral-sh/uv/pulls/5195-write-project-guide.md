```yaml
number: 5195
title: Write project guide
type: pull_request
state: merged
author: ibraheemdev
labels:
  - documentation
  - preview
assignees: []
merged: true
base: main
head: ibraheem/project-guide
created_at: 2024-07-18T18:41:32Z
updated_at: 2024-07-19T00:45:27Z
url: https://github.com/astral-sh/uv/pull/5195
synced_at: 2026-01-10T13:42:52Z
```

# Write project guide

---

_Pull request opened by @ibraheemdev on 2024-07-18 18:41_

## Summary

Write the project guide that was added in https://github.com/astral-sh/uv/pull/5135.

I tried to expand on details as much as I felt was necessary for someone new to python package managers (which was myself a couple months ago).

---

_Label `documentation` added by @ibraheemdev on 2024-07-18 18:41_

---

_Label `preview` added by @ibraheemdev on 2024-07-18 18:41_

---

_Review requested from @zanieb by @ibraheemdev on 2024-07-18 18:41_

---

_Review request for @zanieb removed by @ibraheemdev on 2024-07-18 18:41_

---

_Review requested from @charliermarsh by @ibraheemdev on 2024-07-18 18:41_

---

_Review requested from @zanieb by @ibraheemdev on 2024-07-18 18:41_

---

_@ibraheemdev reviewed on 2024-07-18 18:47_

---

_Review comment by @ibraheemdev on `docs/guides/projects.md`:29 on 2024-07-18 18:47_

This is assuming that `uv init` creates the venv and lockfile. I found that this made it a little easier to explain things than having a distinction between "the things that are created by `uv init`" and "the things that are created the first time you execute `uv run`".

---

_@ibraheemdev reviewed on 2024-07-18 19:00_

---

_Review comment by @ibraheemdev on `docs/guides/projects.md`:3 on 2024-07-18 19:00_

Feel like we need a better tagline here.

---

_Assigned to @zanieb by @zanieb on 2024-07-18 19:04_

---

_Review comment by @ibraheemdev on `docs/guides/projects.md`:80 on 2024-07-18 19:16_

Are we recommending to check this in to VCS (does that depend on application vs. library)?

---

_@ibraheemdev reviewed on 2024-07-18 19:16_

---

_@zanieb reviewed on 2024-07-18 19:17_

---

_Review comment by @zanieb on `docs/guides/projects.md`:80 on 2024-07-18 19:17_

I think it should always be checked into VCS

---

_@zanieb reviewed on 2024-07-19 00:32_

---

_Review comment by @zanieb on `docs/guides/projects.md`:3 on 2024-07-19 00:32_

Maybe just define a project e.g.: uv is capable of managing projects using the `pyproject.toml` standard.


---

_Review comment by @zanieb on `docs/guides/projects.md`:29 on 2024-07-19 00:38_

I think it's fine to create them both personally. Maybe we just want an opt-out flag?

---

_@zanieb reviewed on 2024-07-19 00:38_

---

_@zanieb approved on 2024-07-19 00:45_

Going to merge and iterate on it after. Thanks!

---

_Merged by @zanieb on 2024-07-19 00:45_

---

_Closed by @zanieb on 2024-07-19 00:45_

---

_Branch deleted on 2024-07-19 00:45_

---
