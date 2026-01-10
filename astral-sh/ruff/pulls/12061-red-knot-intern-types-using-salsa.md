```yaml
number: 12061
title: "[red-knot] intern types using Salsa"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/fine
created_at: 2024-06-27T04:15:45Z
updated_at: 2024-07-05T19:16:39Z
url: https://github.com/astral-sh/ruff/pull/12061
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] intern types using Salsa

---

_Pull request opened by @carljm on 2024-06-27 04:15_

Intern types using Salsa interning instead of in the `TypeInference` result.

This eliminates the need for `TypingContext`, and also paves the way for finer-grained type inference queries.

---

_Label `red-knot` added by @MichaReiser on 2024-06-28 07:50_

---

_Comment by @MichaReiser on 2024-06-28 08:16_

As reference, this is how rustc interns types: 

The data type:

https://github.com/rust-lang/rust/blob/42add88d2275b95c98e512ab680436ede691e853/compiler/rustc_middle/src/ty/adt.rs#L97-L106

A reference to a type (bound to the `TyCtxt` lifetime)

https://github.com/rust-lang/rust/blob/42add88d2275b95c98e512ab680436ede691e853/compiler/rustc_middle/src/ty/adt.rs#L172-L174

The definition of `Interned`

https://github.com/rust-lang/rust/blob/42add88d2275b95c98e512ab680436ede691e853/compiler/rustc_data_structures/src/intern.rs#L13-L25

And this is used in `TyCtxt` which is rustcs arena where it interns all data. 

https://github.com/rust-lang/rust/blob/42add88d2275b95c98e512ab680436ede691e853/compiler/rustc_middle/src/ty/context.rs#L194-L197





---

_@MichaReiser reviewed on 2024-06-28 08:30_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/intern.rs`:39 on 2024-06-28 08:30_

I think returning a refence would work if we use an actual `Arena` that allocates a new vector when it runs out of space instead of resizing the existing one (which would invalidate previously returned Ids). 

Or more generally. The way to return references here is by lifting the lifetime of types outside of type store by having the actual data owned further up. 



---

_Comment by @MichaReiser on 2024-06-28 08:41_

I do like splitting out the type internet from the type inference result. I think it makes sense to have them separate and also allows more effective reuse. 

The part that's unclear to me and that you already mentioned on your write up is how to collect unused types. How can we prevent that this is an ever-growing arena? 


We could try to track type references by using an `AtomicUsize` (not an `Arc` because that would heap allocate each type!) It would give us very fine-grained reclamation of types, at least if we use a slab like arena that supports reusing IDs. An other possibility could be to rely on Salsa by using one interner per file, package or some other granularity, and the interner would be retrieved by calling a salsa-query. This won't give us type level collection, but salsa would evict the entire type store if e.g. the file gets deleted (or all files in a package). 



---

_Comment by @carljm on 2024-07-05 06:45_

This now uses Salsa interning, which eliminates a lot of our own interning and ID boilerplate, and actually works!

---

_Comment by @codspeed-hq[bot] on 2024-07-05 06:48_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm/fine)

### Merging #12061 will **degrade performances by 4.03%**

<sub>Comparing <code>cjm/fine</code> (c93aeeb) with <code>main</code> (7b50061)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cjm/fine` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[without_parse]` | 292 µs | 304.3 µs | -4.03% |


---

_Comment by @carljm on 2024-07-05 06:55_

Some things in this PR that might require further attention:
* I think you mentioned you don't want to expose Salsa Db directly to lint rules, but I'm not sure how this will work with the need for lint rules to call methods on types. Previously you exposed typing context, which is just a wrapped db and is passed to all methods on types. Now methods on types take a db, so if we want to avoid exposing db to lint rules, they won't be able to call methods on types.
* Interning `UnionType` and `IntersectionType` in Salsa requires that all their fields be hashable, which means I can't use `IndexSet`. For now I just switched to `Vec`, but I need to do a bit more careful analysis of what properties we need here (do we want ordering? do we need fast contains checks? etc) in order to settle on the right data structure.

---

