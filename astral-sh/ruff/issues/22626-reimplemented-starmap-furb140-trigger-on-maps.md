```yaml
number: 22626
title: "reimplemented-starmap (FURB140): trigger on maps"
type: issue
state: open
author: GideonBear
labels:
  - documentation
  - rule
  - needs-decision
  - preview
assignees: []
created_at: 2026-01-16T18:00:47Z
updated_at: 2026-01-16T18:28:41Z
url: https://github.com/astral-sh/ruff/issues/22626
synced_at: 2026-01-16T19:01:35Z
```

# reimplemented-starmap (FURB140): trigger on maps

---

_@GideonBear_

### Summary

[reimplemented-starmap (FURB140)](https://docs.astral.sh/ruff/rules/reimplemented-starmap/#reimplemented-starmap-furb140) doesn't report an issue for:
```py
map(lambda x: f(*x), l)
```
[unnecessary-map (C417)](https://docs.astral.sh/ruff/rules/unnecessary-map/#unnecessary-map-c417) suggests turning it into:
```py
(f(*x) for x in l)
```
Which is then correctly picked up by FURB140.

This means that anyone not using C417 loses out on FURB140, while it is just as valid on the first example.

[Playground](https://play.ruff.rs/6c61ffcd-91d8-4523-8935-6058591b3e80)

---

_Comment by @ntBre on 2026-01-16 18:28_

I'm curious what others think, but my first reaction is that it's okay to defer to C417 to transform this code first. We could, however, at least link to C417 in the FURB140 docs to let people know they may want to enable both.

---

_Label `rule` added by @ntBre on 2026-01-16 18:28_

---

_Label `needs-decision` added by @ntBre on 2026-01-16 18:28_

---

_Label `preview` added by @ntBre on 2026-01-16 18:28_

---

_Label `documentation` added by @ntBre on 2026-01-16 18:28_

---
