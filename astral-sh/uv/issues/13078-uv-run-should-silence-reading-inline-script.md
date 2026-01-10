---
number: 13078
title: "`uv run` should silence \"Reading inline script metadata from...\" messages"
type: issue
state: closed
author: gregglind
labels:
  - question
assignees: []
created_at: 2025-04-23T23:03:55Z
updated_at: 2025-05-10T20:43:40Z
url: https://github.com/astral-sh/uv/issues/13078
synced_at: 2026-01-10T01:25:28Z
---

# `uv run` should silence "Reading inline script metadata from..." messages

---

_Issue opened by @gregglind on 2025-04-23 23:03_

### Summary

When running a shebanged `uv run --script`:  ./invoke.py , I get console output, that I want to silence.

"Reading inline script metadata from `./invoke.py`"


**What I expect:**

- no extra console output for things that are rust debug level.
- this console input as part of uv verbose or log level 

**What I tried:**

* Changing RUST_LOG:  ```RUST_LOG=uv=error``` and other variants.

**Source Code**

https://github.com/astral-sh/uv/blob/a4ea814159aacb3e95a6a99925780d9f9aa07c3a/crates/uv/src/commands/project/run.rs#L193-L199

### Example

_No response_

---

_Label `enhancement` added by @gregglind on 2025-04-23 23:03_

---

_Renamed from "`uv run` silence "Reading inline script metadata from..." messages" to "`uv run` should silence "Reading inline script metadata from..." messages" by @gregglind on 2025-04-23 23:04_

---

_Comment by @konstin on 2025-04-24 08:55_

Are you setting any environment variables or are you using `-v`, and which uv version are you using? `debug!` messages are not shown by default, and I can't reproduce this locally.

---

_Label `enhancement` removed by @konstin on 2025-04-24 08:55_

---

_Label `question` added by @konstin on 2025-04-24 08:55_

---

_Comment by @gregg-maiven on 2025-04-24 14:16_

@konstin, I will dig a bit and get back to you, once I discover an answer.

I feel **validated** that this is unexpected and strange.  

No further action needed on your end.

---

_Comment by @charliermarsh on 2025-05-10 20:43_

Closing for now but happy to re-open if you find anything odd here.

---

_Closed by @charliermarsh on 2025-05-10 20:43_

---
