---
number: 1754
title: Auto-import should have some rudimentary support for evaluating conditionals
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-04T14:10:50Z
updated_at: 2026-01-05T14:46:11Z
url: https://github.com/astral-sh/ty/issues/1754
synced_at: 2026-01-10T01:51:14Z
---

# Auto-import should have some rudimentary support for evaluating conditionals

---

_Issue opened by @BurntSushi on 2025-12-04 14:10_

At present, auto-import assumes all conditionals are always true. This means it will return symbols that may not be available in the current environment.

Auto-import likely can't be as good as ty itself here since we want to avoid bringing in type machinery for this. But perhaps we can cover some obvious cases like:

```python
if True: ...
if False: ...
if sys.version_info >= (3, 12): ...
if TYPE_CHECKING: ...
```

---

_Label `server` added by @BurntSushi on 2025-12-04 14:10_

---

_Label `completions` added by @BurntSushi on 2025-12-04 14:10_

---

_Comment by @AlexWaygood on 2025-12-04 14:12_

The `sys.version_info` case will be hard without bringing type machinery into this, unless you want to build out an architecture for evaluating `sys.version_info` guards that's totally parallel to the way the type checker does it :/

---

_Comment by @BurntSushi on 2025-12-04 14:27_

I guess I was assuming that most would be simple conditionals like `sys.version_info >= (3, 12)`. Just supporting those seems reasonable to me if it's the vast majority of instances.

---

_Comment by @AlexWaygood on 2025-12-04 14:31_

it gets a bit more complicated to do correctly if you have something like

```py
import sys

sys = something_else_entirely()

if sys.version_info >= (3, 12):
    ...
```

but yes, it's certainly possible. Most (all?) other type checkers only support simple `sys.version_info` checks, because they do it by looking at conditions syntactically, without type inference. Ty is the odd one out here.

---

_Comment by @MichaReiser on 2025-12-04 15:33_

What do other type checkers do here? 

I don't know if auto-completion has to support conditions. I think unioning is fine in most cases and other edge cases seem very hard to handle, e.g. what if the user is typing in a narrowed sys-version branch, would we have to narrow the auto completions too?

---

_Comment by @AlexWaygood on 2025-12-04 15:38_

I think currently our UX is bad in general for code blocks the type checker infers as unreachable, and that's sort-of expected (it matches rust-analyzer and pyright, IIRC). For example, we won't include various attributes that might exist on the Python version corresponding to that`sys.version_info` block, and we will include various attributes that don't exist on the Python version corresponding to that unreachable `sys.version_info` block

---

_Comment by @carljm on 2025-12-04 17:09_

I think it is inevitable that whatever support auto-import has for `sys.version_info` conditionals will be a parallel re-implementation to what ty does. This is perhaps not ideal, but it's already the status quo: the IDE already has an entirely parallel reimplementation of `__all__` parsing, for example. I don't think that's an inherent blocker to doing it. The question is just the cost-benefit: how much value it provides, and how much work it is to implement. I think the work to implement a simple version that matches the capabilities of other type-checkers is not prohibitive. It certainly seems preferable if we don't offer to auto-complete version-guarded symbols which our type checker will immediately turn around and tell you don't exist.

I haven't checked how other type-checkers handle version conditionals in auto-import specifically. I would be somewhat surprised if they don't support it, since other type-checkers use fairly simple syntactic matching on such conditionals, which shouldn't be hard to integrate with their LSP. But I could be wrong.

---

_Comment by @MichaReiser on 2025-12-31 15:39_

@BurntSushi can you triage this (should this be part of stable/pre-stable)?

---

_Added to milestone `Stable` by @BurntSushi on 2026-01-05 14:46_

---
