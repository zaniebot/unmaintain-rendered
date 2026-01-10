```yaml
number: 3693
title: "Let `RequirementSource::Path.editable` be `bool`, not `Option<bool>`"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/not-option-editable-bool
created_at: 2024-05-21T11:34:14Z
updated_at: 2024-05-21T14:34:45Z
url: https://github.com/astral-sh/uv/pull/3693
synced_at: 2026-01-10T14:32:20Z
```

# Let `RequirementSource::Path.editable` be `bool`, not `Option<bool>`

---

_Pull request opened by @konstin on 2024-05-21 11:34_

Small refactoring of the internal representation. This does not change `tool.uv.sources`.

---

_Label `internal` added by @konstin on 2024-05-21 11:34_

---

_@charliermarsh reviewed on 2024-05-21 11:39_

I thought this was for the TOML representation, where it does make sense for it to be optional (since it only applies to directories).

---

_Comment by @konstin on 2024-05-21 11:56_

This only changes the internal representation, not touching the external input. The conversion happens in the `unwrap_or`. I updated the description to make this clearer

---

_Comment by @charliermarsh on 2024-05-21 11:57_

Thanks.

---

_@charliermarsh approved on 2024-05-21 11:57_

---

_Comment by @charliermarsh on 2024-05-21 11:57_

Appreciate the clarification.

---

_Comment by @charliermarsh on 2024-05-21 13:56_

Kicking this to run again, CI seems stuck in queued...

---

_Merged by @charliermarsh on 2024-05-21 14:34_

---

_Closed by @charliermarsh on 2024-05-21 14:34_

---

_Branch deleted on 2024-05-21 14:34_

---
