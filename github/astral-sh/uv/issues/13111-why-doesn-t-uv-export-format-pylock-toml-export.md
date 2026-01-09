---
number: 13111
title: "Why doesn't `uv export --format pylock.toml` export `uv.lock` into a multi-use `pylock.toml`?"
type: issue
state: closed
author: ReinforcedKnowledge
labels:
  - question
assignees: []
created_at: 2025-04-26T01:35:20Z
updated_at: 2025-04-26T03:12:17Z
url: https://github.com/astral-sh/uv/issues/13111
synced_at: 2026-01-07T13:12:18-06:00
---

# Why doesn't `uv export --format pylock.toml` export `uv.lock` into a multi-use `pylock.toml`?

---

_Issue opened by @ReinforcedKnowledge on 2025-04-26 01:35_

### Question

Hi everyone!

I was comparing `uv.lock` to `pylock.toml`, and a question came to my mind, why doesn't `uv export --format pylock.toml` export the `uv.lock`file into a multi-use `pylock.toml`? 

Currently, if we have any extras or dependency groups etc. and we want to export them, we have to use the appropriate options, something like `uv export --format pylock.toml --group dep-group-nape >  pylock.dep-group-name.toml`. Which would give us a single use PEP-751 compatible lock file.

Is it because if you allow for exports to multi-use lock file, users would expect to be able to do `uv pip sync` + give dependency groups or extras and that's not compatible with the `uv`'s `pip` interface? 

Thank you in advance!

### Platform

Ubuntu 24.04.2 LTS x86_64

### Version

uv 0.6.17

---

_Label `question` added by @ReinforcedKnowledge on 2025-04-26 01:35_

---

_Comment by @charliermarsh on 2025-04-26 02:57_

We could eventually support extras and groups there in some cases. It’s not implemented.

---

_Comment by @ReinforcedKnowledge on 2025-04-26 03:12_

Oh ok, it's just not implemented and it's not because there is a fundamental reason or limitation in PEP 751 that doesn't allow for an export of `uv.lock` to a multi-use `pylock.toml` for the generic simple use cases. (Like it's understandable to not be able to export workspaces).

Thank you for your answer. I'll close the issue in that case :)

---

_Closed by @ReinforcedKnowledge on 2025-04-26 03:12_

---
