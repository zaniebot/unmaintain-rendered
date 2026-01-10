---
number: 3480
title: "Add `# via` source messages for `setup.py` and `setup.cfg`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2024-05-09T04:37:52Z
updated_at: 2024-05-09T13:30:42Z
url: https://github.com/astral-sh/uv/issues/3480
synced_at: 2026-01-10T01:23:28Z
---

# Add `# via` source messages for `setup.py` and `setup.cfg`

---

_Issue opened by @charliermarsh on 2024-05-09 04:37_

I think the way to do this is: make `RequirementsSpecification::source_trees` the full file (rather than the directory)... in `SourceTreeResolver`, add the source path to the requirements that are returned at the end.

We probably also need to change `source: PathBuf` on `Requirement` to something like...

```rust
enum Source {
  File(PathBuf),
  Project(Option<String>, PathBuf),
}
```

Where the `Option<String>` is the name of the project.


---

_Label `enhancement` added by @charliermarsh on 2024-05-09 04:37_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-09 04:49_

---

_Referenced in [astral-sh/uv#3481](../../astral-sh/uv/pulls/3481.md) on 2024-05-09 05:05_

---

_Closed by @charliermarsh on 2024-05-09 13:30_

---
