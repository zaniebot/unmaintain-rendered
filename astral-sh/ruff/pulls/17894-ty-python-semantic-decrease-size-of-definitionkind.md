```yaml
number: 17894
title: "ty_python_semantic: decrease size of `DefinitionKind`"
type: pull_request
state: closed
author: BurntSushi
labels:
  - ty
assignees: []
base: main
head: ag/memoryopt-slimmer-definition-kind
created_at: 2025-05-06T17:40:43Z
updated_at: 2025-05-07T13:14:43Z
url: https://github.com/astral-sh/ruff/pull/17894
synced_at: 2026-01-10T18:57:03Z
```

# ty_python_semantic: decrease size of `DefinitionKind`

---

_Pull request opened by @BurntSushi on 2025-05-06 17:40_

This drops the size from 56 bytes to 24. I made this change on a theory
that this would decrease overall memory usage, since a `DefinitionKind`
seems to be used quite a bit. The theory was that if most values of
this type aren't one of the bigger variants, then most values will end
up using a lot of wasted space.

This didn't seem to end up decreasing overall memory usage in the
tests that I had concocted. However this was kind of an annoying
change, and I don't think there are any downsides as far as I can tell.
Moreover, I think this change is mostly just applying the existing
encapsulation pattern a bit more consistently. And makes potential
future representation changes hopefully a little easier.

If this ended up being proven fruitful, then I would have pursued this
a bit more and decreased its size even more. It would be too hard to
get it down to 16 bytes. But given that I don't have any evidence that
this was actually beneficial, I stopped at this point.


---

_Review requested from @carljm by @BurntSushi on 2025-05-06 17:40_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-06 17:40_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-06 17:40_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-06 17:40_

---

_Label `ty` added by @AlexWaygood on 2025-05-06 17:41_

---

_Renamed from "red_knot_python_semantic: decrease size of `DefinitionKind`" to "ty_python_semantic: decrease size of `DefinitionKind`" by @AlexWaygood on 2025-05-06 17:41_

---

_Comment by @AlexWaygood on 2025-05-06 17:53_

Codspeed reports a 1% speedup on one of our benchmarks (which are generally very stable)! https://codspeed.io/astral-sh/ruff/branches/ag%2Fmemoryopt-slimmer-definition-kind

---

_Comment by @github-actions[bot] on 2025-05-06 18:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @MichaReiser on 2025-05-06 18:28_

The change makes sense to me but I'm inclined not to merge it because:

* We create a ton of `Definition`s and we now pay an extra allocation for many of them
* heap allocations aren't free (the allocator has to do some bookkeeping and may round the allocation to a multiple)

What we could do instead is to make use of salsa's new `supertype` feature:

* Make `Definition` an `enum`, derive `salsa::Supertype
* Create specific `Definition` structs for each `kind`. 

This avoids the extra allocation and salsa will store each definition-kind in its own page (salsa arena allocates tracked structs)


But I'm not sure if making this change is worth your time right now.

---

_Comment by @BurntSushi on 2025-05-06 18:41_

In my own ad hoc benchmarking, I wasn't able to measure a perf regression here. So I'm not sure the extra allocs actually matter. But... I suppose this entire PR is itself tilting at windmills, so I'm on pretty flimsy ground here. Also, the extra allocs can, perhaps counter-intuitively, put less pressure on the allocator overall. I didn't do the data analysis to prove this though.

> This avoids the extra allocation and salsa will store each definition-kind in its own page (salsa arena allocates tracked structs)

Not knowing what a Salsa supertype is, I found [this](https://github.com/salsa-rs/salsa/issues/578). I think that makes sense.

> But I'm not sure if making this change is worth your time right now.

Yeah I just wanted to put this PR up since it was done. But I'll probably back off of this if we don't want to bring it in approximately as-is.

---

_Comment by @MichaReiser on 2025-05-07 08:01_

> In my own ad hoc benchmarking, I wasn't able to measure a perf regression here. So I'm not sure the extra allocs actually matter. But... I suppose this entire PR is itself tilting at windmills, so I'm on pretty flimsy ground here. Also, the extra allocs can, perhaps counter-intuitively, put less pressure on the allocator overall. I didn't do the data analysis to prove this though.

I'm not opposed to merging, but the wins aren't as clear to me, and it also leads to more code overall. That's why I'm leaning towards leaving this as is for now. I think this could become more valuable if we'll need to store more data to support garbage collecting `AstNodeRef` (so that we could rematerialize an `AstNodeRef` after the Ast has been collected) and this would then lead to clearer wins.

> Not knowing what a Salsa supertype is, I found https://github.com/salsa-rs/salsa/issues/578. I think that makes sense.

Ironically, this could also lead to more memory usage, at least when measured on small projects because salsa allocates entire pages (currently of 1000 items). The result could be that we end up allocating N pages with a 1000 items (where `N` = number of different kinds) which could be more than allocating `definitions/1000` pages. I would expect this to even out for large programs but it probably means that it's very hard to measure any improvement you make. 

It's also worth noting that salsa tracks additional metadata for each tracked struct, adding to their total size https://github.com/MichaReiser/salsa/blob/e7899c7293b740ef82ccc1dd8ecd60a728e3ebde/src/tracked_struct.rs#L267-L314. Storing a `bool` as a tracked struct takes, in total, 40 bytes. 



---

_Comment by @BurntSushi on 2025-05-07 13:14_

All righty, makes sense. I would personally be in favor of merging because I think it's better to err on the side of encapsulation, but I don't feel strongly enough in this case to champion it.

---

_Closed by @BurntSushi on 2025-05-07 13:14_

---

_Branch deleted on 2025-05-07 13:14_

---
