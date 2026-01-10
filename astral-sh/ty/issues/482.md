```yaml
number: 482
title: memory regression between 0.0.1a5 and 0.0.1a6?
type: issue
state: closed
author: carljm
labels:
  - memory
assignees: []
created_at: 2025-05-21T21:49:24Z
updated_at: 2025-07-23T22:52:18Z
url: https://github.com/astral-sh/ty/issues/482
synced_at: 2026-01-10T02:06:24Z
```

# memory regression between 0.0.1a5 and 0.0.1a6?

---

_Issue opened by @carljm on 2025-05-21 21:49_

We've had a report of a significant memory regression between these two versions (like from 52GB on a5 to >80GB on a6) on a non-public codebase.

Not obvious in the a6 changelog which PR would be the culprit; may need to see if we can reproduce a memory regression on a smaller codebase and then bisect on that.

---

_Label `memory` added by @carljm on 2025-05-21 21:49_

---

_Comment by @carljm on 2025-05-22 03:11_

This was bisected to https://github.com/astral-sh/ruff/pull/18156

---

_Added to milestone `Beta` by @MichaReiser on 2025-05-28 12:12_

---

_Removed from milestone `Beta` by @carljm on 2025-06-11 01:07_

---

_Comment by @carljm on 2025-07-23 22:52_

We've been working on memory usage and have brought it down quite a bit, though it's still higher than we'd like. I think the conclusion here was that as best we could tell there wasn't a specific bug introduced in this PR, just more understanding of more types leading to higher memory usage. So I don't think keeping this specific issue open is useful anymore.

---

_Closed by @carljm on 2025-07-23 22:52_

---
