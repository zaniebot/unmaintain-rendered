```yaml
number: 21312
title: "[ty] name is parameter and global is a syntax error"
type: pull_request
state: merged
author: 11happy
labels:
  - ty
assignees: []
merged: true
base: main
head: ty_bound_parameter
created_at: 2025-11-07T06:08:43Z
updated_at: 2025-11-14T18:15:34Z
url: https://github.com/astral-sh/ruff/pull/21312
synced_at: 2026-01-10T16:53:55Z
```

# [ty] name is parameter and global is a syntax error

---

_Pull request opened by @11happy on 2025-11-07 06:08_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR implements `is_bound_parameter` for ty for syntax error name is parameter and global, this is a follow up to this https://github.com/astral-sh/ruff/pull/20426

## Test Plan

<!-- How was it tested? -->
mdtest and snapshots

## CC
- @ntBre 

---

_Review requested from @carljm by @11happy on 2025-11-07 06:08_

---

_Review requested from @AlexWaygood by @11happy on 2025-11-07 06:08_

---

_Review requested from @sharkdp by @11happy on 2025-11-07 06:08_

---

_Review requested from @dcreager by @11happy on 2025-11-07 06:08_

---

_Comment by @github-actions[bot] on 2025-11-07 06:11_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-11-07 06:12_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `ty` added by @AlexWaygood on 2025-11-07 12:54_

---

_Renamed from "[ty]: name is parameter and global is a syntax error" to "[ty] name is parameter and global is a syntax error" by @MichaReiser on 2025-11-09 15:29_

---

_@MichaReiser had review dismissed on 2025-11-10 12:30_

It seems some mdtests are failing. Would you mind taking a look at why they're failing?

---

_Comment by @11happy on 2025-11-10 13:41_

```
crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md:386 unexpected error: 12 [unresolved-global] "Invalid global declaration of `a`: `a` has no declarations or bindings in the global scope"

crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md:386 unexpected error: 12 [invalid-syntax] "name `a` is used prior to global declaration"
```

actually two more errors are also emitted along with the expected one , can we supress others ? as the changes in this PR are not related to other two errors , 

---

_Comment by @ntBre on 2025-11-10 13:54_

For the first error, I think you can just add a global `a` binding before the function definition. The second case looks interesting, I think it's basically a conflicting error with the error you're adding in this PR. I think we might want to suppress the `used prior to global declaration` error in this case so that we only report the more specific `parameter and global` error.

I checked how we handle this in [ruff](https://play.ruff.rs/b0d433c7-568d-48f2-8288-0ff9ef72c0b1) too, and it looks like we only emit the `parameter and global` syntax error, even if [PLE0118](https://docs.astral.sh/ruff/rules/load-before-global-declaration/) is also enabled.

---

_Comment by @11happy on 2025-11-12 01:47_

> For the first error, I think you can just add a global `a` binding before the function definition. The second case looks interesting, I think it's basically a conflicting error with the error you're adding in this PR. I think we might want to suppress the `used prior to global declaration` error in this case so that we only report the more specific `parameter and global` error.
> 
> I checked how we handle this in [ruff](https://play.ruff.rs/b0d433c7-568d-48f2-8288-0ff9ef72c0b1) too, and it looks like we only emit the `parameter and global` syntax error, even if [PLE0118](https://docs.astral.sh/ruff/rules/load-before-global-declaration/) is also enabled.

what do you think of this approach I have added a new symbol flag _IS_PARAMETER ? is working as expected now 


---

_Review request for @dcreager removed by @MichaReiser on 2025-11-13 08:28_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-13 08:28_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-13 08:28_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-13 08:28_

---

_Comment by @ntBre on 2025-11-13 21:20_

Nice! This does seem to work. My only hesitation is that I suspect there is an existing way to obtain this information without adding adding a new `SymbolFlags` variant. I poked around a bit and didn't find anything, but it's probably worth someone on the ty team taking a look if they get the chance.

Maybe @AlexWaygood and I can have a look tomorrow during our meeting :)

---

_Comment by @carljm on 2025-11-13 23:27_

FWIW I think it's probably _possible_ to do it another way, but this way looks pretty clean and straightforward, and IMO is very much in line with the intended use of `SymbolFlags`, so no objection from me.

---

_Review requested from @dhruvmanila by @AlexWaygood on 2025-11-14 17:46_

---

_@AlexWaygood approved on 2025-11-14 17:46_

thank you!

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 18:14_


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

_Merged by @AlexWaygood on 2025-11-14 18:15_

---

_Closed by @AlexWaygood on 2025-11-14 18:15_

---
