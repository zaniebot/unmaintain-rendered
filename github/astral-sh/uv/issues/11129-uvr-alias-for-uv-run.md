---
number: 11129
title: "`uvr` alias for `uv run`"
type: issue
state: closed
author: 2-5
labels:
  - enhancement
assignees: []
created_at: 2025-01-31T09:34:25Z
updated_at: 2025-01-31T20:15:11Z
url: https://github.com/astral-sh/uv/issues/11129
synced_at: 2026-01-07T13:12:18-06:00
---

# `uvr` alias for `uv run`

---

_Issue opened by @2-5 on 2025-01-31 09:34_

### Summary

I use a `uvr` alias for `uv run` and find it very convenient when you run a lot of scripts, it's shorter, but more importantly 1 word instead of 2.

I think such an alias should be included with `uv`.

Alternatively, make `uvx` behave like `uv run` when ran inside a project. Not sure how much confusion or other problems this would create.


### Example

_No response_

---

_Label `enhancement` added by @2-5 on 2025-01-31 09:34_

---

_Comment by @zanieb on 2025-01-31 20:15_

See https://github.com/astral-sh/uv/issues/7186 for the latter point.

We won't be including an additional alias for `uv run` though. I think multiple short aliases would be confusing.

I'd be more willing to consider something like `uv r` in the style of `cargo` short commands.

---

_Closed by @zanieb on 2025-01-31 20:15_

---
