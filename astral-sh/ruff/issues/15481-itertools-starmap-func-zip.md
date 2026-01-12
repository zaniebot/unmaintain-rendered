```yaml
number: 15481
title: "`itertools.starmap(func, zip(...))`"
type: issue
state: closed
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
created_at: 2025-01-14T21:54:21Z
updated_at: 2025-01-16T14:18:13Z
url: https://github.com/astral-sh/ruff/issues/15481
synced_at: 2026-01-12T15:54:54Z
```

# `itertools.starmap(func, zip(...))`

---

_@InSyncWithFoo_

#14835 asked that [`FURB140`](https://docs.astral.sh/ruff/rules/reimplemented-starmap/) be changed to accommodate the pattern `itertools.starmap(func, zip(...))`, suggesting `map(func, ...)` as the replacement.

[This comment](https://github.com/astral-sh/ruff/issues/14835#issuecomment-2544883959) lists four steps of improvements, of which the second doesn't seem to be actionable/applicable (`FURB140` does not check for `map`). This issue discusses the third:

> A new rule that recommends using `map` over `starmap(func, zip)`

`starmap(func, zip(a, b, c))` can be transformed losslessly to `map(func, a, b, c)`, so violations are both easy to detect and safely fixable, even with comments.


---

_Comment by @InSyncWithFoo on 2025-01-14 21:58_

Alternatively, the fix of `FURB140` could be modified to create a special case when `zip()` is used as the `iter` of a comprehension. I think this is much less preferable as it has [the same pitfalls](https://github.com/astral-sh/ruff/issues/14793) as the original `RUF046`.

---

_Label `rule` added by @dylwil3 on 2025-01-15 00:35_

---

_Label `preview` added by @dylwil3 on 2025-01-15 00:35_

---

_Comment by @MichaReiser on 2025-01-15 09:07_

Copied over from the PR: 

<hr>

I don't think we can implement this right now without changing `FURB140`. You can see in this [playground](https://play.ruff.rs/a9af7dfb-6dc5-4eee-b8c9-f41437eb24bf) that `FURB140` recommends using `starmap` for `map(..., zip)`. I think this is what I meant that we have to change `FURB140` first but I incorrectly wrote `map` instead of saying that it shouldn't apply to `zip`. 

---

_Comment by @MichaReiser on 2025-01-15 09:14_

We should also decide if we want to add this rule (leaning towards yes). 

---

_Comment by @InSyncWithFoo on 2025-01-15 10:49_

I think the situation is similar to that of [`UP031`](https://docs.astral.sh/ruff/rules/printf-string-formatting/) and [`UP032`](https://docs.astral.sh/ruff/rules/f-string/) in that one reports (and fixes) the other's output. Given that this rule is almost always safely fixable, it should be just as painless.

Changing `FURB140` is trivial enough but that would be a perhaps undesired special case.


---

_Comment by @MichaReiser on 2025-01-15 13:54_

I guess that's fair and we can change FURB140 later if necessary. Thanks for explaining.

---

_Closed by @MichaReiser on 2025-01-16 14:18_

---
