---
number: 11745
title: uv tool list should include the version specifiers for each tool
type: issue
state: closed
author: chazmo03
labels:
  - question
assignees: []
created_at: 2025-02-24T15:48:42Z
updated_at: 2025-02-24T18:53:17Z
url: https://github.com/astral-sh/uv/issues/11745
synced_at: 2026-01-07T13:12:18-06:00
---

# uv tool list should include the version specifiers for each tool

---

_Issue opened by @chazmo03 on 2025-02-24 15:48_

### Summary

It would be helpful to display the version specifiers used to install each tool when running `uv tool list`. It might also be helpful to see related output during calls to `uv tool upgrade`, in cases where a tool isn't upgrade because newer available versions do not match the version specifier.

### Example

This is an example of the current behavior. It can be confusing that running `uv tool upgrade` may not upgrade the given package and there's no output that indicates why. In this case, v0.9.7 is available but there's no indication why it's not being upgraded.

```
❯ uv tool install ruff@0.9.6
Resolved 1 package in 1.51s
Prepared 1 package in 478ms
Installed 1 package in 10ms
 + ruff==0.9.6
Installed 1 executable: ruff

❯ uv tool list
ruff v0.9.6
- ruff

❯ uv tool upgrade ruff
Nothing to upgrade
```

---

_Label `enhancement` added by @chazmo03 on 2025-02-24 15:48_

---

_Comment by @zanieb on 2025-02-24 16:05_

See 

```
--show-version-specifiers  Whether to display the version specifier(s) used to install each tool
```

---

_Label `enhancement` removed by @zanieb on 2025-02-24 16:05_

---

_Label `question` added by @zanieb on 2025-02-24 16:05_

---

_Closed by @charliermarsh on 2025-02-24 18:53_

---
