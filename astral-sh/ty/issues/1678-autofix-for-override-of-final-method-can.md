```yaml
number: 1678
title: "Autofix for `override-of-final-method` can introduce invalid syntax"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-11-29T15:56:25Z
updated_at: 2025-11-30T13:40:34Z
url: https://github.com/astral-sh/ty/issues/1678
synced_at: 2026-01-10T01:58:59Z
```

# Autofix for `override-of-final-method` can introduce invalid syntax

---

_Issue opened by @AlexWaygood on 2025-11-29 15:56_

If the user has something like this:

```py
from typing import final

def coinflip() -> bool:
    return True

class A:
    @final
    def method(self): ...

class B(A):
    if coinflip():
        def method(self): ...
    else:
        pass
```

then in the IDE, we currently offer an autofix that turns their code into:

```py
from typing import final

def coinflip() -> bool:
    return True

class A:
    @final
    def method(self): ...

class B(A):
    if coinflip():

    else:
        pass
```

which is invalid syntax. I think the autofix could still be useful here anyway, but ideally we would obviously not introduce invalid syntax. Ruff has [some nice infrastructure](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/fix/edits.rs#L25) to help avoid this kind of issue, but adapting it to our model might be tricky.

---

_Label `bug` added by @AlexWaygood on 2025-11-29 15:56_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-29 15:56_

---

_Label `fixes` added by @AlexWaygood on 2025-11-29 16:08_

---

_Label `diagnostics` removed by @AlexWaygood on 2025-11-29 16:08_

---

_Comment by @MichaReiser on 2025-11-29 17:10_

Hmm, I assumed we removed that fix. It seems bad to offer fixes that result in invalid syntax (In addition that I'm still not convinced that this is the **correct** fix to begin with as it's as likely that the `@final` should be removed). 

I'd remove the fix or only offer it if the parent is a `class`.

---

_Comment by @AlexWaygood on 2025-11-29 17:18_

> it's as likely that the `@final` should be removed

I would wager that in most cases where users encounter this diagnostic, the method decorated with `@final` comes from a third-party library, but they want to override it anyway. Removing the `@final` decorator from that method is outside of their control. They have two options: removing the override, or adding a `type: ignore`.

> I'd remove the fix or only offer it if the parent is a `class`.

Only offering it if the parent is a `class` would be great, yes. But how do we determine what the parent AST node is from the point where we're emitting the diagnostic in https://github.com/astral-sh/ruff/blob/69ace002102c7201f4514ffad87b87ce6a0d604f/crates/ty_python_semantic/src/types/diagnostic.rs#L3803-L3920? I don't know how to obtain that information in ty's model. If I knew how to obtain that information, it would be trivial to move Ruff's `delete_statement` routine to somewhere where ty would also be able to access it, and then just use that to implement the autofix.

---

_Comment by @MichaReiser on 2025-11-29 17:42_

> I would wager that in most cases where users encounter this diagnostic, the method decorated with @final comes from a third-party library, but they want to override it anyway. Removing the @final decorator from that method is outside of their control. They have two options: removing the override, or adding a type: ignore.

That's a fair point, but we could restrict to the fix (or mark it as not preferred, another extension), if the overriden method is not from a library

> But how do we determine what the parent AST node is from the point where we're emitting the diagnostic in [astral-sh/ruff@69ace00/crates/ty_python_semantic/src/types/diagnostic.rs#L3803-L3920](https://github.com/astral-sh/ruff/blob/69ace002102c7201f4514ffad87b87ce6a0d604f/crates/ty_python_semantic/src/types/diagnostic.rs#L3803-L3920)?

There's no convenient way today. You could get the `parsed_module`, keep a stack of ancestor nodes until you find the node with the same ID. Then take the parent. The ideal would be if our AST allows upward traversal, but I don't see this being possible anytime soon. The alternative is to have a query that builds up the inverse tree, but that's also a lot of work. For now, I'd be inclined to just remove the fix.

---

_Closed by @AlexWaygood on 2025-11-30 13:40_

---
