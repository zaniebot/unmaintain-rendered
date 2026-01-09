---
number: 19548
title: "New Rule Proposal: Consolidate imports from the same module unnecessarily split by `TYPE_CHECKING`"
type: issue
state: open
author: vivodi
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-07-25T02:52:44Z
updated_at: 2025-12-15T18:04:48Z
url: https://github.com/astral-sh/ruff/issues/19548
synced_at: 2026-01-07T13:12:16-06:00
---

# New Rule Proposal: Consolidate imports from the same module unnecessarily split by `TYPE_CHECKING`

---

_Issue opened by @vivodi on 2025-07-25 02:52_

### Summary

This issue proposes a new linting rule to discourage placing imports within a `typing.TYPE_CHECKING` block when another import from the same module already exists at the top level. This pattern offers no performance benefits and harms code readability.

**Motivation**

The primary purpose of a `TYPE_CHECKING` block is to prevent circular imports or to import types solely for static analysis, avoiding runtime overhead. However, it is often misused for imports that could be safely imported at runtime without issue, especially when the module is already being imported from.

When one object is already imported from a module at the top level, placing another import from that same module inside a `TYPE_CHECKING` block provides no tangible benefit:

1.  **No Performance Gain:** The overhead of importing the `my_library` module has already been incurred to import object `A`. Importing an additional object (`B`) from the already-loaded module is negligible.
2.  **Reduced Readability:** It unnecessarily splits imports from the same library into two separate locations, making it harder for developers to get a quick overview of the module's dependencies.
3.  **Increased Verbosity:** The code is longer and more complex than it needs to be.
4.  **Misleading Signal:** It incorrectly signals that `B` is a problematic or heavy import when it is not. The use of `TYPE_CHECKING` should be reserved for cases where it is truly necessary.

A better, more readable, and more direct approach is to consolidate the imports into a single statement.

**Proposed Rule and Autofix**

The rule would trigger when an import inside a `TYPE_CHECKING` block can be merged with an existing import from the same module outside the block. The rule should have an automatic fix that consolidates the imports.

**Non-compliant code:**

```python
from my_library import A
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from my_library import B

def func(param: "B") -> A:
    return A()
```

**Compliant code (after autofix):**

```python
from my_library import A, B

def func(param: "B") -> A:
    return A()
```

This proposal would fit well within the `flake8-type-checking` (`TC`) rules.

---

_Comment by @ntBre on 2025-07-25 13:38_

This makes some sense to me. It's similar to [runtime-import-in-type-checking-block (TC004)](https://docs.astral.sh/ruff/rules/runtime-import-in-type-checking-block/#runtime-import-in-type-checking-block-tc004) but extending to members of the same module.

---

_Label `rule` added by @ntBre on 2025-07-25 13:38_

---

_Label `needs-decision` added by @ntBre on 2025-07-25 13:38_

---

_Comment by @nickdrozd on 2025-07-28 15:58_

This is an opinionated rule. Maybe it could be a configurable option?

---

_Comment by @maltevesper on 2025-10-17 08:01_

I dont think this is to opinionated to warrant a configuration option. Apart from not changing existing code, what benefits are there for keeping the import inside the type checking block? I see only the downside of less readable code.
Personally this is a high priority issue to me, since the switch towards Python 3.14 started Ruff asking us to move the imports into typechecking blocks in the first place. So in my personal case some of the code has become a bit cluttered by the TC003 autofix.

---

_Comment by @nickdrozd on 2025-10-29 12:01_

> Apart from not changing existing code, what benefits are there for keeping the import inside the type checking block?

I like to know what imports are actually used for runtime behavior and what imports are only used for typechecking. Helpful for figuring out proper module structure.

Maybe you don't find that information interesting. That's okay, we have different opinions. Hence opinionated 🐈 

---

_Referenced in [astral-sh/ruff#21921](../../astral-sh/ruff/issues/21921.md) on 2025-12-11 17:15_

---

_Comment by @GideonBear on 2025-12-11 18:00_

To add what I said in #21921: This is the enforcement of the opposite of `strict`.

> Looks like a duplicate to me as well. Overall I don't dislike the idea, but it definitely can't be a boolean setting at this point. We don't want to cause arguably unnecessary churn in either direction, unless people specifically opt into it for aesthetics/consistency. So it would have to be a three-stage setting, either you prefer imports staying together, you prefer all the typing only imports staying in type checking blocks, even if it means splitting up imports or you have no preference and ruff will only nag you when there's a tangible benefit to moving the import beyond aesthetics/consistency.

I would personally like this to be the default when using `strict = false`, because *I don't like allowing inconsistency*. But I can see why this should be a three-stage setting instead.

---

_Comment by @GideonBear on 2025-12-11 18:27_

In any case this might need reconsideration of the name of the `strict` setting, both extremes are "strict" in their own way.

---

_Comment by @Avasam on 2025-12-15 18:01_

There's a danger here when it comes to type-checking-only symbols (often found in stubs for protocols or to expose an internal return type).
Maybe that warrant configuration, or maybe it's rare enough one should just `noqa` it. But overall an autofix wouldn't be safe here (unless specifically detecting imports used at runtime, but then [TC004](https://docs.astral.sh/ruff/rules/runtime-import-in-type-checking-block/#runtime-import-in-type-checking-block-tc004) already covers that)

---
