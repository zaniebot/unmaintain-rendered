```yaml
number: 13541
title: "[DOC] Explain `uv sync` in the projects guide for working on existing projects"
type: issue
state: open
author: turbotimon
labels:
  - documentation
  - enhancement
assignees: []
created_at: 2025-05-19T18:52:39Z
updated_at: 2025-05-21T16:16:12Z
url: https://github.com/astral-sh/uv/issues/13541
synced_at: 2026-01-10T03:41:47Z
```

# [DOC] Explain `uv sync` in the projects guide for working on existing projects

---

_Issue opened by @turbotimon on 2025-05-19 18:52_

### Summary

More and more projects using uv, that great! But the current guide only explains how to initialize a new project but not what to do when you start working on a project that already uses uv. This confused me at first, and many colleagues I have spoken to have had the same experience.

Therefore, I propose to add a small but explicit section on working on existing projects to the guide:

#13542

---

_Label `enhancement` added by @turbotimon on 2025-05-19 18:52_

---

_Comment by @zanieb on 2025-05-19 19:13_

i.e., you think we should move more of https://docs.astral.sh/uv/concepts/projects/sync/#automatic-lock-and-sync into the guide document?

---

_Label `documentation` added by @zanieb on 2025-05-19 19:14_

---

_Assigned to @zanieb by @zanieb on 2025-05-19 19:14_

---

_Comment by @turbotimon on 2025-05-20 07:21_

@zanieb I wouldn't move too much and have to maintain it in two location. Also, I like that the guides are rather compact and more details can be found under concepts. But it's a good point, what do you think about briefly introduce it and link to the page you mentioned?

Something like:

```
## Working on an existing project

If you start working on a project that is already managed by uv, simply run `uv sync`. A virtual
environment will be created (if one does not already exist) and all dependencies from the `uv.lock`
file will be installed. Due to the automatic locking and synchronisation in uv, this also applies to other
commands such as `uv run` or `uv lock`.

See the [locking and syncing documentation](../concepts/projects/sync.md)
for details.

```


---

_Comment by @turbotimon on 2025-05-21 16:16_

(PR is updated)

---
