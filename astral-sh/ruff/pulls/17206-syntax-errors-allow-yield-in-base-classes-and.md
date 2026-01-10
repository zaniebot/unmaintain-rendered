```yaml
number: 17206
title: "[syntax-errors] Allow `yield` in base classes and annotations"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: brent/fix-annotations
created_at: 2025-04-04T13:50:36Z
updated_at: 2025-04-04T17:48:55Z
url: https://github.com/astral-sh/ruff/pull/17206
synced_at: 2026-01-10T19:40:37Z
```

# [syntax-errors] Allow `yield` in base classes and annotations

---

_Pull request opened by @ntBre on 2025-04-04 13:50_

Summary
--

This PR fixes the issue pointed out by @JelleZijlstra in https://github.com/astral-sh/ruff/pull/17101#issuecomment-2777480204. Namely, I conflated two very different errors from CPython:

```pycon
>>> def m[T](x: (yield from 1)): ...
  File "<python-input-310>", line 1
    def m[T](x: (yield from 1)): ...
                 ^^^^^^^^^^^^
SyntaxError: yield expression cannot be used within the definition of a generic
>>> def m(x: (yield from 1)): ...
  File "<python-input-311>", line 1
    def m(x: (yield from 1)): ...
              ^^^^^^^^^^^^
SyntaxError: 'yield from' outside function
>>> def outer():
...     def m(x: (yield from 1)): ...
...
>>>
```

I thought the second error was the same as the first, but `yield` (and `yield from`) is actually valid in this position when inside a function scope. The same is true for base classes, as pointed out in the original comment.

We don't currently raise an error for `yield` outside of a function, but that should be handled separately.

On the upside, this had the benefit of removing the `InvalidExpressionPosition::BaseClass` variant, the `InvalidExpressionPosition::TypeAnnotation` variant, and the `allow_named_expr` field from the visitor because they were all no longer used.

Test Plan
--

Updated inline tests.


---

_Label `bug` added by @ntBre on 2025-04-04 13:50_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-04 13:50_

---

_Review requested from @dhruvmanila by @ntBre on 2025-04-04 13:50_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_parser/resources/inline/err/invalid_annotation_function.py`:4 on 2025-04-04 13:53_

These ones should perhaps stay as errors. They are a syntax error in Python 3.14 (and under `from __future__ import annotations`). They're legal otherwise in older versions, but yielding in an annotation is a very questionable idea and it will be a SyntaxError in the future, so it would make sense for Ruff to error about it.

---

_@JelleZijlstra reviewed on 2025-04-04 13:53_

---

_@ntBre reviewed on 2025-04-04 14:00_

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/err/invalid_annotation_function.py`:4 on 2025-04-04 14:00_

Oh, interesting. We can check the version before emitting these errors, but I don't think we have any others that depend on `from __future__ import annotations` yet. It does seem easiest just to make this an error on any version in that case. Thanks!

And it looks like my error message was very similar to CPython in that case too:

```pycon
>>> from __future__ import annotations
>>> def i(x: (yield 1)): ...
  File "<python-input-1>", line 1
    def i(x: (yield 1)): ...
              ^^^^^^^
SyntaxError: yield expression cannot be used within an annotation
```

---

_Comment by @github-actions[bot] on 2025-04-04 14:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@JelleZijlstra reviewed on 2025-04-04 14:00_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_parser/resources/inline/err/invalid_annotation_function.py`:4 on 2025-04-04 14:00_

For reference: https://peps.python.org/pep-0649/#changes-to-allowable-annotations-syntax

Up to you how to handle this in Ruff. You could raise a SyntaxError on all versions for simplicity (pro: helps prepare people for the error, simpler to implement; con: not strictly accurate on 3.13 and earlier, might have false positives in the unlikely case people are actually using `yield` etc. in annotations). You could also add a new error code specifically for this case, so people can ignore it if they want. Or you can just leave it as in this PR for now, and make it a SyntaxError for >3.14 only once you add 3.14 support.

---

_@ntBre reviewed on 2025-04-04 14:03_

---

_Review comment by @ntBre on `crates/ruff_python_parser/resources/inline/err/invalid_annotation_function.py`:4 on 2025-04-04 14:03_

Oh is this the same for named expressions too? I had already skipped those in the initial PR because they were allowed in 3.13:

```pycon
>>> def i(x: (x:=1)): ...
...
>>> from __future__ import annotations
>>> def i(x: (x:=1)): ...
  File "<python-input-2>", line 1
    def i(x: (x:=1)): ...
              ^^^^
SyntaxError: named expression cannot be used within an annotation
>>>
```

---

_@JelleZijlstra reviewed on 2025-04-04 14:37_

---

_Review comment by @JelleZijlstra on `crates/ruff_python_parser/resources/inline/err/invalid_annotation_function.py`:4 on 2025-04-04 14:37_

Yes, and `await`. (The comment above was written before I saw your reply.)

---

_Comment by @ntBre on 2025-04-04 15:38_

I added a `SemanticSyntaxContext::future_annotations_or_stub` method for checking future annotations and restored the function annotations check in that case (or on Python 3.14). I also added `await` handling in these positions to #11934 since I left them out of the initial PR here. For now I just want to fix the existing false positive before the hotfix release.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/semantic_errors.rs`:210 on 2025-04-04 15:47_

Do we need the same future check here to determine the position or are functions different?

---

_@MichaReiser reviewed on 2025-04-04 15:47_

---

_@MichaReiser approved on 2025-04-04 15:48_

---

_@ntBre reviewed on 2025-04-04 16:02_

---

_Review comment by @ntBre on `crates/ruff_python_parser/src/semantic_errors.rs`:210 on 2025-04-04 16:02_

I think base classes still allow this, at least with future annotations:

```pycon
>>> from __future__ import annotations
>>> class F(y := list): ...
... def f[T]():
...     class G((yield 1)): ...
...     class H((yield from 1)): ...
...
```

Although I'm now realizing that this applies to other annotations not considered in this PR:

```pycon
>>> def f():
...     x: (yield 1)
...
>>> from __future__ import annotations
>>> def f():
...     x: (yield 1)
...
  File "<python-input-2>", line 2
    x: (yield 1)
        ^^^^^^^
SyntaxError: yield expression cannot be used within an annotation
```

Another addition to #11934...

---

_Merged by @ntBre on 2025-04-04 17:48_

---

_Closed by @ntBre on 2025-04-04 17:48_

---

_Branch deleted on 2025-04-04 17:48_

---

_Label `preview` added by @ntBre on 2025-04-04 17:48_

---
