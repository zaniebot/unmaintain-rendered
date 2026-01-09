---
number: 15941
title: "[red-knot] Markdown tests: merge multiple code blocks in a single section"
type: issue
state: closed
author: sharkdp
labels:
  - testing
  - ty
assignees: []
created_at: 2025-02-04T15:21:34Z
updated_at: 2025-02-05T21:26:17Z
url: https://github.com/astral-sh/ruff/issues/15941
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Markdown tests: merge multiple code blocks in a single section

---

_Issue opened by @sharkdp on 2025-02-04 15:21_

### Description

As outlined [here](https://github.com/astral-sh/ruff/pull/15704#issuecomment-2614068199):

* concatenate all code blocks without an explicit path in a single section
* additional checks to avoid confusion for authors of tests
  * forbid multiple code blocks without a path in the presence of code paths with paths
  * forbid multiple code blocks without a path in the presence of code blocks with other languages (pyi, toml, …)

This would allow us to write literal tests where the prose is simply interposed in the otherwise normal flow of this specific test (which is still identified by a section heading). For example, I recently wrote this:

````markdown
### Subtypes of `int`

All integer literals are subtypes of `int`:

```py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[0], int))
static_assert(is_subtype_of(Literal[1], int))
static_assert(is_subtype_of(Literal[54165], int))
```

It is tempting to think that `int` is equivalent to the union of all integer literals,
`… | Literal[-1] | Literal[0] | Literal[1] | …`, but this is not the case. `True` and `False` are
also inhabitants of the `int` type, but they are not inhabitants of any integer literal type:

```py path=true_and_false.py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[True], int))
static_assert(is_subtype_of(Literal[False], int))

static_assert(not is_subtype_of(Literal[True], Literal[1]))
static_assert(not is_subtype_of(Literal[False], Literal[0]))
```
````

It would be really nice if I didn't have to come up with am irrelevant file name, and if I didn't have to repeat all my includes:

````markdown
### Subtypes of `int`

All integer literals are subtypes of `int`:

```py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[0], int))
static_assert(is_subtype_of(Literal[1], int))
static_assert(is_subtype_of(Literal[54165], int))
```

It is tempting to think that `int` is equivalent to the union of all integer literals,
`… | Literal[-1] | Literal[0] | Literal[1] | …`, but this is not the case. `True` and `False` are
also inhabitants of the `int` type, but they are not inhabitants of any integer literal type:

```py
static_assert(is_subtype_of(Literal[True], int))
static_assert(is_subtype_of(Literal[False], int))

static_assert(not is_subtype_of(Literal[True], Literal[1]))
static_assert(not is_subtype_of(Literal[False], Literal[0]))
```
````

which would then render like this:

---

### Subtypes of `int`

All integer literals are subtypes of `int`:

```py
from knot_extensions import static_assert, is_subtype_of

static_assert(is_subtype_of(Literal[0], int))
static_assert(is_subtype_of(Literal[1], int))
static_assert(is_subtype_of(Literal[54165], int))
```

It is tempting to think that `int` is equivalent to the union of all integer literals, `… | Literal[-1] | Literal[0] | Literal[1] | …`, but this is not the case. `True` and `False` are also inhabitants of the `int` type, but they are not inhabitants of any integer literal type:

```py path=true_and_false.py
static_assert(is_subtype_of(Literal[True], int))
static_assert(is_subtype_of(Literal[False], int))

static_assert(not is_subtype_of(Literal[True], Literal[1]))
static_assert(not is_subtype_of(Literal[False], Literal[0]))
```


---

_Label `testing` added by @sharkdp on 2025-02-04 15:21_

---

_Label `red-knot` added by @sharkdp on 2025-02-04 15:21_

---

_Comment by @MichaReiser on 2025-02-04 15:24_

The only downside I see to this is that line numbers in diagnostics could be off. But I think that's fine. I don't forsee us writing many pros-tests that also snapshot the entire diagnostic.

---

_Comment by @sharkdp on 2025-02-04 16:16_

> The only downside I see to this is that line numbers in diagnostics could be off.

That's a good point. If we can't patch them up, it might be difficult to debug those tests as well :slightly_frowning_face: 

---

_Comment by @MichaReiser on 2025-02-04 17:50_

I think it's a good enough trade off. We do the same for jupyter notebooks. It just means that it is a deliberate choice whether one uses a prose like test or not.

---

_Assigned to @sharkdp by @sharkdp on 2025-02-04 20:05_

---

_Referenced in [astral-sh/ruff#15950](../../astral-sh/ruff/pulls/15950.md) on 2025-02-04 21:43_

---

_Comment by @sharkdp on 2025-02-04 21:47_

I think I managed to make line numbers work for multi-snippet tests by ~~thinking very carefully about the problem~~ randomly adding and subtracting 1 in various places (#15950). I need to look at that again tomorrow.

---

_Comment by @MichaReiser on 2025-02-04 21:52_

Interesting. We probably won't be able to do the same for full diagnostic snapshots 

---

_Closed by @sharkdp on 2025-02-05 21:26_

---

_Closed by @sharkdp on 2025-02-05 21:26_

---
