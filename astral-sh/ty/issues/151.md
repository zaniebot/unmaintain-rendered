```yaml
number: 151
title: support async/await and Awaitable
type: issue
state: closed
author: carljm
labels:
  - generics
  - runtime semantics
assignees: []
created_at: 2025-03-26T18:13:10Z
updated_at: 2025-07-31T10:59:43Z
url: https://github.com/astral-sh/ty/issues/151
synced_at: 2026-01-10T02:06:24Z
```

# support async/await and Awaitable

---

_Issue opened by @carljm on 2025-03-26 18:13_

- [x] Infer correct return type for `async` functions https://github.com/astral-sh/ruff/pull/19595
- [x] Support for `await` expressions https://github.com/astral-sh/ruff/pull/19595
- [x] Support `async with` https://github.com/astral-sh/ruff/pull/19595
- [x] Support `async for`, async generators, async iterables, async comprehensions https://github.com/astral-sh/ruff/pull/19634


---

_Comment by @AlexWaygood on 2025-03-26 18:58_

(Moved to "backlog" since this requires support for generics)

---

_Renamed from "[red-knot] support async/await and Awaitable" to "support async/await and Awaitable" by @MichaReiser on 2025-05-07 15:25_

---

_Label `generics` added by @AlexWaygood on 2025-05-11 07:32_

---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-11 07:56_

---

_Added to milestone `Beta` by @carljm on 2025-06-11 00:46_

---

_Assigned to @sharkdp by @sharkdp on 2025-07-28 13:09_

---

_Comment by @sharkdp on 2025-07-31 10:59_

I created two new tickets to follow up on some topics around diagnostics / error handling:
* https://github.com/astral-sh/ty/issues/918
* https://github.com/astral-sh/ty/issues/919

Apart from this, all `async` *language* features have been implemented, I think? So I'm closing this ticket for now.

Note that a lot of async APIs make use of rather complicated generic features, and so we still infer `Unknown` in lots of cases. I gave some examples in https://github.com/astral-sh/ty/issues/500#issuecomment-3131706036. Resolving that ticket would certainly help a lot, but is definitely not all that's needed to properly infer correct types in async code.

---

_Closed by @sharkdp on 2025-07-31 10:59_

---
