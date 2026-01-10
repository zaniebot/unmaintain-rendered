```yaml
number: 2691
title: "Unhide `--emit-index-url` and `--emit-find-links`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/unhide
created_at: 2024-03-27T14:48:51Z
updated_at: 2024-03-29T13:41:28Z
url: https://github.com/astral-sh/uv/pull/2691
synced_at: 2026-01-10T14:49:08Z
```

# Unhide `--emit-index-url` and `--emit-find-links`

---

_Pull request opened by @charliermarsh on 2024-03-27 14:48_

## Summary

I don't know why I hid these in the first place. Maybe I copied these over from the `compat.rs` version.

Closes https://github.com/astral-sh/uv/issues/1502.


---

_Label `cli` added by @charliermarsh on 2024-03-27 14:48_

---

_Marked ready for review by @charliermarsh on 2024-03-27 14:48_

---

_@zanieb approved on 2024-03-27 14:49_

---

_Merged by @charliermarsh on 2024-03-27 14:55_

---

_Closed by @charliermarsh on 2024-03-27 14:55_

---

_Branch deleted on 2024-03-27 14:55_

---

_Comment by @hofrob on 2024-03-29 13:39_

I think the wrong params were changed here. The "extra-index-url" parameter should be shown in the requirements.txt. And maybe the "index-url"?

From what I can gather, this MR changes the visibility for the "emit-index-url". (which can stay hidden I think)

---

_Comment by @charliermarsh on 2024-03-29 13:41_

We intentionally did not change the default. We just made the opt-in setting visible on the CLI.

---
