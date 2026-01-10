```yaml
number: 13855
title: "List `.gitignore` in project init files"
type: pull_request
state: merged
author: AbhiUpadhye
labels:
  - documentation
assignees: []
merged: true
base: main
head: AbhiUpadhye-patch-1
created_at: 2025-06-05T06:55:03Z
updated_at: 2025-06-05T14:21:16Z
url: https://github.com/astral-sh/uv/pull/13855
synced_at: 2026-01-10T11:10:42Z
```

# List `.gitignore` in project init files

---

_Pull request opened by @AbhiUpadhye on 2025-06-05 06:55_

Fixes https://github.com/astral-sh/uv/issues/13639

Just added a missing `.gitignore` file in the docs, so that there would be no confusion for readers referring it.

Thank you!

---

_Comment by @zanieb on 2025-06-05 13:15_

Is this true?

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ ls -lah example
total 24
drwxr-xr-x   6 zb  staff   192B Jun  5 08:15 .
drwxr-xr-x  58 zb  staff   1.8K Jun  5 08:15 ..
-rw-r--r--   1 zb  staff     7B Jun  5 08:15 .python-version
-rw-r--r--   1 zb  staff     0B Jun  5 08:15 README.md
-rw-r--r--   1 zb  staff    85B Jun  5 08:15 main.py
-rw-r--r--   1 zb  staff   155B Jun  5 08:15 pyproject.toml
```

---

_Comment by @charliermarsh on 2025-06-05 13:27_

Yup!

```
❯ uv init foo
Initialized project `foo` at `/Users/crmarsh/workspace/foo`
~/workspace via 🐍 v3.11.6
❯ ls -lah foo/
total 32
drwxr-xr-x    8 crmarsh  staff   256B Jun  5 09:26 .
drwxr-xr-x  303 crmarsh  staff   9.5K Jun  5 09:26 ..
drwxr-xr-x    9 crmarsh  staff   288B Jun  5 09:26 .git
-rw-r--r--    1 crmarsh  staff   109B Jun  5 09:26 .gitignore
-rw-r--r--    1 crmarsh  staff     5B Jun  5 09:26 .python-version
-rw-r--r--    1 crmarsh  staff     0B Jun  5 09:26 README.md
-rw-r--r--    1 crmarsh  staff    81B Jun  5 09:26 main.py
-rw-r--r--    1 crmarsh  staff   149B Jun  5 09:26 pyproject.toml
```

It looks like you're initializing within `uv` which is already a Git repo, so we don't create a nested `.git` or `.gitignore` IIRC.

---

_Comment by @zanieb on 2025-06-05 14:20_

Oh! That makes sense.

---

_@zanieb approved on 2025-06-05 14:20_

---

_Label `documentation` added by @zanieb on 2025-06-05 14:21_

---

_Renamed from "Fixes #13639: Update projects.md" to "List `.gitignore` in project init files" by @zanieb on 2025-06-05 14:21_

---

_Merged by @zanieb on 2025-06-05 14:21_

---

_Closed by @zanieb on 2025-06-05 14:21_

---
