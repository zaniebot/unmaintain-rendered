---
number: 19077
title: Some invalid star expressions do not give syntax errors
type: issue
state: open
author: MeGaGiGaGon
labels:
  - bug
  - parser
assignees: []
created_at: 2025-07-01T19:01:33Z
updated_at: 2025-07-01T20:47:20Z
url: https://github.com/astral-sh/ruff/issues/19077
synced_at: 2026-01-07T13:12:16-06:00
---

# Some invalid star expressions do not give syntax errors

---

_Issue opened by @MeGaGiGaGon on 2025-07-01 19:01_

### Summary

I noticed some invalid star expressions do not give a `SyntaxError`. I tested all I could think of, but I might have missed some cases. I also tested if they fail in `ast.parse` or only in `exec`, since I think that affects where the `SyntaxError` should be raised from.
https://play.ruff.rs/140067c2-f088-4992-847a-26a6652cc88e
```py
# SyntaxError: Starred expression cannot be used here
(*
*[],)

# No error
# Should give SyntaxError: Invalid star expression
# Fails in ast.parse
print(*
*[])

# No error
# Should give SyntaxError: can't use starred expression here
# Passes ast.parse, fails in exec
f"{*[]}"

# No error
# Should give SyntaxError: starred assignment target must be in a list or tuple
# Passes ast.parse, fails in exec
for *a in []:...
```

### Version

playground

---

_Comment by @ntBre on 2025-07-01 20:47_

I think the last one is okay because it gets an error when I delete the first one. I guess I'm not quite sure that that's expected (one syntax error interfering with another), but at least we catch it otherwise.

The middle two do seem like bugs. Thanks for checking `ast.parse` vs `exec` too. The first case with `print` should be emitted by the parser as a `ParseError`, and the second should be a `SemanticSyntaxError`.

---

_Label `bug` added by @ntBre on 2025-07-01 20:47_

---

_Label `parser` added by @ntBre on 2025-07-01 20:47_

---
