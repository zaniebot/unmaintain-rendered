```yaml
number: 11511
title: "Replace most usages of `lex_starts_at` with `Tokens`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/update-up031
created_at: 2024-05-23T11:02:12Z
updated_at: 2024-05-28T07:23:19Z
url: https://github.com/astral-sh/ruff/pull/11511
synced_at: 2026-01-12T15:55:38Z
```

# Replace most usages of `lex_starts_at` with `Tokens`

---

_@dhruvmanila_

## Summary

Part of #11401 

This PR refactors most usages of `lex_starts_at` to use the `Tokens` struct available on the `Program`.

This PR also introduces the following two APIs:
1. `count` (on `StringLiteralValue`) to return the number of string literal parts in the string expression
2. `after` (on `Tokens`) to return the token slice after the given `TextSize` offset

## Test Plan

I don't really have a way to test this currently and so I'll have to wait until all changes are made so that the code compiles.


---

_Label `internal` added by @dhruvmanila on 2024-05-23 11:02_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-23 11:02_

---

_Renamed from "Remove `lex_starts_at` usage from `UP031`" to "Replace most usages of `lex_starts_at` with `Tokens`" by @dhruvmanila on 2024-05-23 12:46_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:256 on 2024-05-27 12:44_

Could we just pass `program` to `Importrer::new` and let it extract the relevant fields?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/imports.rs`:22 on 2024-05-27 12:46_

What do you think of storing `tokens` on `Indexer`? I'm asking because the ruff linter has so many structs that methods need to do anything useful and it would be nice if we could reduce them to fewer. 

Edit: I see that there are other places where `Indexer` isn't available. Hopefully we can reudce the number of structs passed around with red knot.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1722 on 2024-05-27 13:00_

Nit: I wouldn't call this `count` because it isn't actually counting. It's also unclear how it is different from `len` (which probably should return a `TextSize`)

Maybe `literals_len`. The alternative is to add `len` as a method to `Iter` and add a `literals` method so that it reads as `string.literals().len()`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:425 on 2024-05-27 13:01_

Do we depend on this behavior? I think I would rather have the implementation `panic` if the offset isn't at a token boundary.

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:420 on 2024-05-27 13:03_

I'm not sure in which PR you added `tokens_in_range` and I also don't know if it is a good idea. 

We could avoid the `end` binary search by either:
* doing a linear serach from the start. That would be based on the assumption that the end is close to the start (linear search has better cache locality)
* Return a custom iterator that lazily checks `end` in the `next` call. I don't know if that's feasible or if any logic depends on the fact that `tokens_in_range` returns a slice.

---

_@MichaReiser approved on 2024-05-27 13:04_

---

_@dhruvmanila reviewed on 2024-05-28 04:39_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1722 on 2024-05-28 04:39_

Why do you think it isn't counting? I can change it to `.as_slice().len()` or `.iter().count()` - both are available.

---

_@dhruvmanila reviewed on 2024-05-28 04:41_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/checkers/imports.rs`:22 on 2024-05-28 04:41_

Here, I can just pass the `Program` which has both `Suite` and `Tokens` similar to your suggestion above for `Importer::new`. As such, if I pass `Program` in `Importer::new`, the `check_imports` should take `Program` because later on it calls `Importer::new`.

---

_@dhruvmanila reviewed on 2024-05-28 04:45_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1722 on 2024-05-28 04:45_

I've changed it to `.as_slice().len()`

---

_@dhruvmanila reviewed on 2024-05-28 04:47_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:425 on 2024-05-28 04:47_

I think I'll have a clearer picture once the code compiles. I plan on being able to do that by today as that will allow me to do a lot of testing. And, I plan on revisiting the APIs once everything is changed.

---

_@dhruvmanila reviewed on 2024-05-28 04:50_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:420 on 2024-05-28 04:50_

> I also don't know if it is a good idea.

Why though?

A lot of usages of the lexer are to get the tokens within a specified range, usually the range belongs to a node. This does mean that the token tree would be more useful than this.

> We could avoid the `end` binary search by either:
> 
> * doing a linear serach from the start. That would be based on the assumption that the end is close to the start (linear search has better cache locality)
> * Return a custom iterator that lazily checks `end` in the `next` call. I don't know if that's feasible or if any logic depends on the fact that `tokens_in_range` returns a slice.

I can explore this ideas once the code compiles.

---

_Merged by @dhruvmanila on 2024-05-28 04:53_

---

_Closed by @dhruvmanila on 2024-05-28 04:53_

---

_Branch deleted on 2024-05-28 04:53_

---

_@MichaReiser reviewed on 2024-05-28 07:23_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lib.rs`:420 on 2024-05-28 07:23_

> Why though?

Sorry, I phrased this poorly. The `tokens_in_range` is a good idea. I'm not sure if my proposal of not using a binary search is a good idea.



---
