```yaml
number: 12706
title: "[red-knot] type narrowing"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/narrow
created_at: 2024-08-06T04:54:57Z
updated_at: 2024-08-16T23:34:15Z
url: https://github.com/astral-sh/ruff/pull/12706
synced_at: 2026-01-10T21:38:31Z
```

# [red-knot] type narrowing

---

_Pull request opened by @carljm on 2024-08-06 04:54_

Extend the `UseDefMap` to also track which constraints (provided by e.g. `if` tests) apply to each visible definition.

Uses a custom `BitSet` and `BitSetArray` to track which constraints apply to which definitions, while keeping data inline as much as possible.

---

_Label `red-knot` added by @carljm on 2024-08-06 04:54_

---

_Renamed from "[red-knot] [WIP] initial narrowing work" to "[red-knot] [WIP] type narrowing" by @carljm on 2024-08-08 02:40_

---

_Comment by @codspeed-hq[bot] on 2024-08-08 02:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/narrow)

### Merging #12706 will **degrade performances by 13.78%**

<sub>Comparing <code>cjm/narrow</code> (15885c6) with <code>main</code> (a9847af)</sub>



### Summary

`❌ 2 (👁 2)` regressions
`✅ 30` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/narrow` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[cold]` | 51 ms | 59.2 ms | -13.78% |
| 👁 | `red_knot_check_file[incremental]` | 2.7 ms | 3 ms | -9.39% |


---

_Renamed from "[red-knot] [WIP] type narrowing" to "[red-knot] type narrowing" by @carljm on 2024-08-15 01:59_

---

_Comment by @github-actions[bot] on 2024-08-15 01:59_

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

_Marked ready for review by @carljm on 2024-08-15 03:25_

---

_Review requested from @MichaReiser by @carljm on 2024-08-15 03:25_

---

_Review requested from @AlexWaygood by @carljm on 2024-08-15 03:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:383 on 2024-08-15 06:23_

Collecting here only to receive the only public definition seems a bit wasteful and is also kind of hard to read. I suggest to add a helper method to `use_def` that returns an `Option`:

```rust
`use_def.single_public_definition(..) -> Option<T>
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:427 on 2024-08-15 06:24_

Using a helpr method also makes this more readable because you don't have to use destructuring
```suggestion
        let definition = use_def
            .single_public_definition(
                global_table
                    .symbol_id_by_name("foo")
                    .expect("symbol to exist"),
            )
            .expect("one definition)
            .definition;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:170 on 2024-08-15 06:26_

You can use the `smallvec` crate to get a stack allocated array up to N elements. You may still want to wrap it to get the specific API you want

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:13 on 2024-08-15 06:28_

Is the motivation for using a `Set` over a `Vec` because the bitset is likely to dense? 

I wonder if it is worth the complexity. Using a `Vec<u64>` could remove a lot of branching in your code. You could even consider using the smallvec crate to get the inline vs heap allocated representation.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:31 on 2024-08-15 06:33_

The assertion above should be an `assert` if we use it to *assert* the correctness of the program. It should also not cost that much. 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:32 on 2024-08-15 06:40_

Nit: I would rename this to `INLINE_CAPACITY` because the `HEAP` variant can have more than that number of bits. Or maybe `INLINE_MAX`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:27 on 2024-08-15 06:49_

One problem of this implementation is that it assumes that `usize` >= `u32` which isn't given on all platforms. That means, some of those clippy warnings are indeed issues. I'm even surprised why clippy doesn't complain about casting `value` to an `usize` because that could also truncate.

It may be worth adding some `assert` to ensure we aren't truncating values.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:58 on 2024-08-15 06:50_

Is this a python convention to use underscore? I don't think I've seen this in any rust code before
```suggestion
                let missing = blocks[block] & (1u64 << index) == 0;
                blocks[block] |= 1u64 << index;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:92 on 2024-08-15 06:53_

Isn't this implementing `union` instead of intersect? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:117 on 2024-08-15 06:53_

 This seems more the `next_block_index` than the current because we increment it as soon as we move to the next block. It is then one index ahead of where `cur_block` points to

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:120 on 2024-08-15 06:54_

