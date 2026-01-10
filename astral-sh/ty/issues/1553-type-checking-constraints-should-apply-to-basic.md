```yaml
number: 1553
title: "`TYPE_CHECKING` constraints should apply to basic blocks, not scopes"
type: issue
state: open
author: AlexWaygood
labels:
  - typing semantics
assignees: []
created_at: 2025-11-14T13:33:31Z
updated_at: 2025-11-18T16:10:41Z
url: https://github.com/astral-sh/ty/issues/1553
synced_at: 2026-01-10T01:58:59Z
```

# `TYPE_CHECKING` constraints should apply to basic blocks, not scopes

---

_Issue opened by @AlexWaygood on 2025-11-14 13:33_

We currently track whether an entire scope is enclosed inside an `if TYPE_CHECKING` block. But there are certain semantics that it would be useful to apply to `TYPE_CHECKING` blocks where tracking this at the scope level is not sufficiently fine-grained. For example, if your code supports Python 3.9, we allow this type alias (it doesn't really matter that it uses the PEP-604 syntax, since it will never be executed at runtime, and the intent is clear):

```py
import typing

if typing.TYPE_CHECKING:
    class Foo:
        X = int | str
```

but we don't support this type alias, which is something a user is much more likely to write:

```py
import typing

if typing.TYPE_CHECKING:
    X = int | str
```

Another case where this would be useful is `@type_check_only`. Currently we always put symbols marked as `@type_check_only` at the bottom of autocompletion suggestions, since they're not available at runtime. But this is suboptimal: ideally we wouldn't include them in the list of suggestions at all if we're outside a `TYPE_CHECKING` block (or maybe we'd keep them at the bottom of the list of suggestions), but we wouldn't apply any downranking for these symbols if the user's cursor is inside a `TYPE_CHECKING` block, e.g.

```py
if TYPE_CHECKING:
    # we could freely suggest typeshed's `_SupportsSynchronousAnext` protocol here, for example
    `_SupportsS<CURSOR>
```

We could also freely suggest modules inside `TYPE_CHECKING` blocks that we know not to exist at runtime, such as the `_typeshed` module (which contains some very useful types!):

```py
if TYPE_CHECKING:
    from _typesh<CURSOR>
```

Ruff also has a number of linter rules that require a fine-grained analysis of which basic blocks are inside `if TYPE_CHECKING` conditions. If we ever want to reimplement these rules using ty's semantic model, we will need to replicate this fine-grained understanding.

---

_Label `typing semantics` added by @AlexWaygood on 2025-11-14 13:33_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-14 17:20_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
