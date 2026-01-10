```yaml
number: 1790
title: Add hover info for language keywords
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
  - wish
assignees: []
created_at: 2025-12-05T23:15:17Z
updated_at: 2025-12-06T01:29:47Z
url: https://github.com/astral-sh/ty/issues/1790
synced_at: 2026-01-10T01:56:41Z
```

# Add hover info for language keywords

---

_Issue opened by @MeGaGiGaGon on 2025-12-05 23:15_

One of my favorite features from rust-analyzer, if you hover over a keyword it shows documentation about that keyword.

<img width="872" height="136" alt="Image" src="https://github.com/user-attachments/assets/b9267f07-a440-4fb6-92cd-af83ecf6ae84" />

The main place I would find this very helpful in is with `for`/`while` loops and their `else` blocks. I use them just rarely enough I forget the exact semantics, but also go through some headache trying to figure them back out when I need to use them. Having that sort of info available in a hover would be very nice.

This also helps when learning/picking back up the language. I often go a good stretch of time between using rust, so being able to quickly pull up usage info on the language itself is very nice. I could see that same sort of benefit also translating to python.

---

_Label `server` added by @AlexWaygood on 2025-12-05 23:16_

---

_Comment by @Gankra on 2025-12-05 23:42_

Off the top of my head: this would be a new GotoTarget with a special implementation of the docstring method that the hover code invokes. possibly would need to make a new specialcase for ResolvedDefinition too.

Are there specific docs for python keywords that are worth showing? Seems like a similar problem to special forms having no docs for us to show.

---

_Comment by @MeGaGiGaGon on 2025-12-05 23:58_

There is docs from The Python Language Reference that could be used, though I don't think that actually goes with python so in the end it would be something built into ty. They also are not the best in terms of readability
https://docs.python.org/3/reference/expressions.html
https://docs.python.org/3/reference/simple_stmts.html
https://docs.python.org/3/reference/compound_stmts.html
(rust-analyzer also shows for example the equal docs on hovering `==`, which is where most of the expressions reference would come in handy, though keywords also also there)

---

_Label `wish` added by @Gankra on 2025-12-06 01:29_

---
