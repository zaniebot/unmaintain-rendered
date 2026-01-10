```yaml
number: 11170
title: Make ImportFrom level just a u32
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - great writeup
assignees: []
merged: true
base: main
head: cjm/import-level-2
created_at: 2024-04-27T02:09:10Z
updated_at: 2024-04-29T09:01:12Z
url: https://github.com/astral-sh/ruff/pull/11170
synced_at: 2026-01-10T22:37:02Z
```

# Make ImportFrom level just a u32

---

_Pull request opened by @carljm on 2024-04-27 02:09_

## Summary

Currently we represent the `level` of an `ImportFrom` (the leading dots) as an `Option<u32>`, but the parser always parses a non-relative import (`from foo import bar`) as having `level: Some(0)`. But various other representations of imports throughout the codebase also use `Option<u32>`, and inconsistently sometimes use `Some(0)` and sometimes use `None` to represent the same situation: an absolute import. And then there's tons of code checking for absolute vs relative import that jumps through various hoops to handle `None` and `Some(0)` the same way. And of course it's easy to get this wrong -- in particular, it would be easy to check for `None` and miss the `Some(0)` case.

There is no useful distinction between what `None` and `Some(0)` represent here; they are exactly the same import. It's just two different inconsistent representations for the same thing.

This PR changes our representation of `level` everywhere to be just `u32`, so an absolute import always has level `0`, and simplifying a bunch of code that checks `level`. This is also consistent with CPython AST, which uses `level=0`, not `level=None`.

It would be possible to go the other way and try to eliminate the use of `Some(0)` and always use `None`, but it will be harder to stay consistent with that, since `Some(0)` will always be possible. And since it's possible, every usage site should technically still check for it.

`Option<u32>` is twice as big as `u32`, so this change saves a bit of memory, too. And some CPU instructions, from all the usage sites doing various forms of double checks for `None` and `0`.

## Test Plan

Compiles and test suite passes. Updated some `insta` tests.


---

_Label `internal` added by @carljm on 2024-04-27 02:09_

---

_Review requested from @MichaReiser by @carljm on 2024-04-27 02:09_

---

_Review requested from @dhruvmanila by @carljm on 2024-04-27 02:09_

---

_@charliermarsh approved on 2024-04-27 02:11_

Excellent, thank you.

---

_Label `great writeup` added by @zanieb on 2024-04-27 02:14_

---

_Comment by @charliermarsh on 2024-04-27 02:14_

(This has bothered me for a while.)

---

_Comment by @carljm on 2024-04-27 02:15_

Oh, looks like I need to learn to run doc-tests locally, too.

---

_Comment by @charliermarsh on 2024-04-27 02:22_

By the way, have you seen [`NonZeroU32`](https://doc.rust-lang.org/std/num/type.NonZeroU32.html)? I think that would've been another option here, since `Option<NonZeroU32>` is the same size as `u32`. But I think what you have here is simpler.

---

_Comment by @carljm on 2024-04-27 02:24_

Oh yeah, `Option<NonZeroU32>` would be a really good option! Honestly I could see an argument for that over this, since it makes the special case (not relative) more clearly different. And it eliminates the problem of two different representations, and the efficiency argument.

Now I just have to decide if I like that enough better to do the work to switch...

---

_Comment by @carljm on 2024-04-27 02:25_

Going to leave this unmerged for a bit while I think about that :)

---

_Comment by @carljm on 2024-04-27 02:32_

Nah I think I'll just merge this. `u32` is nice and simple, everything is slightly less verbose this way, and `0` is already special enough. I don't see any really convincing advantage for `Option<NonZeroU32>`. But that's something I'll definitely keep in mind in the future.

---

_Comment by @github-actions[bot] on 2024-04-27 02:37_

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

_Comment by @charliermarsh on 2024-04-27 02:37_

Agreed, this looks good to me. But `NonZeroU32` is useful sometimes and thought you'd find it interesting :)

---

_Merged by @carljm on 2024-04-27 02:38_

---

_Closed by @carljm on 2024-04-27 02:38_

---

_Branch deleted on 2024-04-27 02:38_

---

_Comment by @MichaReiser on 2024-04-27 06:19_

I definitely prefer this over Option<NonZeroUsize>. The thing that bothered me most is that the AST allows for both level and module to be None which should never happen in practice. The new design doesn't prevent this from happening, but at least it's no longer required to handle the case explicitly (you can test if module is set and otherwise take the default case)

---

_Comment by @dhruvmanila on 2024-04-29 08:03_

> The thing that bothered me most is that the AST allows for both level and module to be None which should never happen in practice.

This could be useful for error recovery to represent `from import y`, but of course that depends on the strategy we decide to use for it in the future. This looks good to me for now.

---

_Comment by @MichaReiser on 2024-04-29 09:01_

> This could be useful for error recovery to represent from import y, but of course that depends on the strategy we decide to use for it in the future. This looks good to me for now.

We can represent this as `level = 0` and `module = None`. However, I need to double-check the rune code to see if we support this without blowing up. 

---
