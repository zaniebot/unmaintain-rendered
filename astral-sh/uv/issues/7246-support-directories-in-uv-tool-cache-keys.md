```yaml
number: 7246
title: "Support directories in `uv.tool.cache-keys`"
type: issue
state: closed
author: vivienm
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2024-09-10T08:16:00Z
updated_at: 2024-09-11T19:31:01Z
url: https://github.com/astral-sh/uv/issues/7246
synced_at: 2026-01-10T04:45:10Z
```

# Support directories in `uv.tool.cache-keys`

---

_Issue opened by @vivienm on 2024-09-10 08:16_

I work on projects using both Python and Rust. I'd like to make uv rebuild the project when the Rust code is modified, typically when any `*.rs` file in the `src` directory is modified.

The new configuration entry `uv.tool.cache-keys` is promising, but doesn't seem to work with directories.

Possibly related: https://github.com/astral-sh/uv/issues/2152.

---

_Comment by @charliermarsh on 2024-09-10 13:26_

I've been hesitant to add this because in some cases it may just be slower than building the project. Perhaps not for an extension module like that, though.

---

_Label `enhancement` added by @charliermarsh on 2024-09-10 13:47_

---

_Label `needs-decision` added by @charliermarsh on 2024-09-10 13:47_

---

_Comment by @vivienm on 2024-09-10 14:08_

Also, it looks like entries such as `{ file = "some-directory" }` are silently ignored. Maybe it could be nice to have a warning?

---

_Comment by @sbidoul on 2024-09-10 15:00_

Related: the ask for glob patterns in cache-keys, discussed in https://github.com/astral-sh/uv/pull/7136


---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-10 18:17_

---

_Closed by @charliermarsh on 2024-09-11 19:31_

---
