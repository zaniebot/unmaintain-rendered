```yaml
number: 6138
title: "Add support for `package@latest` in `tool run`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: charlie/latest-tool
created_at: 2024-08-16T03:06:27Z
updated_at: 2024-08-19T16:58:38Z
url: https://github.com/astral-sh/uv/pull/6138
synced_at: 2026-01-12T16:07:14Z
```

# Add support for `package@latest` in `tool run`

---

_@charliermarsh_

## Summary

`@latest` will ignore any installed tools and force a cache refresh.

Closes https://github.com/astral-sh/uv/issues/5807.


---

_Label `cli` added by @charliermarsh on 2024-08-16 03:06_

---

_Label `preview` added by @charliermarsh on 2024-08-16 03:06_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-16 03:10_

---

_Comment by @charliermarsh on 2024-08-16 12:30_

(This needs docs.)

---

_@zanieb reviewed on 2024-08-16 20:23_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/run.rs`:308 on 2024-08-16 20:23_

I didn't know what this meant without reading the parser. Can you make this a bit clearer?

---

_@zanieb approved on 2024-08-16 20:24_

---

_Merged by @charliermarsh on 2024-08-19 16:58_

---

_Closed by @charliermarsh on 2024-08-19 16:58_

---

_Branch deleted on 2024-08-19 16:58_

---
