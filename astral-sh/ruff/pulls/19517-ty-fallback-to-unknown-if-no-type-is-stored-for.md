```yaml
number: 19517
title: "[ty] Fallback to Unknown if no type is stored for an expression"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/unknown-fallback
created_at: 2025-07-24T00:03:04Z
updated_at: 2025-07-25T02:06:19Z
url: https://github.com/astral-sh/ruff/pull/19517
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Fallback to Unknown if no type is stored for an expression

---

_Pull request opened by @carljm on 2025-07-24 00:03_

## Summary

See discussion at https://github.com/astral-sh/ruff/pull/19478/files#r2223870292

Fixes https://github.com/astral-sh/ty/issues/865

## Test Plan

Added one mdtest for invalid Callable annotation; removed `pull-types: skip` from that test file.

Co-authored-by: lipefree <willy.ngo.2000@gmail.com>


---

_Review requested from @AlexWaygood by @carljm on 2025-07-24 00:03_

---

_Review requested from @sharkdp by @carljm on 2025-07-24 00:03_

---

_Review requested from @dcreager by @carljm on 2025-07-24 00:03_

---

_Label `ty` added by @carljm on 2025-07-24 00:03_

---

_Comment by @github-actions[bot] on 2025-07-24 00:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @carljm on 2025-07-24 00:10_

I don't know what's up with "CodSpeed Macro Runners" but I don't think it's related to this PR.

---

_@dcreager approved on 2025-07-24 01:16_

:+1: I like this. There are definitely places where we're currently jumping through extra hoops to infer a type for every expression, but we can simplify those as needed moving forward.

This also opens the door for storing separate value form and type form types for each expression, I think? That would in turn let us simplify parts of the call binding machinery.

---

_@dhruvmanila approved on 2025-07-24 05:09_

This also resolves https://github.com/astral-sh/ty/issues/865 (I tested this PR locally on the examples mentioned in that issue), it might be useful to add a couple of test cases from that issue.

---

_Comment by @MichaReiser on 2025-07-24 05:25_

I think it's worth a try and we can always come back if it makes tracking down bugs harder


What I would find useful is to add least log a tracing debug message if we take the fallback branch

---

_Comment by @sharkdp on 2025-07-24 06:31_

> What I would find useful is to add least log a tracing debug message if we take the fallback branch

I thought about that as well. In https://github.com/astral-sh/ruff/pull/19478/files#r2223870292, I suggested that we might only want to do that in valid code (and in this case, it could even be an error?). Because otherwise, if the `Unknown` fallback is now our preferred way of dealing with AST nodes inside invalid code, we would see a lot of these tracing messages (in the LSP), e.g. when the user is currently typing a annotation, but it's not yet valid?

---

_@AlexWaygood approved on 2025-07-24 09:59_

I agree that some logging for the fallback case might be good, but also that it might be hard to stop that from being very noisy in an IDE context. Even if you only emit the logs when there's valid syntax, there are lots of situations where you might have valid syntax but an invalid type expression when you're halfway through typing an annotation

---

_Comment by @MichaReiser on 2025-07-24 10:03_

> If the Unknown fallback is now our preferred way of dealing with AST nodes inside invalid code, we would see a lot of these tracing messages (in the LSP), e.g. when the user is currently typing a annotation, but it's not yet valid?

That's a fair concern. I don't think it should impact most users because the LSP only shows info level logs by default.

---

_Comment by @carljm on 2025-07-25 02:03_

The tracing feels likely to be quite noisy to me, I would rather try without it -- if anyone finds themselves debugging a case like this and the tracing would have helped, we can add it.

---

_Merged by @carljm on 2025-07-25 02:05_

---

_Closed by @carljm on 2025-07-25 02:05_

---

_Branch deleted on 2025-07-25 02:05_

---
