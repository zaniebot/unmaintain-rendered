```yaml
number: 12553
title: "UV pip compile doesn't include main dependencies when using group"
type: issue
state: closed
author: Lyranile
labels:
  - question
assignees: []
created_at: 2025-03-30T07:59:55Z
updated_at: 2025-08-04T18:21:04Z
url: https://github.com/astral-sh/uv/issues/12553
synced_at: 2026-01-12T16:01:06Z
```

# UV pip compile doesn't include main dependencies when using group

---

_@Lyranile_

### Summary

I'm expecting uv pip compile --group test to include the main dependencies specified in 

pyproject
Dependencies: [
   Somepackage
]

Group test
Dependencies: [
    Someotherpackage
]

There is also no all-groups or extra-groups, and a lot of the other parameters that uv sync has

--group main also didn't work despite working like that in poetry

### Platform

macos 14

### Version

0.6.10

### Python version

3.13.2

---

_Label `bug` added by @Lyranile on 2025-03-30 07:59_

---

_Comment by @charliermarsh on 2025-03-30 15:07_

I believe that `uv pip compile --group test` omits the main dependencies because that matches pip's semantics. You want `uv pip compile --group dev pyproject.toml` (which is syntactic sugar for `uv pip compile --group pyproject.toml:dev pyproject.toml`). In other words: "include the `dev` group from `pyproject.toml`, plus the production dependencies from `pyproject.toml`).

(`--group main` is a Poetry-specific construct and not something that's standardized -- I doubt we would support it.)

---

_Label `bug` removed by @charliermarsh on 2025-03-30 15:07_

---

_Label `question` added by @charliermarsh on 2025-03-30 15:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-30 15:08_

---

_Closed by @charliermarsh on 2025-03-30 15:09_

---

_Comment by @sirusbaladi on 2025-08-04 18:14_

We need this badly. 

something like: uv pip compile --group test --core 

Where it gives the default dependencies + group test

---

_Comment by @zanieb on 2025-08-04 18:20_

Why not `uv pip compile --group test pyproject.toml`?

---

_Comment by @zanieb on 2025-08-04 18:21_

(as suggested in the comment above...)

---
