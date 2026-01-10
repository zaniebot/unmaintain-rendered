```yaml
number: 4131
title: "`uv lock` fails if the default python version doesn't match project python version"
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-06-07T15:21:35Z
updated_at: 2024-06-10T14:36:15Z
url: https://github.com/astral-sh/uv/issues/4131
synced_at: 2026-01-10T05:31:37Z
```

# `uv lock` fails if the default python version doesn't match project python version

---

_Issue opened by @konstin on 2024-06-07 15:21_

I have an environment where `python` is 3.12 and a project with `requires-python = "==3.11.*"`. This makes `uv lock` fail:

```console
$ cargo run -q -- lock --preview
  × No solution found when resolving dependencies:
  ╰─▶ Because the current Python version (3.12.3) does not satisfy Python>=3.11.dev0,<3.12.dev0 and warehouse==1.0.0 depends on
      Python>=3.11.dev0,<3.12.dev0, we can conclude that warehouse==1.0.0 cannot be used.
      And because only warehouse==1.0.0 is available and warehouse depends on warehouse, we can conclude that the requirements are
      unsatisfiable.
```

The better behavior would be `uv lock` finding the python 3.11 that i have in path.

---

_Comment by @charliermarsh on 2024-06-07 15:50_

I thought you solved this by making `init_environment` respect `Requires-Python`?

---

_Comment by @konstin on 2024-06-07 16:08_

It's in https://github.com/astral-sh/uv/pull/3945

---

_Label `preview` added by @charliermarsh on 2024-06-07 18:34_

---

_Closed by @charliermarsh on 2024-06-10 14:36_

---
