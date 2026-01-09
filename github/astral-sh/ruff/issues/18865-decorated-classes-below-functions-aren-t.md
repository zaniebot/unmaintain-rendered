---
number: 18865
title: "Decorated classes below functions aren't separated with blank lines in `.pyi` files"
type: issue
state: closed
author: bzoracler
labels:
  - good first issue
  - formatter
  - help wanted
  - style
assignees: []
created_at: 2025-06-22T20:47:40Z
updated_at: 2025-06-24T14:25:46Z
url: https://github.com/astral-sh/ruff/issues/18865
synced_at: 2026-01-07T13:12:16-06:00
---

# Decorated classes below functions aren't separated with blank lines in `.pyi` files

---

_Issue opened by @bzoracler on 2025-06-22 20:47_

### Summary

In a `stub.pyi`, I expect a blank line before a class (including its decorators) when it's defined below a function.

Before formatting:

```python
def hello(): ...

@lambda _, /: _
class A: ...
```

After formatting:

```python
def hello(): ...
@lambda _, /: _
class A: ...
```

For reference, the blank line is added by the formatter when the class sits below other statements, like `import`s, assignments, and other classes.

(I couldn't provide a ruff playground link because I couldn't figure out how to make it format a source file like it would do for stubs. Does such an option in the playground exist?)

---

See https://github.com/psf/black/issues/4256 for reference - the missing blank line isn't intentional, but fixing it in black is apparently not trivial. Is it also not trivial in ruff?

### Version

ruff 0.12.0

---

_Label `formatter` added by @ntBre on 2025-06-23 00:39_

---

_Comment by @MichaReiser on 2025-06-23 07:46_

That makes sense to me. I think that should be fairly easy to change. It might be as easy as removing

https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_python_formatter/src/statement/suite.rs#L693-L706

We would need to gate the change behind preview mode because we can only release it as part of the next minor. See https://github.com/astral-sh/ruff/blob/1aad180aae84cd5d12d5a47c879183f559c871ed/crates/ruff_python_formatter/src/preview.rs#L1-L0 for a few exmaples on how to do that

---

_Label `help wanted` added by @MichaReiser on 2025-06-23 07:46_

---

_Label `style` added by @MichaReiser on 2025-06-23 07:46_

---

_Label `good first issue` added by @MichaReiser on 2025-06-23 07:47_

---

_Referenced in [astral-sh/ruff#18888](../../astral-sh/ruff/pulls/18888.md) on 2025-06-23 10:20_

---

_Closed by @MichaReiser on 2025-06-24 14:25_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---
