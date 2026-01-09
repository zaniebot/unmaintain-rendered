---
number: 16188
title: "Add `usedforsecurity=False` flag to `S324` docs"
type: issue
state: closed
author: Skylion007
labels:
  - documentation
assignees: []
created_at: 2025-02-16T15:56:28Z
updated_at: 2025-02-16T18:06:56Z
url: https://github.com/astral-sh/ruff/issues/16188
synced_at: 2026-01-07T13:12:16-06:00
---

# Add `usedforsecurity=False` flag to `S324` docs

---

_Issue opened by @Skylion007 on 2025-02-16 15:56_

### Description

It seems like adding `usedforsecurity=False` flag properly suppresses the rule `S324`. This makes enabling it useful for [FIPS](https://en.wikipedia.org/wiki/Federal_Information_Processing_Standards) compliance of popular python projects. I only discovered this due to trial and error though so it would be useful to add it the documentation for the rule

---

_Referenced in [astral-sh/ruff#16190](../../astral-sh/ruff/pulls/16190.md) on 2025-02-16 16:13_

---

_Referenced in [pytorch/pytorch#147236](../../pytorch/pytorch/issues/147236.md) on 2025-02-16 16:16_

---

_Label `documentation` added by @ntBre on 2025-02-16 17:02_

---

_Closed by @ntBre on 2025-02-16 18:06_

---

_Referenced in [pytorch/pytorch#147627](../../pytorch/pytorch/issues/147627.md) on 2025-02-21 15:07_

---
