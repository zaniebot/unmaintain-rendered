```yaml
number: 1679
title: Show suggested fixes when rendering diagnostics on the CLI
type: issue
state: open
author: AlexWaygood
labels:
  - cli
  - diagnostics
  - fixes
assignees: []
created_at: 2025-11-29T16:03:05Z
updated_at: 2025-12-02T07:49:09Z
url: https://github.com/astral-sh/ty/issues/1679
synced_at: 2026-01-12T15:54:25Z
```

# Show suggested fixes when rendering diagnostics on the CLI

---

_@AlexWaygood_

For example, we now offer an IDE autofix for this snippet that will correct `"naem"` to `"name"`:

```py
from typing import TypedDict

class Person(TypedDict):
    name: str

def f(p: Person):
    print(p["naem"])
```

This autofix is [also included in our test snapshots](https://github.com/astral-sh/ruff/blob/594b7b04d3b04bcf42861f86207017c8117678ca/crates/ty_python_semantic/resources/mdtest/snapshots/typed_dict.md_-_%60TypedDict%60_-_Diagnostics_(e5289abf5c570c29).snap#L239-L255) now. However, we don't render the suggested fix on the CLI yet:

<img width="1166" height="416" alt="Image" src="https://github.com/user-attachments/assets/8ac271e7-2566-4c2b-9691-ac866bbe3ea1" />

Even if the user does not want the autofix automatically applied when they invoke `ty check` on their code, the suggested fix can be very helpful in informing the user how they might fix the problem, so we should show it.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-11-29 16:03_

---

_Label `cli` added by @AlexWaygood on 2025-11-29 16:03_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-29 16:03_

---

_Label `fixes` added by @AlexWaygood on 2025-11-29 16:14_

---

_Comment by @MichaReiser on 2025-11-29 17:07_

I was a bit confused about why we aren't showing this particular fix, since we are showing fixes on the CLI. The reason is that this fix is unsafe, so we might need a concept similar to Ruff's unsafe fixes.

Figuring out how to handle unsafe fixes is definitely something we want to do. However, I'm not sure whether rendering a fix for this diagnostic is actually useful, given that the primary annotation already contains all the information. It feels like we'll just present the exact same information in a much verbose way

---

_Comment by @AlexWaygood on 2025-11-29 17:14_

Hmm, yes... I mainly opened this issue because of your comment in https://github.com/astral-sh/ruff/pull/21667#discussion_r2569692570, but I think I agree with you on all points here! Happy to close this again if you don't think this is a helpful issue for tracking purposes.

I think most of ty's fixes will probably be marked as unsafe (most of them will change behaviour in some way), and I don't think something like Ruff's `--fix --unsafe-fixes` mode would be helpful on the CLI. In this case, it _might_ be a simple `naem -> name` typo (it's a very good guess), but it also might not -- as a ty user, I'd want to review each suggested edit in isolation before deciding whether to apply it to my code. That implies something like a "review mode" would be more helpful.

---

_Comment by @carljm on 2025-12-01 23:23_

I'm not sure this belongs in stable milestone, but since it's CLI/fix related I'll eave that to @MichaReiser to decide

---

_Removed from milestone `Stable` by @MichaReiser on 2025-12-02 07:49_

---
