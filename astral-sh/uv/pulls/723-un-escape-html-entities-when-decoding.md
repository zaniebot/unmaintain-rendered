```yaml
number: 723
title: Un-escape HTML entities when decoding
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/decode
created_at: 2023-12-24T21:24:02Z
updated_at: 2023-12-24T21:45:15Z
url: https://github.com/astral-sh/uv/pull/723
synced_at: 2026-01-10T15:44:44Z
```

# Un-escape HTML entities when decoding

---

_Pull request opened by @charliermarsh on 2023-12-24 21:24_

I don't have a good testing strategy here (I'm manually testing against `devpi` via `packse`), but the HTML index uses (e.g.) `data-requires-python="&gt;=3.8"`, so we need to decode.

---

_Label `bug` added by @charliermarsh on 2023-12-24 21:24_

---

_Merged by @charliermarsh on 2023-12-24 21:35_

---

_Closed by @charliermarsh on 2023-12-24 21:35_

---

_Branch deleted on 2023-12-24 21:35_

---

_Comment by @zanieb on 2023-12-24 21:36_

It's possible this is a bug in devpi but probably best to support it.

---

_Comment by @charliermarsh on 2023-12-24 21:45_

👍 I think you generally _do_ need to escape at least `>` and `<` so it seems reasonable to me.

---
