```yaml
number: 1631
title: Add missing match arms code action
type: issue
state: open
author: MatthewMckee4
labels:
  - server
assignees: []
created_at: 2025-11-25T13:20:15Z
updated_at: 2025-11-26T08:39:48Z
url: https://github.com/astral-sh/ty/issues/1631
synced_at: 2026-01-12T15:54:25Z
```

# Add missing match arms code action

---

_@MatthewMckee4_

Been looking through the R-A code actions and https://rust-analyzer.github.io/book/assists.html#add_missing_match_arms looks quite cool.

before:

```py
from enum import Enum

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

def handle_color(c: Color):
    match c<CURSOR>:
        case Color.RED:
            return "red"
```

after:

```py
from enum import Enum

class Color(Enum):
    RED = 1
    GREEN = 2
    BLUE = 3

def handle_color(c: Color):
    match c:
        case Color.RED:
            return "red"
        case Color.GREEN:
            raise NotImplementedError # (or "...")
        case Color.BLUE:
            raise NotImplementedError
```

I suppose this could start with a warning diagnostic on the "c" in the match, that we have a non exhaustive match.

Currently rust emits a diagnostic

```text
missing match arm: Green and Blue not covered (rust-analyzer E0004)
```

---

_Label `server` added by @MichaReiser on 2025-11-25 13:45_

---

_Comment by @sharkdp on 2025-11-26 08:39_

> I suppose this could start with a warning diagnostic on the "c" in the match, that we have a non exhaustive match.

You already get a "non-exhaustiveness" diagnostic if you add a `-> str` return type annotation to `handle_color`. In the absence of this, the `match` does not *need* to be exhaustive. If you meant to say that we should add a new (opt-in) diagnostic/lint that would discover non-exhaustive `match` statements either way, that's certainly a reasonable idea. We certainly have all of the necessary information available in control flow analysis today.



> [rust-analyzer.github.io/book/assists.html#add_missing_match_arms](https://rust-analyzer.github.io/book/assists.html#add_missing_match_arms) looks quite cool.

Definitely agree! It's interesting to think about how this could be implemented. I guess we would start with the type that we would currently see, if we were to add a wildcard `case _` arm to the `match` statement. For your example above, we would see `Color & ~Literal[Color.RED]`, which is eagerly simplified to `Literal[Color.GREEN, Color.BLUE]`. And that union type already contains all of the information needed.

For cases where the union members are non-singletons, there are probably some questions that we need to answer (if the match subject has a type of `A | B` with non-final classes `A` and `B`, do we provide this auto-fix? Subclasses of `A` and `B` would be matched by both patterns, so this becomes order-dependent?). But starting with things like enums sounds like a huge win already.

---
