```yaml
number: 12475
title: "POC: Implement `uv pip wheel`"
type: pull_request
state: open
author: abhiaagarwal
labels: []
assignees: []
draft: true
base: main
head: pip_wheel
created_at: 2025-03-26T03:22:03Z
updated_at: 2025-10-29T21:30:41Z
url: https://github.com/astral-sh/uv/pull/12475
synced_at: 2026-01-12T16:10:17Z
```

# POC: Implement `uv pip wheel`

---

_@abhiaagarwal_

## Summary

Implements a rudimentary `uv pip wheel` command. This is simply a repackaged `uv pip install` that short-circuits at the actual install step and instead takes the cached wheels and zips them up. Marked as draft because I don't really like my implementation, it's a bit messy and duplicates a lot of `uv pip install`, and there's questions about the semantics of `pip wheel` that I don't have enough knowledge to answer.

Resolves #1681 

## TODO:

1. De-duplicate most of the logic between `uv pip wheel` and `uv pip install`
2. Decide whether a notion of `resolving dependencies` is really how `pip wheel` is supposed to behave
3. Introduce a separate crate for writing a zipped wheel (I basically just copied from `uv-build-backend`)
4. Write tests / integration tests.

(On the last point: I can `uv pip wheel` and install said wheels, and they're identical to `uv pip wheel`, sans the records being slightly different due to console scripts not being included. I do think this is an actual bug though, which is why I want to use the logic in `uv-build-backend` which has already solved this, though it will need to be in a new crate).

---

_Comment by @abhiaagarwal on 2025-03-26 03:30_

`uv pip download` can be supported without much of a lift from this logic as well. `uv pip {download, wheel, install}` are conceptually all ancestors of each other and follow the same logic. 

---

_Comment by @Tengs-Fan on 2025-10-29 21:30_

come from https://github.com/astral-sh/uv/issues/1681, any update ?

---
