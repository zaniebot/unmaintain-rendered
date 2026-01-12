```yaml
number: 6514
title: "Add `--no-project` alias for `uv python pin --no-workspace`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/pin-alias-no-project
created_at: 2024-08-23T13:56:24Z
updated_at: 2024-08-23T16:08:29Z
url: https://github.com/astral-sh/uv/pull/6514
synced_at: 2026-01-12T16:07:24Z
```

# Add `--no-project` alias for `uv python pin --no-workspace`

---

_@zanieb_

This matches the other interfaces and seems like an oversight.

---

_Label `cli` added by @zanieb on 2024-08-23 13:56_

---

_@charliermarsh approved on 2024-08-23 15:53_

I think this was intentional but I'm fine with the change.

---

_Comment by @zanieb on 2024-08-23 15:54_

@charliermarsh why intentional? We support `--no-workspace` as a `--no-project` alias elsewhere?

---

_Comment by @zanieb on 2024-08-23 15:55_

e.g. for `uv run`
https://github.com/astral-sh/uv/blob/d8c41481ec884cc5166bcfadd95674ff7b2e918d/crates/uv-cli/src/lib.rs#L2211-L2212

---

_Merged by @zanieb on 2024-08-23 16:08_

---

_Closed by @zanieb on 2024-08-23 16:08_

---

_Branch deleted on 2024-08-23 16:08_

---
