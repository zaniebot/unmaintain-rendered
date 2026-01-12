```yaml
number: 13591
title: Support ruff discovery in pip build environments
type: pull_request
state: merged
author: gaborbernat
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-pip-env-discovery-fails
created_at: 2024-10-01T15:12:22Z
updated_at: 2024-10-03T17:52:09Z
url: https://github.com/astral-sh/ruff/pull/13591
synced_at: 2026-01-12T15:55:44Z
```

# Support ruff discovery in pip build environments

---

_@gaborbernat_

Resolves https://github.com/astral-sh/ruff/issues/13321.

Contents of overlay:
```bash
/private/var/folders/v0/l8q3ghks2gs5ns2_p63tyqh40000gq/T/pip-build-env-e0ukpbvo/overlay/bin:
total 26M
-rwxr-xr-x 1 bgabor8 staff 26M Oct  1 08:22 ruff
drwxr-xr-x 3 bgabor8 staff  96 Oct  1 08:22 .
drwxr-xr-x 4 bgabor8 staff 128 Oct  1 08:22 ..
```

Python executable:
```bash
'/Users/bgabor8/git/github/ruff-find-bin-during-build/.venv/bin/python'
```
PATH is:
```bash
['/private/var/folders/v0/l8q3ghks2gs5ns2_p63tyqh40000gq/T/pip-build-env-e0ukpbvo/overlay/bin',
 '/private/var/folders/v0/l8q3ghks2gs5ns2_p63tyqh40000gq/T/pip-build-env-e0ukpbvo/normal/bin',
'/Library/Frameworks/Python.framework/Versions/3.11/bin',
'/Library/Frameworks/Python.framework/Versions/3.12/bin',
```
Not sure where to add tests, there does not seem to be any existing one. Can someone help me with that?

---

_Marked ready for review by @gaborbernat on 2024-10-01 15:40_

---

_Comment by @github-actions[bot] on 2024-10-01 15:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2024-10-03 14:35_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-03 14:35_

---

_Label `bug` added by @charliermarsh on 2024-10-03 14:36_

---

_Comment by @charliermarsh on 2024-10-03 14:53_

I can take this one.

---

_@charliermarsh approved on 2024-10-03 17:32_

---

_Comment by @charliermarsh on 2024-10-03 17:33_

I'm comfortable merging assuming you tested it for your own workflow.

---

_Merged by @charliermarsh on 2024-10-03 17:38_

---

_Closed by @charliermarsh on 2024-10-03 17:38_

---

_Branch deleted on 2024-10-03 17:43_

---

_Comment by @gaborbernat on 2024-10-03 17:44_

> I'm comfortable merging assuming you tested it for your own workflow.

pip does not allow installing build dependencies from git so could not really test it...

---
