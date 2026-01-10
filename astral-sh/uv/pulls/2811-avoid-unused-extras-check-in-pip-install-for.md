```yaml
number: 2811
title: "Avoid unused extras check in `pip install` for source trees"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/inst
created_at: 2024-04-03T15:17:35Z
updated_at: 2024-04-03T15:23:27Z
url: https://github.com/astral-sh/uv/pull/2811
synced_at: 2026-01-10T14:49:08Z
```

# Avoid unused extras check in `pip install` for source trees

---

_Pull request opened by @charliermarsh on 2024-04-03 15:17_

## Summary

This was an oversight in the `-r pyproject.toml` refactor. We can't enforce unused extras if we have a source tree. We made the correct changes to `pip compile`, but not `pip install`. This PR just mirrors those changes to `pip install`, and adds a few tests.

Closes https://github.com/astral-sh/uv/issues/2801.


---

_Label `bug` added by @charliermarsh on 2024-04-03 15:17_

---

_Marked ready for review by @charliermarsh on 2024-04-03 15:17_

---

_Merged by @charliermarsh on 2024-04-03 15:23_

---

_Closed by @charliermarsh on 2024-04-03 15:23_

---

_Branch deleted on 2024-04-03 15:23_

---
