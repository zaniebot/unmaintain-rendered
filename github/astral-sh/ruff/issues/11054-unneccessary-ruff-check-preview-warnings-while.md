---
number: 11054
title: unneccessary ruff check preview warnings while running ruff format without preview
type: issue
state: open
author: 57an
labels:
  - cli
  - preview
assignees: []
created_at: 2024-04-20T09:21:19Z
updated_at: 2025-06-02T17:21:55Z
url: https://github.com/astral-sh/ruff/issues/11054
synced_at: 2026-01-07T13:12:15-06:00
---

# unneccessary ruff check preview warnings while running ruff format without preview

---

_Issue opened by @57an on 2024-04-20 09:21_

When I run ruff format I get such warning:

```
warning: Selection `FURB` has no effect because preview is not enabled.
```

FURB warning should not occur because it is just ruff check rule.


---

_Renamed from "Not-neccessary warnings while running ruff format " to "unneccessary ruff check preview warnings while running ruff format without preview" by @57an on 2024-04-21 03:57_

---

_Label `cli` added by @dhruvmanila on 2024-04-22 09:42_

---

_Label `preview` added by @dhruvmanila on 2024-04-22 09:42_

---

_Comment by @some-unprofessional-account on 2025-06-02 16:57_

I get the same for DOC rule when I try to use `ruff format`:
``warning: Selection `DOC` has no effect because preview is not enabled.``

I do not know how this happens as `DOC` is a linting rule and has nothing to do with formatting.


---

_Comment by @ntBre on 2025-06-02 17:21_

It looks like this warning is emitted while loading the config file, where (I think) we don't know which subcommand was invoked:

https://github.com/astral-sh/ruff/blob/47698883ae06f8d647fbe234aeedcf2333240218/crates/ruff_workspace/src/configuration.rs#L1069-L1074

I agree that it would be nice to fix this, but it might be a fairly substantial refactor.

---
