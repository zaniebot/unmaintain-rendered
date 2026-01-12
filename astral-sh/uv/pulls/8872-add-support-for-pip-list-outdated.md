```yaml
number: 8872
title: "Add support for `pip list --outdated`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/pip-list-outdated
created_at: 2024-11-07T02:10:13Z
updated_at: 2025-01-23T16:01:27Z
url: https://github.com/astral-sh/uv/pull/8872
synced_at: 2026-01-12T16:08:32Z
```

# Add support for `pip list --outdated`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/2150.


---

_Label `enhancement` added by @charliermarsh on 2024-11-07 02:10_

---

_Label `compatibility` added by @charliermarsh on 2024-11-07 02:10_

---

_Merged by @charliermarsh on 2024-11-07 02:32_

---

_Closed by @charliermarsh on 2024-11-07 02:32_

---

_Branch deleted on 2024-11-07 02:32_

---

_Comment by @ulgens on 2025-01-23 08:14_

This PR adds support for `--outdated` but not `-o`. Was that on purpose?

---

_Comment by @charliermarsh on 2025-01-23 16:01_

Mildly against adding `-o` given that it has a different meaning on other CLI commands.

---
