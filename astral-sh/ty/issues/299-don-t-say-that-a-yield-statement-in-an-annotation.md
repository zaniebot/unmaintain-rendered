```yaml
number: 299
title: "Don't say that a `yield` statement in an annotation scope in a function is \"not in a function\""
type: issue
state: closed
author: ntBre
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-05-09T17:23:29Z
updated_at: 2025-05-21T16:16:26Z
url: https://github.com/astral-sh/ty/issues/299
synced_at: 2026-01-12T15:54:22Z
```

# Don't say that a `yield` statement in an annotation scope in a function is "not in a function"

---

_@ntBre_

I think there is still something here. With 
```py
def _():
# error: [invalid-type-form] "`yield` expressions are not allowed in type expressions"
# error: [invalid-syntax] "yield expression cannot be used within a TypeVar bound"
    type X[T: (yield 1)] = int

def _():
# error: [invalid-type-form] "`yield` expressions are not allowed in type expressions"
# error: [invalid-syntax] "yield expression cannot be used within a type alias"
    type Y = (yield 1)
```

I still get the outside of a function errors

```bash
semantic_syntax_errors.md - Semantic syntax error diagnostics - Invalid expression

  crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md:304 unexpected error: 16 [invalid-syntax] "`yield` statement outside of a function"
  crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md:309 unexpected error: 15 [invalid-syntax] "`yield` statement outside of a function"
```

_Originally posted by @maxmynter in https://github.com/astral-sh/ruff/pull/17748#discussion_r2082080479_
            

---

_Renamed from "Fix scope handling for `yield` statements in functions" to "Don't say that a `yield` statement in an annotation scope in a function is "not in a function"" by @carljm on 2025-05-09 20:17_

---

_Label `diagnostics` added by @carljm on 2025-05-09 20:18_

---

_Label `help wanted` added by @carljm on 2025-05-09 20:18_

---

_Comment by @ntBre on 2025-05-09 22:35_

In case someone wants to get to this before me, I think you'll just need to update [this function](https://github.com/astral-sh/ruff/blob/ec2fdadb35e618624d865e7a241452d6aff833e4/crates/ty_python_semantic/src/semantic_index/builder.rs#L2503-L2506) to look more like `in_await_allowed_context` a few lines above it. And that's called [here](https://github.com/astral-sh/ruff/blob/ec2fdadb35e618624d865e7a241452d6aff833e4/crates/ruff_python_parser/src/semantic_errors.rs#L765) in case it turns out to be more involved than that.

---

_Comment by @maxmynter on 2025-05-10 15:26_

Having stumbled upon this, I would love to take a jab at it :) 

---

_Comment by @maxmynter on 2025-05-10 19:04_

PR ready in the Ruff repository: https://github.com/astral-sh/ruff/pull/18008

---

_Closed by @MichaReiser on 2025-05-21 16:16_

---
