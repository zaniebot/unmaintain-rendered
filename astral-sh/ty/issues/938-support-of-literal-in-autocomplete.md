```yaml
number: 938
title: Support of Literal in autocomplete
type: issue
state: open
author: AKuederle
labels:
  - server
  - completions
assignees: []
created_at: 2025-08-05T13:10:06Z
updated_at: 2026-01-03T18:34:19Z
url: https://github.com/astral-sh/ty/issues/938
synced_at: 2026-01-10T01:56:40Z
```

# Support of Literal in autocomplete

---

_Issue opened by @AKuederle on 2025-08-05 13:10_

It would be great, if the code suggestions could cover `Literal` and suggest only possible values for e.g. function arguments.

It would be even better, if it would be possible to support something like `Literal["option1", "option2"] | str` to allow for autocomplete of the `Literal` options without widening the type to `str`.
I often use annotations like this to indicate that the sensible values are `"option1", "option2"`, but that the function theoretically supports any str.

At the moment ty "correctly" widens the type option to just `str`, as the literal values are subtypes of `str`. However, with that we loose autocomplete for the other options.

There is "prior art" on this hack in typescript: https://medium.com/@florian.schindler_47749/typescript-hacks-1-string-suggestions-58806363afeb


---

_Label `server` added by @MichaReiser on 2025-08-05 13:58_

---

_Comment by @MichaReiser on 2025-08-05 13:58_

CC: @BurntSushi 

---

_Comment by @carljm on 2025-08-05 14:42_

Separate from the autocomplete support, this is another data point suggesting maybe we shouldn't eagerly simplify user-authored union types.

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:50_

---

_Added to milestone `Z post-stable` by @MichaReiser on 2025-11-14 08:46_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---

_Comment by @AlexWaygood on 2026-01-03 18:34_

I just closed #2318 as a duplicate of this, but it has some nice examples of autocompletions that are supported by other language servers.

---
