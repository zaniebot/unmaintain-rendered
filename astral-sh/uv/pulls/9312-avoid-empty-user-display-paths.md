```yaml
number: 9312
title: Avoid empty user display paths
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/avoid-displaying-an-empty-path
created_at: 2024-11-21T11:36:05Z
updated_at: 2024-11-21T19:56:51Z
url: https://github.com/astral-sh/uv/pull/9312
synced_at: 2026-01-12T16:08:45Z
```

# Avoid empty user display paths

---

_@konstin_

Currently, user display returns an empty path if the current dir is the directory we are printing. This leads to odd messages such as

 > Including project.license-files at `` with `LICENSE*`

or

> Not a license files match: ``

Instead, we display the current path as a dot.


---

_Label `enhancement` added by @konstin on 2024-11-21 11:36_

---

_Review requested from @BurntSushi by @konstin on 2024-11-21 11:36_

---

_Review requested from @charliermarsh by @konstin on 2024-11-21 11:36_

---

_@BurntSushi approved on 2024-11-21 12:34_

For just display the current directory, I do kind of like `./` better than just a `.`. I find the former a little more readable/unambiguous, but I don't feel strongly.

---

_Comment by @charliermarsh on 2024-11-21 14:38_

I think `PortablePath` handles this in the lockfile at least?

---

_Comment by @konstin on 2024-11-21 14:57_

This is for user facing messages (log messages and error messages), in which I'd use the system path separator over portable paths. (At least i think we'd give Windows user a harder time than necessary by switching the path separators when they debugging their globbing)

---

_Merged by @charliermarsh on 2024-11-21 19:56_

---

_Closed by @charliermarsh on 2024-11-21 19:56_

---

_Branch deleted on 2024-11-21 19:56_

---
