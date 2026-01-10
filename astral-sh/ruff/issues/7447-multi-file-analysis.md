```yaml
number: 7447
title: Multi-file analysis
type: issue
state: open
author: DetachHead
labels:
  - core
  - type-inference
assignees: []
created_at: 2023-09-17T00:05:36Z
updated_at: 2025-08-15T06:11:01Z
url: https://github.com/astral-sh/ruff/issues/7447
synced_at: 2026-01-10T11:09:49Z
```

# Multi-file analysis

---

_Issue opened by @DetachHead on 2023-09-17 00:05_

currently many rules are limited by the fact that ruff can't do multifile analysis (eg. #6932)

raising a central tracking issue for this as suggested in https://github.com/astral-sh/ruff/issues/6932#issuecomment-1696641747

---

_Renamed from "multifile analysis" to "Multi-file analysis" by @charliermarsh on 2023-09-19 03:19_

---

_Label `core` added by @charliermarsh on 2023-09-19 03:19_

---

_Label `multifile-analysis` added by @MichaReiser on 2024-03-21 14:46_

---

_Comment by @danieleades on 2024-03-29 15:03_

is there any defined roadmap or plan for this? I can imagine it's a complicated problem to solve, especially without significant performance regressions. Would it be opt in?

---

_Comment by @zanieb on 2024-03-29 15:15_

We are designing this right now. The roadmap is still being determined. As always, performance will be very important to us :)

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---

_Comment by @robtaylor on 2025-03-19 10:52_

Is there a branch I could track for this?

---

_Comment by @MichaReiser on 2025-03-19 11:48_

You can follow our work on red-knot (see the `red_knot` crate and issues/PRs labeled with `red-knot`)

---

_Comment by @jasha-hrp on 2025-05-09 18:48_

Hi @MichaReiser, has red-knot been renamed?
- I can't find [red_knot](https://crates.io/search?q=red_knot) or [red-knot](https://crates.io/search?q=red-knot) on crates.io
- I can't find [red_knot](https://github.com/astral-sh/ruff/issues?q=is%3Aissue%20state%3Aopen%20label%3Ared_knot) or [red-knot](https://github.com/astral-sh/ruff/issues?q=is%3Aissue%20state%3Aopen%20label%3Ared-knot) labels among the ruff issues.

---

_Comment by @jasha-hrp on 2025-05-09 19:04_

Aah, is this what the `ty` repo is for? 
https://github.com/astral-sh/ty

---

_Comment by @insane-dreamer on 2025-08-15 04:45_

Is there any hope we'll see this supported? Makes a big difference for codebases where there's a lot of inheritance, star imports, etc. 

---

_Comment by @MichaReiser on 2025-08-15 06:11_

This is happening right now but we decided to build it outside ruff first because it's a pretty big architectural shift. See [astral-sh/ty](https://github.com/astral-sh/ty?rgh-link-date=2025-05-09T19%3A04%3A54.000Z)

---