_Comment by @github-actions[bot] on 2024-07-05 06:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/python-trio/trio">python-trio/trio</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
Failed to clone python-trio/trio: warning: Could not find remote branch master to clone.
fatal: Remote branch master not found in upstream origin
```

</p>
</details>




---

_Comment by @carljm on 2024-07-05 06:58_

The message says I'm supposed to acknowledge the regression on CodSpeed, but when I login to CodSpeed with GitHub, it says I'm not allowed to acknowledge the regression...

---

_Marked ready for review by @carljm on 2024-07-05 06:58_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-05 06:58_

---

_Review requested from @MichaReiser by @carljm on 2024-07-05 06:58_

---

_Renamed from "[red-knot] intern types separately from type inference result" to "[red-knot] intern types using Salsa" by @carljm on 2024-07-05 06:58_

---

_@MichaReiser reviewed on 2024-07-05 08:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:275 on 2024-07-05 08:44_

I'm unsure if it is worth interning `ModuleType`, considering that it only stores a `VfsFile`. A salsa ingredient requires more metadata than the amount of memory that we save by interning the module type. 

---

_@MichaReiser approved on 2024-07-05 08:47_

I'm a bit surprised by the regression but I think it makes sense. Previously, accessing a field on `ClassType` was reading a reference but it is now a lookup in the salsa ingredient table. Niko's work should help reduce that cost but certainly something to be aware of. The good news, we can still build a more fancy type interner if it turns out that this is the bottleneck

---

_Comment by @MichaReiser on 2024-07-05 08:49_

You now have the permissions to acknowledge regressions. Use it responsibly :P

---

_@MichaReiser reviewed on 2024-07-05 11:01_

---

_Review comment by @MichaReiser on `crates/red_knot/src/lint.rs`:153 on 2024-07-05 11:01_

Okay, This is a bit problematic because I don't want to give lint rules access to the `db`, because I don't consider the `db` to be part of the stable API. 

The solution for `has_decorator` is relatively simple, we make it a method on `model`: `model.has_decorator(ty, decorator_ty)`. It's less clear to me me how we remove the `db` dependency from `name`, and `inherited_class_member`. Can we change `has_decorator` and add a TODO for the other methods.

---

_Review comment by @carljm on `crates/red_knot/src/lint.rs`:153 on 2024-07-05 15:46_

After offline discussion, I'm going to just add a TODO here; we need to explore this further and find a general solution, and it's not clear that we'd even want `has_decorator` to be a method directly on the `SemanticModel` once we have that general solution.

---

_@carljm reviewed on 2024-07-05 15:46_

---

_@carljm reviewed on 2024-07-05 15:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:275 on 2024-07-05 15:47_

Good point, I can try removing this.

---

_Comment by @carljm on 2024-07-05 15:50_

After offline discussion with @MichaReiser, going to switch to `OrderSet` for unions and intersections (from https://crates.io/crates/ordermap/0.5.0 ), which is a newtype wrapper around `IndexSet` that also adds order-respecting `Eq` and `Hash`. If we care about respecting order in unions (for UX reasons only, not because it's meaningful in the type system), which I think we do, and we care about fast contains checks (I think we also do), then this seems like the best option. It means we will intern "duplicate" unions that differ only in order separately, but this is pretty much unavoidable if we really want to respect order for UX. We will have to see how much of a cost this is in practice, and if it's worth paying for the UX.

---

_@carljm reviewed on 2024-07-05 18:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:275 on 2024-07-05 18:37_

I just removed `ModuleType` entirely in favor of having `Type::Module` directly hold a `VfsFile`. At the moment there's really no reason to have the extra `ModuleType` layer at all, just to implement the `member` method, which is a one-liner that can just as well go directly in `Type::member` in the `Type::Module` arm.

---

_Comment by @carljm on 2024-07-05 18:58_

The updated perf results are a bit better; not sure if this is just noise or if not interning `ModuleType` actually helped. I don't think Vec vs OrderSet should help, since we aren't currently doing any contains checks on unions or intersections.

---

_Merged by @carljm on 2024-07-05 19:16_

---

_Closed by @carljm on 2024-07-05 19:16_

---

_Branch deleted on 2024-07-05 19:16_

---
