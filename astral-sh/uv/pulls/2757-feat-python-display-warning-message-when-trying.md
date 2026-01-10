```yaml
number: 2757
title: "feat(python): display warning message when trying to use an unsupported Python version"
type: pull_request
state: closed
author: stegayet
labels: []
assignees: []
base: main
head: feat/add-warning-message-unsupported-python-version
created_at: 2024-04-01T13:54:38Z
updated_at: 2024-04-24T17:51:58Z
url: https://github.com/astral-sh/uv/pull/2757
synced_at: 2026-01-10T14:37:54Z
```

# feat(python): display warning message when trying to use an unsupported Python version

---

_Pull request opened by @stegayet on 2024-04-01 13:54_

## Summary

Closes https://github.com/astral-sh/uv/issues/2587.

## Test Plan

<!-- How was it tested? -->

Locally tested.

---

_@zanieb reviewed on 2024-04-01 14:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/venv.rs`:122 on 2024-04-01 14:40_

We might want to say like:

"uv is only compatible with Python 3.8+, found Python {version}."

---

_Review requested from @zanieb by @stegayet on 2024-04-04 15:28_

---

_Marked ready for review by @stegayet on 2024-04-04 15:28_

---

_Assigned to @zanieb by @zanieb on 2024-04-04 15:46_

---

_Comment by @zanieb on 2024-04-24 16:27_

Thanks for the contributing! Sorry I took so long for a second review, I went to update this with `main` and realized we should probably move the warning into the `uv-interpreter` crate. See https://github.com/astral-sh/uv/pull/3250

I've included you as a co-author still :)

---

_Closed by @zanieb on 2024-04-24 17:51_

---
