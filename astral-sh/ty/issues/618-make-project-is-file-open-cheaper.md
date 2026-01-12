```yaml
number: 618
title: "Make `project.is_file_open` cheaper"
type: issue
state: open
author: MichaReiser
labels:
  - performance
assignees: []
created_at: 2025-06-09T16:05:19Z
updated_at: 2025-07-24T17:15:23Z
url: https://github.com/astral-sh/ty/issues/618
synced_at: 2026-01-12T15:54:23Z
```

# Make `project.is_file_open` cheaper

---

_@MichaReiser_

`project.is_file_open` can be pretty expensive because it requires indexing all project files. The main reason for it is to check if a file isn't gitignored. 

This isn't as much of a problem for the CLI where we index all files anyway, but it is somewhat costly for the VS code extension.

It would be great if `is_file_open` could determine whether a file is included simply on its path without requiring a full traversal.

Related to https://github.com/astral-sh/ty/issues/97

---

_Label `performance` added by @MichaReiser on 2025-06-09 16:05_

---

_Comment by @MichaReiser on 2025-07-23 09:18_

The method has been renamed to `should_check_file`

---

_Comment by @CodeMan62 on 2025-07-24 17:08_

I think we can use `is_file_included` here. will try to do it asap

---

_Comment by @MichaReiser on 2025-07-24 17:15_

I'm not entirely sure how to fix this yet. I don't think I can provide the guidance you need on this task

---