```suggestion
        current_block: u64,
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:130 on 2024-08-15 06:59_

I find the name value hard to understand. Isn't it the index inside the block? Which is also why need to do some more math before returning it

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:144 on 2024-08-15 06:59_

Nit
```suggestion
								*cur_block ^= 1 << value;
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:183 on 2024-08-15 07:02_

I think that matches what in Rust commonly is named `with_capacity`

```suggestion
    pub(super) fn with_capacity(size: usize) -> Self {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:176 on 2024-08-15 07:04_

I'm not sure if the compiler can see through this. Might be worth to have two extra branches
```suggestion
				if size <= N {
					Self::default()
				} else {
					Self::Heap(Vec::with_capacity(size))
				}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:182 on 2024-08-15 07:04_

Can we pre-allocate the vec with the right size?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:244 on 2024-08-15 07:07_

Could we slice the array instead and directly return an `std::slice::Iter`?
```suggestion
                &array[..*size],
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:57 on 2024-08-15 07:16_

Could you let me know how you determined this number? We could probably go higher than this because the size of a `BTreeSet` is 24 bytes. This means we should use at least two blocks to make the best use of the stack memory. Whether we want to use 3 depends if Rust is able to use a niche to represent the `BitSet` enum as 24 bytes if the array is small enough. If not, then we should use 3 blocks at least.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:64 on 2024-08-15 07:16_

Same here, we probably want to use a number higher than 1

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:66 on 2024-08-15 07:17_

What does it mean: It can handle that many visible definitions per symbol at a given time. What happens if there are more?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:157 on 2024-08-15 07:21_

Nit: I would use `expect` here instead of unreachable.
```suggestion
            // SAFETY: we only ever create SymbolState with either no definitions and no
            // constraint bitsets (`::unbound`) or one definition and one constraint bitset
            // (`::with`), and `::merge` always pushes one definition and one constraint bitset
            // together (just below), so the number of definitions and the number of constraint
            // bitsets can never get out of sync.
            let constraints = constraints_iter.next().expect("definitions and constraints length mismatch");
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:124 on 2024-08-15 07:28_

I'm unsure if we can make use of this but:

We should change `builder.flow_merge` to take an owned snapshot. I didn't had to insert a single `snapshot.clone`. This could help us to possibly avoid some `clone` calls because it seems we can consume the value.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:180 on 2024-08-15 07:33_

