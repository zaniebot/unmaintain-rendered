```yaml
number: 19775
title: use Self where possible, added clippy config for it
type: pull_request
state: closed
author: adamnemecek
labels: []
assignees: []
base: main
head: main
created_at: 2025-08-06T00:26:27Z
updated_at: 2025-08-06T15:38:39Z
url: https://github.com/astral-sh/ruff/pull/19775
synced_at: 2026-01-10T17:52:17Z
```

# use Self where possible, added clippy config for it

---

_Pull request opened by @adamnemecek on 2025-08-06 00:26_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Use Self where possible, added clippy config for it.

## Test Plan

Not needed.

---

_Review requested from @carljm by @adamnemecek on 2025-08-06 00:26_

---

_Review requested from @AlexWaygood by @adamnemecek on 2025-08-06 00:26_

---

_Review requested from @sharkdp by @adamnemecek on 2025-08-06 00:26_

---

_Review requested from @dcreager by @adamnemecek on 2025-08-06 00:26_

---

_Review requested from @MichaReiser by @adamnemecek on 2025-08-06 00:26_

---

_Review requested from @BurntSushi by @adamnemecek on 2025-08-06 00:26_

---

_Review requested from @dhruvmanila by @adamnemecek on 2025-08-06 00:26_

---

_Comment by @MichaReiser on 2025-08-06 06:05_

Thank you.

While I tend to prefer using `Self` too, it's also something that IDEs struggle with (e.g. fill in match arms doesn't use `Self`) and I suspect that coding agents will struggle with this too. 

That's why I think this creates more work than it's worth it.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-06 07:15_

---

_Comment by @BurntSushi on 2025-08-06 11:15_

I don't feel strongly about this, but I've generally resisted using `Self` because I find that while it makes _writing_ code a little easier, it makes code just a little bit harder to read than it needs to be. I like to optimize for readers. :-)

---

_Comment by @charliermarsh on 2025-08-06 11:36_

(We merged this change in uv, it's up to you all though, not a big deal either way.)

---

_Comment by @adamnemecek on 2025-08-06 13:54_

@BurntSushi how do explicit types make reading easier? 

---

_Comment by @dcreager on 2025-08-06 14:22_

I weakly prefer using the type name instead of `Self`, but am more bothered by our _inconsistency_. So I would be in favor of a clippy lint that enforces a consistent style, even if it's not the style I'd prefer. I don't suppose there's a clippy lint that _disallows_ `Self`, is there? :grin: 

Though looking at the diff, it doesn't look like this affects enum variants (e.g. mandating one of `Type::TypeVar` vs `Self::TypeVar` inside of `Type`), which is where most of our inconsistency currently is. So it doesn't seem like this lint actually enforces full consistency.

---

_Comment by @BurntSushi on 2025-08-06 14:23_

Because it makes it easier to see what type is being returned without having to scroll up to resolve the `Self` symbol.

(There are some instances where `Self` is required, particularly in trait definitions. So any Clippy lint that forbade `Self` would need to know when to _not_ forbid it haha.)

---

_Comment by @adamnemecek on 2025-08-06 14:25_

@BurntSushi but you generally know inside what type you are.

---

_Comment by @BurntSushi on 2025-08-06 14:41_

I don't think I want to keep debating this. It's not always obvious (to me) which type I'm in when I jump to a place in the code. It's an opinion of mild degree that I don't hold strongly. 

---

_Comment by @adamnemecek on 2025-08-06 15:23_

@dcreager if I fix it in all the places, would you merge it?

---

_Comment by @MichaReiser on 2025-08-06 15:38_

I'll close this for now. Not because not using `Self` or inconsistency is better, but because I want to avoid such a minor detail distracting the team, and it seems there's no clear consensus.





---

_Closed by @MichaReiser on 2025-08-06 15:38_

---
