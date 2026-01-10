```yaml
number: 15335
title: "[red-knot] Reduce `Name` clones in call signature checking"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/call-nits
created_at: 2025-01-07T23:54:43Z
updated_at: 2025-01-08T18:29:37Z
url: https://github.com/astral-sh/ruff/pull/15335
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] Reduce `Name` clones in call signature checking

---

_Pull request opened by @AlexWaygood on 2025-01-07 23:54_

## Summary

This gets rid of several `Name` clones in our implementation of call-signature checking, at the cost of quite a few more lifetime annotations. There may be a simpler way of doing this, but I couldn't immediately see it... using `'db` for everything did not work.

Not sure if this is the way to go, but figured I'd put it up for discussion as a proof-of-concept anyway

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-01-07 23:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-07 23:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-07 23:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-07 23:54_

---

_Comment by @github-actions[bot] on 2025-01-08 00:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2025-01-08 00:36_

I had also played with trying to use the `'db` lifetime for this. I think it would work if we explicitly threaded that lifetime through every reference to an AST node throughout the type inference builder (i.e. replace every `&ast::Whatever` with `&'db ast::Whatever` in all inference method signatures). But the reason _that_ won't work is stringified annotations. Sometimes we infer types from an AST that isn't actually owned by the db at all, it's owned locally by the `infer_string_annotation_expression` method. I don't see a way around that without totally reworking how we handle stringified annotations, which is a can of worms I'd rather not reopen.

Using a distinct lifetime (as in this PR) just means that the `CallArguments`, `CallBinding`, and `CallOutcome` all ultimately borrow from the AST that the `CallArguments` were created from (or whatever lifetime the `CallArguments` borrows its keyword names from.) I _think_ this should be OK; generally `CallBinding` and `CallOutcome` should be ephemeral and not need to outlive the arguments.

So I'm open to this.

But it does make the code harder to read. And CodSpeed doesn't suggest any detectable performance improvement. If we can't detect a performance improvement, I'm not sure it makes sense to do this? We can spend complexity for perf, but IMO we should spend that budget where it actually wins us detectable perf. We could check whether this makes a noticeable difference in local hyperfine benchmarking (rather than CodSpeed), and on larger codebases (e.g. Black)?

Interested in others' thoughts.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:244 on 2025-01-08 07:11_

I'd use a `Name` here as constructing an error should be the exception. That allows removing the lifetime from `CallBindingError`, `CallBinding`, `CallDunderResult`, significantly reducing the size of the diff.

---

_@MichaReiser reviewed on 2025-01-08 07:15_

I'm leaning towards not making this change because the performance impact seems minimal and the lifetime does introduce a fair amount of complexity (it also means that we'll never be able to cache call arguments but maybe that's fine). 

But we may want to undo the changes to `CallBindingError` and reevaluate the diff then. 

I overall suspect that the cost of checking call arguments (infering every argument type, creating the `CallArguments` struct with a vector etc simpliy outweights the cost of cloning the name (which is stack allocated for most names )

---

_@AlexWaygood reviewed on 2025-01-08 18:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:244 on 2025-01-08 18:20_

Nice, thanks! It's much simpler now

---

_@carljm approved on 2025-01-08 18:27_

This looks fine to me now.

---

_Merged by @AlexWaygood on 2025-01-08 18:29_

---

_Closed by @AlexWaygood on 2025-01-08 18:29_

---

_Branch deleted on 2025-01-08 18:29_

---
