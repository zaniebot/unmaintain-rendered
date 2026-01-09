---
number: 5725
title: Workspace wildcard member should only match directory
type: issue
state: closed
author: jpambrun-vida
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-02T12:40:25Z
updated_at: 2024-08-02T19:44:45Z
url: https://github.com/astral-sh/uv/issues/5725
synced_at: 2026-01-07T13:12:17-06:00
---

# Workspace wildcard member should only match directory

---

_Issue opened by @jpambrun-vida on 2024-08-02 12:40_

I have a project with the form
```
service/
   README.md
   service_a/
     pyproject.toml
     src/
   service_b/
     pyproject.toml
     src/
```

with
```toml
members = [
  "services/*"
]
```

I get
```
 uv sync
error: failed to read from file `/####/services/README.md/pyproject.toml`
  Caused by: Not a directory (os error 20)
```

A simple fix would be to filter out files. 
It also complains if any subfolder doesn't contain a pyproject.toml. Would it be better if it would ignore them?
Being able to recursively search for pyproject.toml would also be nice.

---

_Comment by @charliermarsh on 2024-08-02 12:44_

That's funny, I just added #5724.

> It also complains if any subfolder doesn't contain a pyproject.toml. Would it be better if it would ignore them?

You should mark any such directories as excluded via `workspace.exclude`.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-02 12:44_

---

_Label `bug` added by @charliermarsh on 2024-08-02 12:44_

---

_Label `preview` added by @charliermarsh on 2024-08-02 12:44_

---

_Comment by @jpambrun-vida on 2024-08-02 13:07_

I know about exclude, but I imagined it was to explicitly exclude a folder with a pyproject.toml while not erroring out of folder not containing any.

I can see it both ways.. 

---

_Comment by @charliermarsh on 2024-08-02 13:49_

Yeah, Cargo rejects directories that are listed as members but aren't valid packages, so I think we followed that precedent.

---

_Referenced in [astral-sh/uv#5735](../../astral-sh/uv/pulls/5735.md) on 2024-08-02 19:35_

---

_Closed by @charliermarsh on 2024-08-02 19:44_

---

_Closed by @charliermarsh on 2024-08-02 19:44_

---
