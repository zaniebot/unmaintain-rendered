```yaml
number: 1564
title: "Allow non-nested archives for `hexdump` and others"
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/hexdump
created_at: 2024-02-17T02:48:23Z
updated_at: 2024-02-17T04:17:36Z
url: https://github.com/astral-sh/uv/pull/1564
synced_at: 2026-01-10T15:33:24Z
```

# Allow non-nested archives for `hexdump` and others

---

_Pull request opened by @charliermarsh on 2024-02-17 02:48_

## Summary#1562 

It turns out that `hexdump` uses an invalid source distribution format whereby the contents aren't nested in a top-level directory -- instead, they're all just flattened at the top-level. In looking at pip's source (https://github.com/pypa/pip/blob/51de88ca6459fdd5213f86a54b021a80884572f9/src/pip/_internal/utils/unpacking.py#L62), it only strips the top-level directory if all entries have the same directory prefix (i.e., if it's the only thing in the directory). This PR accommodates these "invalid" distributions.

I can't find any history on this method in `pip`. It looks like it dates back over 15 years ago, to before `pip` was even called `pip`.

Closes https://github.com/astral-sh/uv/issues/1376.


---

_Label `compatibility` added by @charliermarsh on 2024-02-17 02:48_

---

_Review requested from @zanieb by @charliermarsh on 2024-02-17 02:54_

---

_@zanieb approved on 2024-02-17 04:08_

---

_Merged by @charliermarsh on 2024-02-17 04:17_

---

_Closed by @charliermarsh on 2024-02-17 04:17_

---

_Branch deleted on 2024-02-17 04:17_

---
