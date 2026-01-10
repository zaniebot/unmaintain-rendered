---
number: 12572
title: No feedback when an error occurs in the server in certain cases
type: issue
state: open
author: dhruvmanila
labels:
  - server
assignees: []
created_at: 2024-07-30T02:27:26Z
updated_at: 2025-01-15T07:50:09Z
url: https://github.com/astral-sh/ruff/issues/12572
synced_at: 2026-01-10T01:22:52Z
---

# No feedback when an error occurs in the server in certain cases

---

_Issue opened by @dhruvmanila on 2024-07-30 02:27_

With the way the tracing system is set in the server, there's no feedback provided to the user if an error occurs in certain cases. This is the case because for every `tracing::error` expression, we also require to send a request to display the message like:

https://github.com/astral-sh/ruff/blob/7571da877811100d88fd0a33c933835d05c7c2e4/crates/ruff_server/src/server/api/requests/execute_command.rs#L129-L130

But, it's missing in a few cases. For example, if the path specified in `ruff.configuration` is invalid, the user won't know that:

https://github.com/astral-sh/ruff/blob/7571da877811100d88fd0a33c933835d05c7c2e4/crates/ruff_server/src/session/index/ruff_settings.rs#L288-L291

Related to https://github.com/astral-sh/ruff/issues/12523

---

_Label `server` added by @dhruvmanila on 2024-07-30 02:27_

---

_Comment by @dhruvmanila on 2024-08-09 13:41_

I looked at this and I think the reason this error message isn't displayed is because this code gets executed many number of times while building the settings index. We could either introduce a macro like `show_err_msg_once` but the logs would still contain multiple entries.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-01-15 07:50_

---

_Referenced in [astral-sh/ruff#15494](../../astral-sh/ruff/pulls/15494.md) on 2025-01-15 12:42_

---
