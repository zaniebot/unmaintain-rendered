---
number: 16886
title: Types assigned to a type alias should be moved under type checking block
type: issue
state: open
author: haarisr
labels:
  - rule
assignees: []
created_at: 2025-03-21T07:23:26Z
updated_at: 2025-03-24T07:56:13Z
url: https://github.com/astral-sh/ruff/issues/16886
synced_at: 2026-01-07T13:12:16-06:00
---

# Types assigned to a type alias should be moved under type checking block

---

_Issue opened by @haarisr on 2025-03-21 07:23_

### Summary

```python3
# Before

from __future__ import annotations
from pathlib import Path

type Paths = list[Path] # Ruff should raise an error to move path to a type checking block
```

```python3
# After

from __future__ import annotations
from typing import TYPE_CHECKING

if TYPE_CHECKING:
    from pathlib import Path

type Paths = list[Path]
```
I can run the code in the second example and python does not raise an error. I believe this is valid with type aliases.

I would like ruff to tell me to move the pathlib import under a type checking block.

### Version

_No response_

---

_Comment by @ntBre on 2025-03-21 11:33_

This sounds like it should fall under [typing-only-standard-library-import (TC003)](https://docs.astral.sh/ruff/rules/typing-only-standard-library-import/#typing-only-standard-library-import-tc003), but it looks like it's not being found for type aliases.

---

_Label `rule` added by @ntBre on 2025-03-21 11:33_

---

_Comment by @Daverball on 2025-03-21 12:50_

I'm kind of surprised that this doesn't already work for type statements. The type expression in type statements is deferred and I was under the impression that all deferred type expression are considering in a typing execution context, rather than a runtime execution context. And TC004 should trigger if all the references are inside typing execution context. But I guess the flags for typing only annotations only get set on annotations, but not on type statements or type parameter bounds/defaults, even though they're all deferred.

There is however an argument to be made, that it's less safe for type aliases to refer to typing only bindings than other annotations, since it's more likely to end up being passed to something that does need runtime access to the types and since we don't have multi-file analysis that would more likely result in false positives.

That being said #14787 provides new options for quoting types in annotated type aliases. So arguably type statements should receive a similar treatment and get a lever you can pull to influence whether or not you want to consider the value expression of type statements in typing or runtime context for the purposes of TC001-004.

Although to be fair, the flake8 plugin currently doesn't consider type statements runtime uses and there's no option to change it there either. So it might be fine to align ruff's behavior with the flake8 plugin. I can make an exploratory PR to see what the impact of this change would be (although I'm not sure how many Python 3.12+ projects there are in the ecosystem results, if any).

As a side note: `from __future__ import annotations` has no effect on type statements.

---

_Comment by @eli-schwartz on 2025-03-23 09:25_

> There is however an argument to be made, that it's less safe for type aliases to refer to typing only bindings than other annotations, since it's more likely to end up being passed to something that does need runtime access to the types and since we don't have multi-file analysis that would more likely result in false positives.

I'm not sure I fully understand this. I use the style `Paths: TypeAlias = list[path]` quite a bit for providing better type annotations in type-checking-only code, including the ability to change the argument contract in one place and have it affect multiple locations. It also allowed recursive types which solved a particularly nasty problem we previously had. But I wouldn't dream of ever using runtime annotations. I'm quite happy simply having better mypy checking.

Why would a type alias be more likely to be used at runtime than an ordinary class type?

...

Somewhat amusingly, flake8-type-checking beat ruff with PEP 695 type aliases, but ruff beat flake8-type-checking with TypeAlias plus manually stringified `Path`. (Both report TC003 if a PEP 695 type alias is quoted.)

Nobody seems to be a fan of moving either variant itself into the `if T.TYPE_CHECKING:` block, only suggesting to move the pathlib import...

---

_Comment by @Daverball on 2025-03-23 10:02_

@eli-schwartz Problems arise when a type alias is used inside something like a pydantic model. Ruff will detect that the reference to the type alias can't be a forward reference, but if the type alias is defined in another file it can't detect that the value expression of the type alias also needs to be available at runtime. So that's where the argument comes from. We can detect this case as long as the type alias is defined in the same file, and I do that check in #14787 for deciding whether or not it's fine to quote references in the value expression of an annotated type aliases.

So there's multiple ways we could decide to go here. We could decide that `flake8-type-checking` is opinionated and decide that type aliases generally shouldn't be used at runtime, so we can safely treat those value expressions as typing only if we can't prove that it's unsafe in the same file. We could decide this is an ambiguous case and we should neither emit TC001-003 nor TC004, which is essentially the status quo in ruff, if I'm not mistaken. We could employ some heuristics, like be more liberal for type aliases prefixed with an underscore and more careful about type aliases than will potentially be imported by other modules. We could also decide this needs to be configurable and add a setting for which way ruff should lean.

