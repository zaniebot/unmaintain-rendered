```yaml
number: 6969
title: Declaring script dependencies in jupyter notebook first cell
type: issue
state: open
author: EtienneT
labels:
  - enhancement
assignees: []
created_at: 2024-09-03T13:52:45Z
updated_at: 2024-09-05T11:29:19Z
url: https://github.com/astral-sh/uv/issues/6969
synced_at: 2026-01-12T15:59:09Z
```

# Declaring script dependencies in jupyter notebook first cell

---

_@EtienneT_

This is a feature request to be able to [declare script dependencies](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) directly in the first cell of a jupyter notebook.  This way we could have completely isolated environment per notebook.  We would have to be able to use `uv run` with jupyter notebooks and the vscode scenario would also need to be looked at with extensions, but this would be awesome.

---

_Label `enhancement` added by @zanieb on 2024-09-03 17:23_

---

_Comment by @schlich on 2024-09-05 03:44_

[marimo](https://github.com/marimo-team/marimo/releases) implemented this in their last couple of releases, check them out

---

_Comment by @EtienneT on 2024-09-05 11:29_

> [marimo](https://github.com/marimo-team/marimo/releases) implemented this in their last couple of releases, check them out

Just saw that, I will definitely have a look!

---
