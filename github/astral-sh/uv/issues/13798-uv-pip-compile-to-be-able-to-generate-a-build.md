---
number: 13798
title: "`uv pip compile` to be able to generate a build constraints file"
type: issue
state: closed
author: paveldikov
labels:
  - enhancement
assignees: []
created_at: 2025-06-03T08:41:32Z
updated_at: 2025-06-03T08:43:12Z
url: https://github.com/astral-sh/uv/issues/13798
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv pip compile` to be able to generate a build constraints file

---

_Issue opened by @paveldikov on 2025-06-03 08:41_

### Summary

The idea of build constraints is fantastic.. but how does one generate such a constraints file in the first place?

If only `uv pip compile` had flags such as `--include-build-system-deps` and `--only-build-system-deps` -- things would be so much easier.

A possible complication, of course, is that project dependencies will in turn have their own build requirements, and those may well conflict with each other thanks to PEP 517 isolation. This is probably something that has to be resolved more systematcally within the `pylock.toml` spec (currently it does not track build system dependencies at all)

### Example

_No response_

---

_Label `enhancement` added by @paveldikov on 2025-06-03 08:41_

---

_Comment by @paveldikov on 2025-06-03 08:43_

Never mind -- seems my first pass at searching did not find #5190. Seems that I had commented on that, too...

---

_Closed by @paveldikov on 2025-06-03 08:43_

---
