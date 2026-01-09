---
number: 15092
title: "Improve failed resolution error messages for \"splits\""
type: issue
state: open
author: zanieb
labels:
  - error messages
assignees: []
created_at: 2025-08-05T19:58:00Z
updated_at: 2025-08-06T13:26:55Z
url: https://github.com/astral-sh/uv/issues/15092
synced_at: 2026-01-07T13:12:19-06:00
---

# Improve failed resolution error messages for "splits"

---

_Issue opened by @zanieb on 2025-08-05 19:58_

See https://github.com/astral-sh/uv/pull/15041#discussion_r2255215788
            

---

_Comment by @zanieb on 2025-08-05 19:58_

We need some loose consensus on what information is _needed_ here and _why_ then how we could display it in a more friendly way.

---

_Comment by @zanieb on 2025-08-05 20:14_

@konstin maybe to start, could you explain why we need to say which packages are included in the split when they're present in the subsequent error message? I think this is also often the case for splits based on markers?

---

_Label `error messages` added by @zanieb on 2025-08-05 20:25_

---

_Referenced in [astral-sh/uv#15041](../../astral-sh/uv/pulls/15041.md) on 2025-08-06 07:54_

---

_Comment by @konstin on 2025-08-06 10:07_

Usually, we show the influence of a marker and of extras in the error trace itself. I don't think we're confident enough that we don't have any of these cases anymore that we could drop this entirely, at least I regularly have to look at the information of the marker, both when debugging user problems and when debugging problem I hit myself.

Something I've also seen come up is "This install with `uv pip install`, why is `uv sync` failing?", which the fork markers usually answer.

With the current rendering, we're using the same string for the full range from error traces with specific context and hints to `debug!` messages about which branch we're in and what branches we resolved. Prior to #15041, we were incorrectly logging that some forks were the universal fork even though they were conflict forks. We could trim this down to only use the full string in verbose mode, and either drop it in default mode, or only show the markers (since the extras are always part of the error trace).

---

_Referenced in [astral-sh/uv#15222](../../astral-sh/uv/issues/15222.md) on 2025-08-12 02:30_

---
