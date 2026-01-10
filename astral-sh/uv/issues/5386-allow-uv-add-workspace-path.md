```yaml
number: 5386
title: "Allow `uv add --workspace <path>`"
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2024-07-23T22:23:14Z
updated_at: 2024-08-20T18:21:31Z
url: https://github.com/astral-sh/uv/issues/5386
synced_at: 2026-01-10T04:53:49Z
```

# Allow `uv add --workspace <path>`

---

_Issue opened by @zanieb on 2024-07-23 22:23_

e.g. `uv add [<package>] --workspace <path>` to add a workspace member.

We may want to consider an alternative option name, e.g. `--workspace-member`? Is this just a case of #5385 in a workspace?

Part of https://github.com/astral-sh/uv/issues/5381

---

_Label `cli` added by @zanieb on 2024-07-23 22:24_

---

_Label `preview` added by @zanieb on 2024-07-23 22:24_

---

_Comment by @chrisrodrigue on 2024-08-07 21:47_

Could a workspace be a remote git repo or does that path need to be a local directory/submodule?

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---
