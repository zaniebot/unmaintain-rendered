```yaml
number: 1771
title: "Teach the `SymbolVisitor` about more statements and expressions that can define symbols"
type: issue
state: open
author: AlexWaygood
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-05T13:03:19Z
updated_at: 2025-12-05T13:03:19Z
url: https://github.com/astral-sh/ty/issues/1771
synced_at: 2026-01-12T15:54:25Z
```

# Teach the `SymbolVisitor` about more statements and expressions that can define symbols

---

_@AlexWaygood_

The `SymbolVisitor` in the ty_ide crate is much less exhaustive than the `ExportFinder` in the ty_python_semantic crate:
- It does not recognise that `type X = int` defines a symbol `X`
- It does not recognise that `for x in range(42)` defines a symbol `x`
- It does not recognise that `case bar:` in a `match` statement will define a symbol `bar`
- It does not recognise that `with expr as whatever` will define a symbol `whatever`
- It does not recognise that `(y := []).append(42)` defines a symbol `y`
- (There are more)

Most of these are edge cases that won't usually occur in the global scope in well-written Python code. But some of them (e.g. the `type X = int` case) are reasonable things that could well occur in well-written code. And the most important thing here, in my opinion, is for the two crates to have a consistent understanding of which symbols will be potentially imported by a `*` import. Currently `ty_python_semantic` does a much more rigorous job here at attempting to emulate the runtime semantics.

Cc. @BurntSushi 

---

_Label `server` added by @AlexWaygood on 2025-12-05 13:03_

---

_Label `completions` added by @AlexWaygood on 2025-12-05 13:03_

---
