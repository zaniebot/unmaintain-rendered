```yaml
number: 15702
title: "[red-knot] Use itertools to clean up `SymbolState::merge`"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/merge-cleanup
created_at: 2025-01-23T21:58:07Z
updated_at: 2025-01-24T21:24:26Z
url: https://github.com/astral-sh/ruff/pull/15702
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Use itertools to clean up `SymbolState::merge`

---

_@dcreager_

[`merge_join_by`](https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.merge_join_by) handles the "merge two sorted iterators" bit, and `zip` handles iterating through the bindings/definitions along with their associated constraints.


---

_Label `internal` added by @dcreager on 2025-01-23 21:58_

---

_Label `red-knot` added by @dcreager on 2025-01-23 21:58_

---

_Review requested from @carljm by @dcreager on 2025-01-23 21:58_

---

_Review requested from @MichaReiser by @dcreager on 2025-01-23 21:58_

---

_Review requested from @AlexWaygood by @dcreager on 2025-01-23 21:58_

---

_Review requested from @sharkdp by @dcreager on 2025-01-23 21:58_

---

_Comment by @carljm on 2025-01-23 22:00_

I think you need @MichaReiser review on this PR :) Both because he knows Rust a lot better than I do, and because I'm aware that he has an aversion to itertools, because it has iterators that silently allocate, and arguably this is bad because allocations should be more visible than that.

---

_Comment by @carljm on 2025-01-23 23:22_

Thanks for this!

This definitely looks simpler and easier to understand; having the "merge-join-by" logic extracted as a utility is nice to separate it from the red-knot-specific logic. I'm also not sure how much to weight that here, since this is core infra that shouldn't have to change too often; eking out the best performance we can might be higher priority.

On that note, CodSpeed suggests that this is a 2% regression on "cold" check (which is where I'd expect semantic indexing cost to be relevant, as opposed to "incremental" check where Salsa validation costs usually dominate): https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fmerge-cleanup

I'm not sure where that regression would be coming from, exactly; CodSpeed flame graph suggests it is all coming from a salsa ingredient `get_or_create` method, which doesn't make a lot of sense, so maybe it's just noise? It does seem like there would be some loss of efficiency here in that we can no longer reuse one of the declarations bitsets (via union-in-place), but instead we start from zero and add all declarations? That maybe isn't likely to make much difference?

Like I said above, I'd like to hear what @MichaReiser thinks.

---

_Comment by @carljm on 2025-01-23 23:23_

Might be worth running that cold benchmark a few times locally with and without this change, just to see if we can get a sense of how stable vs noisy that regression is?

---

_Comment by @dcreager on 2025-01-24 00:22_


> Might be worth running that cold benchmark a few times locally with and without this change, just to see if we can get a sense of how stable vs noisy that regression is?

Can do

> we can no longer reuse one of the declarations bitsets (via union-in-place)

I can also try a version that adds back the union in-place.  The union itself was OR-ing block by block, instead of bit by bit, and also had a reserve step to make sure there was at most 1 (re)allocation.  Interestingly, we were using union for `SymbolDeclarations`, but not for `SymbolBindings`!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:187 on 2025-01-24 07:37_

I'd use the regular `Iterator::zip` method here because it's better known and we don't use the functionality that `izip` provides

> This is a version of the standard .zip() that’s supporting more than two iterators. 
```suggestion
        let a = a.live_declarations.iter().zip(a.visibility_constraints);
        let b = b
            .live_declarations
            .iter()
            .zip(b.visibility_constraints.into_iter());
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:281 on 2025-01-24 07:38_

```suggestion
        let mut a = std::mem::take(self);
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:207 on 2025-01-24 07:44_

Should we inline this function because there's nothing that actually guarantees the constraint of the safety comment -- the function can be called from anywhere in this module. 



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:177 on 2025-01-24 07:44_

```suggestion
        let mut a = std::mem::take(self);
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:293 on 2025-01-24 07:45_

Nit: Use regular `zip`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:337 on 2025-01-24 07:46_

I think it would be nice to still preserve some of those comments. E.g .this comment could be a great comment above `merge_join_by`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:352 on 2025-01-24 07:47_

