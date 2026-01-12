```yaml
number: 1885
title: Documentation does not list rules in alphabetical order
type: issue
state: closed
author: AlexWaygood
labels:
  - documentation
assignees: []
created_at: 2025-12-14T16:09:26Z
updated_at: 2025-12-19T19:11:07Z
url: https://github.com/astral-sh/ty/issues/1885
synced_at: 2026-01-12T15:54:26Z
```

# Documentation does not list rules in alphabetical order

---

_@AlexWaygood_

Most of our rules are listed in alphabetical order at https://docs.astral.sh/ty/reference/rules/, but when you scroll down towards the bottom there are several that are out of order, which is confusing:

<img width="608" height="878" alt="Image" src="https://github.com/user-attachments/assets/ad730a06-3ddb-4032-bfcb-95e4c3efc46c" />

---

_Label `documentation` added by @AlexWaygood on 2025-12-14 16:09_

---

_Comment by @MichaReiser on 2025-12-14 21:59_

That's because they're sorted by default severity first. We could consider showing the default severity more prominently or, as you suggest, just sort alphabetically.

But it's not entirely random ;)

---

_Comment by @AlexWaygood on 2025-12-14 22:02_

I see! That wasn't at all obvious to me. I think I'd vote for keeping it simple and just sorting them alphabetically, unless we want to have a whole different section in the docs for each severity group.

---

_Added to milestone `Stable` by @carljm on 2025-12-15 18:47_

---

_Closed by @AlexWaygood on 2025-12-19 19:11_

---
