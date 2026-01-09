---
number: 10832
title: Update annotate-snippets to 0.11.0
type: issue
state: closed
author: MichaReiser
labels:
  - dependencies
assignees: []
created_at: 2024-04-08T07:05:52Z
updated_at: 2025-01-15T18:37:54Z
url: https://github.com/astral-sh/ruff/issues/10832
synced_at: 2026-01-07T13:12:15-06:00
---

# Update annotate-snippets to 0.11.0

---

_Issue opened by @MichaReiser on 2024-04-08 07:05_

See https://github.com/astral-sh/ruff/pull/10699

CC: @carljm in case you plan to work on this.

We may also decide not to use annotate-snippets anymore (it has some weirdness like it supports some markup that can mess up rendering)

---

_Label `help wanted` added by @MichaReiser on 2024-04-08 07:05_

---

_Label `dependencies` added by @MichaReiser on 2024-04-08 07:05_

---

_Comment by @dhruvmanila on 2024-04-08 12:16_

This will be useful for the new parser test cases as well. If I understand this correctly,  https://github.com/rust-lang/annotate-snippets-rs/pull/107 allows highlighting empty spans which can occur at EOF. You might've noticed that there are no error messages in some of the cases :)

---

_Comment by @MichaReiser on 2024-04-08 12:57_

Oh nice, that annoyed me for a long time. I think we should also change the expected errors to point to an empty range that starts at the next token's position instead of using the entire range of the next token. 

---

_Label `help wanted` removed by @MichaReiser on 2024-07-04 11:35_

---

_Referenced in [astral-sh/ruff#11618](../../astral-sh/ruff/pulls/11618.md) on 2024-07-04 11:35_

---

_Referenced in [astral-sh/ruff#13473](../../astral-sh/ruff/pulls/13473.md) on 2024-09-23 07:35_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-10-08 13:20_

---

_Removed from milestone `v0.8` by @AlexWaygood on 2024-11-18 11:39_

---

_Added to milestone `v0.9` by @AlexWaygood on 2024-11-18 11:39_

---

_Removed from milestone `v0.9` by @MichaReiser on 2025-01-08 13:35_

---

_Added to milestone `v.0.10` by @MichaReiser on 2025-01-08 13:35_

---

_Referenced in [astral-sh/ruff#15359](../../astral-sh/ruff/pulls/15359.md) on 2025-01-08 20:23_

---

_Closed by @BurntSushi on 2025-01-15 18:37_

---