Do we still need this safety documentation?

---

_@MichaReiser approved on 2025-01-24 07:48_

This looks great. `merge_join_by` doesn't allocate internally. So I'm fine with this `itertools` usage. I do recommend using the regular `zip` over `izip` because we don't need its functionality and `zip` is better known (I had to read `izip`'s documentation)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:187 on 2025-01-24 14:57_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:293 on 2025-01-24 14:57_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:207 on 2025-01-24 14:58_

Good idea, done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:337 on 2025-01-24 15:09_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:352 on 2025-01-24 15:10_

I reworded it a bit and put it up at the definition of the fields in the struct

---

_@dcreager reviewed on 2025-01-24 15:10_

---

_Merged by @dcreager on 2025-01-24 15:21_

---

_Closed by @dcreager on 2025-01-24 15:21_

---

_Branch deleted on 2025-01-24 15:21_

---

_Comment by @carljm on 2025-01-24 15:24_

The latest version here still showed as a 1% regression in cold benchmark on codspeed. Were you able to reproduce that regression locally?

---

_Comment by @dcreager on 2025-01-24 18:33_

> The latest version here still showed as a 1% regression in cold benchmark on codspeed. Were you able to reproduce that regression locally?

Yes — it was a 2% regression according to `cargo bench` before I went back to using `union` to combine the bitsets.  Now it's 1%, as reported by both `cargo bench` and `hyperfine` when running against black:

```
$ ./go
Benchmark 1: ./red_knot_main --project /home/dcreager/git/code-nav/black --venv-path .venv --extra-search-path src
  Time (mean ± σ):     209.2 ms ±  15.7 ms    [User: 873.3 ms, System: 252.1 ms]
  Range (min … max):   185.3 ms … 240.4 ms    15 runs

  Warning: Ignoring non-zero exit code.

Benchmark 2: ./red_knot_feature --project /home/dcreager/git/code-nav/black --venv-path .venv --extra-search-path src
  Time (mean ± σ):     211.6 ms ±  15.0 ms    [User: 899.7 ms, System: 254.9 ms]
  Range (min … max):   185.9 ms … 243.5 ms    15 runs

  Warning: Ignoring non-zero exit code.

Summary
  ./red_knot_main --project /home/dcreager/git/code-nav/black --venv-path .venv --extra-search-path src ran
    1.01 ± 0.10 times faster than ./red_knot_feature --project /home/dcreager/git/code-nav/black --venv-path .venv --extra-search-path src
```

I felt like that was small enough to be worth it, relative to the code being easier to understand.  I can revert if folks feel strongly otherwise.

---

_Comment by @carljm on 2025-01-24 19:23_

I do think the new version is nicer to read, but personally I'm not sure that it is worth 1% overall regression in cold-check performance, if that regression is real and stable. I think that's a significant regression to accept in exchange for a less concrete benefit. I don't think this SymbolState merging code will require frequent changes, and it is a very hot code path, so I think it is OK to accept more complex code here if it performs better.

Maybe there would be some way to claw back the regression in the new version, if we can identify the source of it? But I also don't know how much time we want to devote to that investigation.

---

_@MichaReiser reviewed on 2025-01-24 20:02_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:302 on 2025-01-24 20:02_

Oh, I wasn't aware that we're zipping three iterators here. It could make sense to use `izip` here ;)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:302 on 2025-01-24 20:17_

With the extra set of parentheses around the first iterator, I don't mind how this looks, and tbh that was the only reason I reached for `izip` in the first place!  (I never like it when `rustfmt` takes `A.zip(B).zip(C)` and breaks up the `A` across multiple lines...)

---

_@dcreager reviewed on 2025-01-24 20:21_

> Maybe there would be some way to claw back the regression in the new version, if we can identify the source of it? But I also don't know how much time we want to devote to that investigation.

I am cautiously optimistic about https://github.com/astral-sh/ruff/pull/15731

---

_Comment by @sharkdp on 2025-01-24 21:24_

I'm late to the party, but wanted to say that I talked about this function with Micha in our 1:1 some weeks ago, and we both agreed that it should be refactored. We then proceeded by doing nothing. So thanks for taking this up @dcreager!

---
