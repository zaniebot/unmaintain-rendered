```yaml
number: 970
title: "Waterfall effect in `ProjectFilesWalker::collect_vec`"
type: issue
state: closed
author: MichaReiser
labels:
  - performance
assignees: []
created_at: 2025-08-12T07:08:34Z
updated_at: 2025-08-14T18:38:40Z
url: https://github.com/astral-sh/ty/issues/970
synced_at: 2026-01-12T15:54:24Z
```

# Waterfall effect in `ProjectFilesWalker::collect_vec`

---

_@MichaReiser_

<img width="3360" height="1055" alt="Image" src="https://github.com/user-attachments/assets/cf325c22-fb1b-469f-a175-61b96150e44c" />

We walk the project files in parallel but all the `system_path_to_file` calls happen on the main thread. This is mainly because `dyn Db` can't be shared across threads. 

There are a few solutions:

1. Add a `Files::from_walk_entry` method (or similar) that returns a `File` from a walk entry. Extend walkentry to contain all the information needed to create a `File`
2. Change `ProjectFilesWalker` to take a &ProjectDatabase`, which allows cloning

---

_Label `performance` added by @MichaReiser on 2025-08-12 07:08_

---

_Closed by @MichaReiser on 2025-08-14 18:38_

---