---

_Comment by @eli-schwartz on 2025-03-23 10:22_

> Problems arise when a type alias is used inside something like a pydantic model.

Sure but this is the case for all code anywhere? What I don't understand is how this differs from other annotations purely due to the fact that it is an alias. Why would its being an alias matter -- I would assume people using pydantic are likely to use "types imports, including type aliases" at runtime, and other people would use neither.



> We could also decide this needs to be configurable and add a setting for which way ruff should lean.

Sure, like `flake8.type-checking-pydantic-enabled`. Though it seems ruff wants much more granular option handling.

---

_Comment by @Daverball on 2025-03-23 11:04_

Yes, it is a problem for any type alias, due to its indirect nature. It isn't a problem for other types though, since there we have options like `runtime-evaluated-base-classes` that ensure that annotations are treated as runtime required by ruff.

The main difference is that type aliases are indirect, so `runtime-evaluated-base-classes` only ensures that the type alias itself is available at runtime, but not the types its referring to. That makes it inherently different from other annotations. Yes, someone can still execute something like `typing.get_type_hints` on our functions/classes/modules and run into problems, but that's less likely to be a problem, than a type alias (it's only really likely to come up with something like beartype, which has magic to insert runtime type checking into any python code with annotations), which might directly be used in an annotation that needs to be available at runtime.

So it's reusability vs. full runtime introspection. I agree that if you want full runtime introspection of your annotations, then flake8-type-checking is the wrong tool, because you don't want any typing only imports at all if you want full runtime introspection. But you may still want people to be able to use your type aliases that are part of the public API with something like pydantic, so they aren't forced to redefine that type alias themselves. Or maybe a more compelling use-case: You want to be able to define type aliases for your own pydantic models, without having to `noqa` every import that's used in the type alias.

That's why it's not quite the same to decide that deferred type aliases should be typing-only uses than it is to decide that deferred annotations should be typing-only uses. It's more likely to cause false positives that people currently would have no way to avoid beyond a `noqa`.

---

_Comment by @eli-schwartz on 2025-03-23 22:52_

> The main difference is that type aliases are indirect, so `runtime-evaluated-base-classes` only ensures that the type alias itself is available at runtime, but not the types its referring to.

If it's possible to tag all type aliases needed at runtime, can that not then tag all the types inside the alias and imported in the same file as recursively needed at runtime?

> But you may still want people to be able to use your type aliases that are part of the public API with something like pydantic, so they aren't forced to redefine that type alias themselves.

That sounds tough to solve.

> Or maybe a more compelling use-case: You want to be able to define type aliases for your own pydantic models, without having to `noqa` every import that's used in the type alias.

... but that should be solved if it's possible to recursively mark those imports as used?

---

_Comment by @Daverball on 2025-03-24 07:54_

> If it's possible to tag all type aliases needed at runtime, can that not then tag all the types inside the alias and imported in the same file as recursively needed at runtime?

Yes, but not across multiple files, so we can only ensure this for type aliases defined in the same file as they are used. Although doing it recursively will be somewhat challenging since we would need to detect recursive type aliases, so we don't get stuck in an infinite loop. I have a non-recursive version of this check implemented in one of my open PRs for annotated type aliases. It's easier to get away with a non-recursive version for annotated type aliases, since the value expression is always executed at runtime (although it's still possible to get the wrong results when forward references are involved), for type statements the need for a recursive version for accurate results would be far greater.

Overall I think we mostly agree, it's just that Ruff is trying much harder for the out of the box experience to yield few false positives, whereas the flake8 plugin is a bit more liberal, since there are no fixes that could automatically be applied. So it's important to weigh the cost of making this work for people that don't care about runtime typing and instead care about reducing import overhead as much as possible against the flexibility of making these rules still useful for people that do care about runtime typing to some degree, because they use a popular library like SQLAlchemy, FastAPI, pydantic, cattrs etc.

We may be able to get away with a balanced approach that works for most use-cases using some heuristics, but we may also end up needing to provide a configuration option, if we want to satisfy most people's use-cases, instead of just a niche audience. It's difficult for me to predict which of the two is more likely to end up successful.

---

_Referenced in [astral-sh/ruff#16981](../../astral-sh/ruff/pulls/16981.md) on 2025-03-26 13:01_

---
