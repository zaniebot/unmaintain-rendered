---
number: 14000
title: RUF027 False positive for non-runtime context binding
type: issue
state: closed
author: Daverball
labels:
  - rule
assignees: []
created_at: 2024-10-30T14:52:33Z
updated_at: 2024-12-18T07:50:50Z
url: https://github.com/astral-sh/ruff/issues/14000
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF027 False positive for non-runtime context binding

---

_Issue opened by @Daverball on 2024-10-30 14:52_

This might be incredibly easy to fix depending on how this check is implemented. Currently RUF027 can be triggered by something like:

```python
from typing import TYPE_CHECKING
if TYPE_CHECKING:
    from datetime import date

path = "foo/{date}/"  # RUF027
```

Now, to be fair, this would also be a false positive for a runtime binding. But this seems like an easy low hanging fruit to reduce the false positive rate at least a little bit.

Although a more reliable solution long-term would be to add a setting for specifying additional methods which expect a template string.

---

_Comment by @MichaReiser on 2024-10-30 15:00_

> Now, to be fair, this would also be a false positive for a runtime binding. But this seems like an easy low hanging fruit to reduce the false positive rate at least a little bit.

Can you talk me through the heuristic you have in mind to identifying the false positives? Is it that ruff should ignore all strings in function decorator positions if the function has a parameter with the same name?

---

_Label `rule` added by @MichaReiser on 2024-10-30 15:00_

---

_Comment by @Daverball on 2024-10-30 15:18_

It's very simple. From what I understand, this rule triggers when a string template contains a name that would reference a binding if it were changed into a f-string. But if the binding is not in a runtime context, then turning it into a f-string would trigger a `NameError` (or `UnboundLocalError` if the name is shadowed later in the same scope).

So all I'm suggesting is to ignore references that can't actually be used at runtime.

---

_Comment by @Daverball on 2024-10-30 15:33_

@MichaReiser I've simplified the example, so it's just about the runtime context of the binding and there's no additional visual noise, even if it would trigger additional violations for the unused import.

---

_Comment by @AlexWaygood on 2024-10-30 15:34_

@Daverball, is this something that's actually causing false-positive errors to be emitted on your code, or is this just a theoretical concern? It feels like a bit of an edge case

---

_Comment by @Daverball on 2024-10-30 15:41_

@AlexWaygood I am seeing some false positives in my code that would go away with this heuristic, yes, otherwise I would not have reported it. Since you're already keeping track of the runtime context of bindings in the semantic model anyways this seemed like a fairly cheap win.

Although as I've said, being able to specify functions in addition to `gettext`/`logging`/`FastAPI.path` where this check is skipped would be more reliable.

---

_Comment by @AlexWaygood on 2024-10-30 15:47_

> @AlexWaygood I am seeing some false positives in my code that would go away with this heuristic, yes, otherwise I would not have reported it.

Thanks for confirming. We get more issues reported than you might think which _are_ actually purely theoretical, so I just wanted to check :-)

I agree that RUF027 has too many false positives right now, and I'd happily accept any PR that's not too complicated and manages to reduce the number of false positives from the rule! Thanks for opening the issue with the suggestion.

---

_Comment by @Daverball on 2024-10-30 15:51_

I just realized that this might come for free with #12927, since this will add `simulate_runtime_load` to the semantic model, the current implementation appears to be using `lookup_symbol` which handles deletion properly, but doesn't care about which context the lookup happens from, it just assumes it's being called at the right time during semantic analysis.

So if that ever gets merged I will open a PR to change `lookup_symbol` to `simulate_runtime_load`.

---

_Referenced in [astral-sh/ruff#15037](../../astral-sh/ruff/pulls/15037.md) on 2024-12-17 13:39_

---

_Closed by @MichaReiser on 2024-12-18 07:50_

---

_Closed by @MichaReiser on 2024-12-18 07:50_

---
