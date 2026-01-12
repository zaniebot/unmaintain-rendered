```yaml
number: 12616
title: "Add `excluded-dependencies` support"
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - configuration
assignees: []
created_at: 2025-04-02T01:38:42Z
updated_at: 2025-10-31T14:14:13Z
url: https://github.com/astral-sh/uv/issues/12616
synced_at: 2026-01-12T16:01:08Z
```

# Add `excluded-dependencies` support

---

_@zanieb_

We support this now by setting an impossible marker, e.g., `override-dependencies = ["package; sys_platform = "never"]` (https://docs.astral.sh/uv/reference/settings/#override-dependencies). We should add a dedicated option, e.g., `excluded-dependencies = ["package"]`. Unlike `--no-install-package`, this omits the whole dependency tree. We should add a dedicated option, as this is fairly bespoke.

Related:

- #12523 
- #4422 

---

_Label `configuration` added by @zanieb on 2025-04-02 01:38_

---

_Comment by @MrPandir on 2025-05-15 17:32_

This is literally what I requested in issue #7214, so I'll just mention it so it can be referenced. 

---

_Label `help wanted` added by @zanieb on 2025-05-15 17:48_

---

_Comment by @zanieb on 2025-05-15 17:48_

This should be fairly easy to add, if someone is interested.

---

_Comment by @CodeMan62 on 2025-05-20 18:59_

let me investigate and propose a fix 
EDIT:- Working 

---

_Comment by @CodeMan62 on 2025-05-23 12:00_

@zanieb  i have a small question can you tell me is this PR #10207 is related to this can I take reference of that and implement it or what I need just small clarification and then I will be good to go 
EDIT:- sorry I found what to do

---

_Closed by @charliermarsh on 2025-10-31 14:14_

---
