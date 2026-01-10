---
number: 19259
title: The ambiguous symbols lint feels overzealous and maybe should be split in two
type: issue
state: closed
author: itamarst
labels: []
assignees: []
created_at: 2025-07-10T12:59:20Z
updated_at: 2025-07-10T13:41:05Z
url: https://github.com/astral-sh/ruff/issues/19259
synced_at: 2026-01-10T01:23:00Z
---

# The ambiguous symbols lint feels overzealous and maybe should be split in two

---

_Issue opened by @itamarst on 2025-07-10 12:59_

### Summary

Just got this:

```
RUF002 Docstring contains ambiguous `×` (MULTIPLICATION SIGN). Did you mean `x` (LATIN SMALL LETTER X)?
```

But I _deliberately_ used the _correct_ unicode sign for multiplication, that's what it's for 😁 I don't see how anyone would accidentally use this instead of `x`, you need to do a rather more work to type `×`, e.g. on Linux it's three keystrokes (`COMPOSE x x`).

I can sort've see the security concern, but if you're going to include symbols that people might deliberately use, perhaps split it into two lints, one for "no one would ever do this legitimately" and one for "symbols that actually look somewhat different and might be legitimately used".

### Version

0.9.4

---

_Renamed from "A lint that complains about using multiplication signs is silly" to "The ambiguous symbols lint feels overzealous and maybe should be split in two" by @itamarst on 2025-07-10 13:17_

---

_Closed by @MichaReiser on 2025-07-10 13:41_

---
