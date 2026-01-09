---
number: 373
title: Manual PEP 440 version parser
type: issue
state: closed
author: konstin
labels:
  - performance
assignees: []
created_at: 2023-11-09T13:02:13Z
updated_at: 2024-01-05T16:57:34Z
url: https://github.com/astral-sh/uv/issues/373
synced_at: 2026-01-07T13:12:16-06:00
---

# Manual PEP 440 version parser

---

_Issue opened by @konstin on 2023-11-09 13:02_

Benchmarking the top 1k pypi packages serially, we spend 8% our in parsing version numbers with regex captures, solving the top 8k in parallel it's 14%. We should instead move to a hand-written PEP 440 parser, which would also much improve the error messages.

https://github.com/astral-sh/puffin/blob/6144de0a7ee67ca9ebb1348543ae643de7492e3b/crates/pep440-rs/src/version.rs#L692-L701

---

_Comment by @charliermarsh on 2023-11-09 14:22_

This could be a fun one for @BurntSushi.

---

_Label `performance` added by @charliermarsh on 2023-11-09 14:22_

---

_Added to milestone `Future` by @charliermarsh on 2023-11-09 14:22_

---

_Comment by @konstin on 2023-11-09 14:24_

As reference, this is the regex from the spec: https://packaging.python.org/en/latest/specifications/version-specifiers/#appendix-parsing-version-strings-with-regular-expressions

---

_Comment by @BurntSushi on 2023-11-10 17:59_

I agree that getting rid of the regex captures and hand-rolling a version parser is probably the right tact to take here for the interim. But I wanted to pop up a level too. So I wrote this issue: https://github.com/astral-sh/puffin/issues/396

---

_Referenced in [astral-sh/uv#399](../../astral-sh/uv/pulls/399.md) on 2023-11-10 19:36_

---

_Referenced in [astral-sh/uv#789](../../astral-sh/uv/pulls/789.md) on 2024-01-04 19:55_

---

_Assigned to @BurntSushi by @charliermarsh on 2024-01-05 04:33_

---

_Closed by @BurntSushi on 2024-01-05 16:57_

---

_Referenced in [astral-sh/uv#1138](../../astral-sh/uv/pulls/1138.md) on 2024-01-26 22:01_

---