Same as above
```suggestion
                        let b_constraints = b_constraints_iter.next().expect("definitions and constraints length mismatch");
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:210 on 2024-08-15 07:36_

```suggestion
    pub(super) fn visible_definitions(&self) -> DefinitionIdWithConstraintsIterator {
```

The `iter` prefix is less common when iterating named parts of a struct as we do here. Just `visible_definitions` seems enough. The return type already specifies that it is an iterator.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:251 on 2024-08-15 07:40_

We could avoid for `from_u32` by making `BitSet` generic over `T` where `T: Into<u32> + From<u32>`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:256 on 2024-08-15 07:40_

It's unclear where `above` references to

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:117 on 2024-08-15 07:42_

Not sure if this method shows up in the profiles but the `IntersectionBuilder` now always allocates an `FxHashSet` even for definitions with no constraints.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:180 on 2024-08-15 07:46_

Nit: I prefer to call this `inner` and I think I saw this pattern in some other rust code

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def.rs`:211 on 2024-08-15 07:46_

Implement `FusedIterator` (yes, it's annoying)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:12 on 2024-08-15 07:48_

I don't understand this comment

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:23 on 2024-08-15 07:49_

You want `return_ref` here or salsa clones the value everytime the query is called
```suggestion
#[salsa::tracked(return_ref)]
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:49 on 2024-08-15 07:50_

Lol. `node().node()`. This is a bit awkward (probably my doing)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:83 on 2024-08-15 07:52_

Are these operations we expect to call often? Would it make sense to query them once instead of going through salsa multiple times? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:89 on 2024-08-15 07:52_

```suggestion
            let symbol = self.symbols().symbol_id_by_name(id).unwrap();
            let inference = self.inference();
            let scope = self.scope();
            
            for (op, comparator) in std::iter::zip(ops.as_ref(), comparators.as_ref()) {
                let comp_ty = inference.expression_ty(comparator.scoped_ast_id(self.db, scope));
```



---

_@MichaReiser approved on 2024-08-15 07:54_

This is great work and the documentation is excellent. 

We may be able to remove quiet a bit of code from `BitSet` by using smallvec (if we want?) and we probably clone the narrowing constraints on every query run which we should avoid ;)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index.rs`:383 on 2024-08-15 14:53_

Yeah I'll add a helper for this; it's test-only though so I'll just implement it for tests I think.

---

_@carljm reviewed on 2024-08-15 14:53_

---

_@carljm reviewed on 2024-08-15 14:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:170 on 2024-08-15 14:54_

Yeah I knew about smallvec, not sure why I didn't use it for `BitSetArray`. I'll try that and see if it works; I've had lifetime-invariance problems before when I've tried using smallvec.

---

_@MichaReiser reviewed on 2024-08-15 14:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:383 on 2024-08-15 14:58_

Whoops. I didn't notice that this is all in tests. We need a testing framework 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:170 on 2024-08-15 14:59_

Yeah, you can run into lifetime errors with smallvec because the variance is incorrect https://github.com/servo/rust-smallvec/issues/146 I'm desperately waiting for small vec 2

---

_@MichaReiser reviewed on 2024-08-15 14:59_

---

_@carljm reviewed on 2024-08-15 15:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:13 on 2024-08-15 15:07_

The semantics of the overall type are ordered-set semantics: a u32 should only appear in it once, and it should stay ordered. These semantics are natural for a bitset representation.

The current design here is that we use a stack-allocated bitset for small values, and fall back to a `BTreeSet` of the u32 values if we overflow the size of the stack-allocated bitset, because a `BTreeSet` has the same ordered-set semantics that we need.

It sounds like you are suggesting an alternative design where we just have a growable bitset instead, using `SmallVec<u64>` to just extend the bitset as far as needed. That sounds like it could also work, and would probably be a bit more efficient when we do need to overflow. I can give it a try.


---

_@carljm reviewed on 2024-08-15 15:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:32 on 2024-08-15 15:13_

The `Heap` variant in the current code is not a bitset at all, so I think the naming is good for the current code. But I think it probably should be a growable bitset instead, so I'll try making that change and address this as part of it.

---

_@carljm reviewed on 2024-08-15 15:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:27 on 2024-08-15 15:15_

I can add an `assert!(usize::BITS >= 32)` -- I assume we don't care about actually supporting any 16-bit platforms?

---

_@carljm reviewed on 2024-08-15 15:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:58 on 2024-08-15 15:16_

No, I think the underscore is what clippy/rustc suggested when giving me an error here and suggesting I should clarify the type of the `1` literal. Doesn't matter to me either way, I can remove it.

---

_@MichaReiser reviewed on 2024-08-15 15:16_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:13 on 2024-08-15 15:16_

Yeah. The basic idea is to use the same logic for both variants where we first find the block and then the index inside the block where the only difference is where the blocks are allocated.

---

_@carljm reviewed on 2024-08-15 16:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:92 on 2024-08-15 16:05_

Uh, yeah, good call -- I think I had a union implementation at some point and copy-pasted intersect from it, but apparently failed to update this mixed case, and don't have any tests for it. Will fix.

---

_@carljm reviewed on 2024-08-15 16:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:117 on 2024-08-15 16:07_

I don't think so; `cur_block_index` is always the index of `cur_block`. We initialize with `cur_block_index = 0` and `cur_block = blocks[0]`, and when we increment `cur_block_index` the very next line is `*cur_block = blocks[*cur_block_index]`.

---

_@carljm reviewed on 2024-08-15 16:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:130 on 2024-08-15 16:09_

Yeah, it's the low set bit in the current block, which is the next value to return only if the current block is block 0, otherwise we need to account for the bits in all the other blocks. I'll find a better name for it.

---

_@MichaReiser reviewed on 2024-08-15 16:09_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:58 on 2024-08-15 16:09_

Same. Up to you

---

_@MichaReiser reviewed on 2024-08-15 16:10_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:27 on 2024-08-15 16:10_

Probably not but Ruff supports a large range of targets

---

_@carljm reviewed on 2024-08-15 16:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:144 on 2024-08-15 16:11_

I might check how Rust compiles each of these; I thought the `wrapping_sub(1)` trick might be a bit more efficient. But it probably doesn't matter. I agree your version is easier to understand, which is why I added the comment :)

---

_@carljm reviewed on 2024-08-15 16:18_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:183 on 2024-08-15 16:18_

I think it's different; the "capacity" here is already fixed by the const generic parameter. This constructor is actually creating a non-empty array of `size` user-visible empty bitset values. The name `with_capacity` would wrongly suggest that it's creating an empty bitset array with a capacity to hold a certain number of bitsets without resizing.

---

_@carljm reviewed on 2024-08-15 16:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:176 on 2024-08-15 16:20_

This suggestion won't work as written for the reason described above: the semantics of this method are not `with_capacity` semantics. The first branch would also need to increment `self.size`, and the second branch needs to also actually push `size` elements onto the vec, after creating it.

But it still may be worth the separate branches here.

---

_@carljm reviewed on 2024-08-15 16:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:244 on 2024-08-15 16:22_

Good idea, will try that!

---

_@carljm reviewed on 2024-08-15 16:24_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:57 on 2024-08-15 16:24_

I think I initially had this set as 4, and found that 1 performed better, and 1-3 didn't show any meaningful difference. It's a good point though that it should be at least as big as the `BTreeSet` (though this factor may disappear if I go with your growable-bitset suggestion.)

---

_@carljm reviewed on 2024-08-15 16:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:64 on 2024-08-15 16:25_

I initially had a higher number and switched to 1 because it performed best. In this case I'm not sure the argument for using a bigger number is as strong, because I suspect an array of 1 `BitSet` is already as large as a `Vec`? But I'll double check both the sizes and the actual performance.

---

_@carljm reviewed on 2024-08-15 16:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:66 on 2024-08-15 16:27_

The key word "inline" is at the end of the sentence. I'll re-phrase to make it clearer.

What happens if there are more is that our `BitSetArray` of constraints-per-definition (where there is a `BitSet` per definition, and the constraints are individual bits in the `BitSet`) will overflow to the heap-allocated vector representation.

---

_@carljm reviewed on 2024-08-15 16:30_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:57 on 2024-08-15 16:30_

I think currently, given the factor I mentioned in our 1:1 (that the performance bottleneck in this PR is cloning the entire IndexVec of SymbolStates at every branch point), the dominating factor in performance is the size of that `IndexVec`, which means wasting as little memory here as possible seems to be more important than keeping the data always inline (though likely only to a point; I suspect it would be a regression if we always had to allocate everything as vectors-of-vectors.)

---

_@MichaReiser reviewed on 2024-08-15 16:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:57 on 2024-08-15 16:37_

Storing more inline can help with reducing the cost of cloning. 

> (though this factor may disappear if I go with your growable-bitset suggestion.)

An `IndexVec` has the same constraints except that it already uses a niche to determine the variant, in which case we would have to go with 2 (I don't expect it to help much because 64 is already a lot of values)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:256 on 2024-08-15 17:46_

It refers to the previous safety comment about this same assertion that definitions and constraints length should match. I can just repeat the comment instead.

---

_@carljm reviewed on 2024-08-15 17:46_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:117 on 2024-08-15 17:47_

I'll check the profile. Could easily special-case no-constraints here to avoid this.

---

_@carljm reviewed on 2024-08-15 17:47_

---

_@carljm reviewed on 2024-08-15 17:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:12 on 2024-08-15 17:50_

The expression `test` might (or might not) apply some constraint on `definition`. E.g. if the `definition` is of symbol `x`, then the `test` `x is not None` does apply a constraint on that definition, and we'd return the type `~None` here. But the `test` expression `y is not None` does not apply any constraint to the definition of `x`, so we'd return `None`.

I can add a fuller description/example like this to the doc comment.

---

_@carljm reviewed on 2024-08-15 17:53_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:49 on 2024-08-15 17:53_

I guess the field on `Expression` could be called `node_ref` instead of `node`, so this would be `.node_ref(self.db).node()` which is probably a bit clearer.

---

_@carljm reviewed on 2024-08-15 17:54_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:83 on 2024-08-15 17:54_

It would be really nice if we could think of calling a cached Salsa query as "free", but I guess we can't :/ Yeah, I'll cache these on the builder.

---

_@carljm reviewed on 2024-08-15 20:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:49 on 2024-08-15 20:15_

I thought maybe we could provide an `Expression::node` method that would return a reference to the actual expression, but that causes borrowing problems because then the caller thinks the lifetime of the `&ast::Expr` is tied to the entire `Expression`.

---

_@carljm reviewed on 2024-08-15 23:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:251 on 2024-08-15 23:57_

I tried this, but I wasn't convinced it was a net benefit. It introduces a _lot_ of extra noise in the BitSet implementation, in exchange for eliminating very little noise in `symbol_state.rs`. And in making the change I somehow introduced a bug; it didn't seem worth tracking it down, I just reverted the change instead.

---

_@carljm reviewed on 2024-08-16 00:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:124 on 2024-08-16 00:03_

I think I used to have this but I changed it to take a reference because clippy doesn't like it if it's passed by value but not consumed. And it doesn't seem like we can gain anything by consuming it; it isn't cloned anywhere in this method. We already reuse `self` in-place and merge the data from `snapshot` into it; we can't reuse both of them.

---

_@carljm reviewed on 2024-08-16 00:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:244 on 2024-08-16 00:16_

Great suggestion, this allowed totally removing `BitSetArrayIterator`.

---

_@carljm reviewed on 2024-08-16 01:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:83 on 2024-08-16 01:40_

On looking at this more, for a given `NarrowingConstraintsBuilder` we'd expect to usually call any of these zero or one times; if the constraint is particularly complex, maybe several times. I moved the calls outside the loop over comparators as you suggested below, but I don't think further caching of these is worth it. We could try it later once we have more kinds of constraints supported.

Unconditionally querying these on construction of a `NarrowingConstraintsBuilder` is like a 10-15% regression, because currently most of the time we query these zero times (because the constraint is not in the form `x is not y`, which is the only constraint form we support in this PR.)

---

_@carljm reviewed on 2024-08-16 02:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:64 on 2024-08-16 02:57_

`[BitSet<1>; 1]` is 32 bytes, `Vec<BitSet>` is 24 bytes, so there's no wasted memory in leaving this at 1. Empirically 1 and 2 perform the same on the benchmark, 3 is a small regression, 4 is a large regression, so I'll set this to 2 for now.

---

_@carljm reviewed on 2024-08-16 03:05_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:57 on 2024-08-16 03:05_

A `BitSet<1>` is 32 bytes, suggesting Rust isn't able to use a niche to differentiate the enum. And indeed, BitSets of size 1, 2, and 3 are all 32 bytes; 4 increases to 40 bytes.

Empirically I'm not able to detect any performance difference between 1, 2, 3, or 4. I'll leave it at 3 for now; 3 * 64 is quite a lot of definitions in one scope.

---

_@carljm reviewed on 2024-08-16 03:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:57 on 2024-08-16 03:06_

I'll revisit this after I implement the growable-bitset version.

---

_@carljm reviewed on 2024-08-16 03:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:176 on 2024-08-16 03:15_

The two explicit branches don't seem to affect benchmark performance detectably, but the code is still quite concise and I think it is likely the compiler doesn't see through the other version, so I'll leave the two branches.

---

_@carljm reviewed on 2024-08-16 04:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:144 on 2024-08-16 04:22_

Going to leave this; the `wrapping_sub(1)` way looks a bit more efficient; possibly most importantly it doesn't have a data dependence on storing the lowest set bit first, so it can allow a bit more parallel execution. I think with the comment it's sufficiently understandable.

---

_@carljm reviewed on 2024-08-16 05:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:23 on 2024-08-16 05:02_

Surprisingly (to me) this didn't make a detectable difference in the benchmarks.

---

_@carljm reviewed on 2024-08-16 05:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:23 on 2024-08-16 05:04_

I think perhaps that's just because in practice `NarrowingConstraints` will usually be quite small: either an empty `FxHashMap` or an `FxHashMap` with a single entry in it.

---

_@carljm reviewed on 2024-08-16 05:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:13 on 2024-08-16 05:48_

This was a good suggestion, thanks! Doesn't seem to have a significant impact on perf either way, but the code is simpler.

---

_@MichaReiser reviewed on 2024-08-16 05:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/narrow.rs`:23 on 2024-08-16 05:52_

Interesting. This is not for this PR, but it might be worth considering returning a `Vec` instead of a `HashSet` (or implementing our own `TinySet` that is based on `SmallVec`). Vecs are cheaper to create and can also be faster to search when they are only very few items. 

---

_@MichaReiser reviewed on 2024-08-16 05:54_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:13 on 2024-08-16 05:54_

Did you made the changes locally? Because I don't see them here yet :D

---

_@carljm reviewed on 2024-08-16 06:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:170 on 2024-08-16 06:06_

This is a great suggestion too; no lifetime errors, no effect on perf, but allows removing a lot of code.

---

_Comment by @carljm on 2024-08-16 06:13_

@MichaReiser Thanks for the excellent review! I think I've addressed all the comments. Lots of changes, so you may want to take another look.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:20 on 2024-08-16 06:15_

Nit: Could this also be a static assertion?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:57 on 2024-08-16 06:16_

Nit [naming-conventions](https://rust-lang.github.io/api-guidelines/naming.html#getter-names-follow-rust-convention-c-getter)

```suggestion
    fn blocks(&self) -> &[u64] {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:45 on 2024-08-16 06:17_

Nit: I guess the method now does slightly more than just "overflowing". Maybe rename to `reserve` or `resize`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:124 on 2024-08-16 06:19_

I do think we could make use of it but it would probably complicate the logic a bit because you need two versions of `push` and a `into_iter` version for `constraints` 

For example, couldn't we avoid cloning the constraints and instead take them right from the symbol state?

---

_@MichaReiser approved on 2024-08-16 06:21_

I only skimmed through. Thanks for addressing all the feedback and we're down to a 13% perf regression :)

---

_@carljm reviewed on 2024-08-16 22:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/bitset.rs`:20 on 2024-08-16 22:15_

No, it can't, I tried; using const generic parameters in const expressions is not supported (or at least not in stable) yet.

---

_@carljm reviewed on 2024-08-16 23:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/use_def/symbol_state.rs`:124 on 2024-08-16 23:14_

Ok, yeah, I was too tired yesterday after going through all the other comments to push this all the way through :) You're right, I can eliminate the `constraints.clone()` this way; I don't even need two variants of `push` because I can use `into_iter` for iterating both incoming constraint sets.

Sadly this doesn't seem to move the needle at all on the benchmarks, but it makes sense to do it this way so I'll leave it.

Going to go ahead and merge this, but feel free to take a look at the updated `SymbolState::merge` and `UseDefMapBuilder::merge` and tell me if you see anywhere else I'm leaving potential perf on the floor. Thanks!

---

_Comment by @carljm on 2024-08-16 23:28_

Hmm, I made a reply comment about the "taking ownership in merge" thing but now I don't see it in the UI anywhere. Anyway the upshot was that you're right, I was able to do that; it didn't help the benchmarks but I left it in anyway. Going to go ahead and merge this, but if you see anything in those latest changes that looks dumb, don't hesitate to comment and I'll fix!

---

_Merged by @carljm on 2024-08-16 23:34_

---

_Closed by @carljm on 2024-08-16 23:34_

---

_Branch deleted on 2024-08-16 23:34_

---
