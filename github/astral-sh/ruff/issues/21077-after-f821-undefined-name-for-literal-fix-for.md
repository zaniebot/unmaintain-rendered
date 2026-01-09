---
number: 21077
title: "After F821 (Undefined name) for \"Literal\", fix for PYI020 (QuotedAnnotationInStub) is undesirable"
type: issue
state: open
author: pajod
labels:
  - question
assignees: []
created_at: 2025-10-26T00:52:16Z
updated_at: 2025-10-27T19:24:47Z
url: https://github.com/astral-sh/ruff/issues/21077
synced_at: 2026-01-07T13:12:16-06:00
---

# After F821 (Undefined name) for "Literal", fix for PYI020 (QuotedAnnotationInStub) is undesirable

---

_Issue opened by @pajod on 2025-10-26 00:52_

### Summary

When combined with some other formatting fix, this one is still consistent .. but hardly ever helpful:
```diff
ruff check --diff --no-fix --fix --isolated --stdin-filename="r.pyi" --select=PYI020,F821 -- <<<'v: Literal["return", "0o7", "0*3", "int", "foo"]'
--- r.pyi
+++ r.pyi
@@ -1 +1 @@
-v: Literal["return", "0o7", "0*3", "int", "foo"]
+v: Literal["return", 0o7, 0*3, int, foo]

Would fix 4 errors.
```
NB: Selected F821 only to showcase what is going on. Nothing special about that, just happens to match one case where PYI020 cannot be positively confirmed to be safe to apply.

* #11378

---

_Referenced in [python/typeshed#14921](../../python/typeshed/pulls/14921.md) on 2025-10-26 00:56_

---

_Comment by @ntBre on 2025-10-27 14:09_

Is the undesirable part that `"return"` is still quoted? I also tried this without selecting F821, and I think I got the same result, but I might be missing something there. I'm assuming that `return` is skipped since it's a keyword. If you omit `--diff` and `--fix`, you can see that diagnostics are only emitted for the other entries:

```
> ruff check --preview --no-fix --isolated --stdin-filename="r.pyi" --select=PYI020 - <<<'v: Literal["return", "0o7", "0*3", "int", "foo"]'
PYI020 [*] Quoted annotations should not be included in stubs
 --> r.pyi:1:22
  |
1 | v: Literal["return", "0o7", "0*3", "int", "foo"]
  |                      ^^^^^
  |
help: Remove quotes
  - v: Literal["return", "0o7", "0*3", "int", "foo"]
1 + v: Literal["return", 0o7, "0*3", "int", "foo"]

PYI020 [*] Quoted annotations should not be included in stubs
 --> r.pyi:1:29
  |
1 | v: Literal["return", "0o7", "0*3", "int", "foo"]
  |                             ^^^^^
  |
help: Remove quotes
  - v: Literal["return", "0o7", "0*3", "int", "foo"]
1 + v: Literal["return", "0o7", 0*3, "int", "foo"]

PYI020 [*] Quoted annotations should not be included in stubs
 --> r.pyi:1:36
  |
1 | v: Literal["return", "0o7", "0*3", "int", "foo"]
  |                                    ^^^^^
  |
help: Remove quotes
  - v: Literal["return", "0o7", "0*3", "int", "foo"]
1 + v: Literal["return", "0o7", "0*3", int, "foo"]

PYI020 [*] Quoted annotations should not be included in stubs
 --> r.pyi:1:43
  |
1 | v: Literal["return", "0o7", "0*3", "int", "foo"]
  |                                           ^^^^^
  |
help: Remove quotes
  - v: Literal["return", "0o7", "0*3", "int", "foo"]
1 + v: Literal["return", "0o7", "0*3", "int", foo]

Found 4 errors.
[*] 4 fixable with the `--fix` option.
```

---

_Label `question` added by @ntBre on 2025-10-27 14:10_

---

_Comment by @pajod on 2025-10-27 18:56_

> Quoted annotations should not be included in stubs

Slightly exaggerated verbosity for the edge case:

> Quoted annotations should not be included in stubs, except when expressing string literals, such as with typing.Literal. I know that I have no way of determining if this `Literal` thing is `typing.Literal`, but lets assume the common case: it is not. I tested my assumption on the first element and unambiguously failed. So **this fix is not safe here**, probably because my assumption about what kind of subscript I am dealing with was incorrect. I could stop here, but where is the fun in that? I am going to try the same "fix" on all the other occurrences and their sub-elements. Possibly, some of them are going to produce valid syntax, yet change meaning in less obvious ways. There is never a bad day for users to learn about the `--show-traceback` option in mypy!

Suggested extra output, conditional on simple identifier name matching (whether actual behavior is changed or not):

```
hint: Did you mean: `typing.Literal` here?
 --> a.pyi:1:4
1 | v: Literal[3e+9999, "0o8", int, "int" "", .0^.0]
  |    ^^^^^^^
```

---

_Referenced in [astral-sh/ruff#21102](../../astral-sh/ruff/pulls/21102.md) on 2025-10-27 19:44_

---
