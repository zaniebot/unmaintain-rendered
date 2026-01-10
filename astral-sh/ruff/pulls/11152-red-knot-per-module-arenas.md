```yaml
number: 11152
title: "[red-knot] per-module arenas"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: red-knot
head: cjm/red-knot/type-arenas
created_at: 2024-04-25T20:43:04Z
updated_at: 2024-04-26T20:05:50Z
url: https://github.com/astral-sh/ruff/pull/11152
synced_at: 2026-01-10T22:37:01Z
```

# [red-knot] per-module arenas

---

_Pull request opened by @carljm on 2024-04-25 20:43_

I got this all working and solved the API lifetime issues without Arc, by means of a new set of `XTypeRef` structs.

The remaining potential performance issue is that anytime you hold on to any of the new `XTypeRef` structs, you lock a shard of the `TypeStore::modules` dashmap to writes (because you are holding a reference into it). So it will be important to minimize the use and scope of these type-refs. I think we can do this to some degree by caching type judgments using just type IDs. I also think for CLI use when we want to be highly parallel, we can be smart about ordering (check all module bodies first, then check function bodies when module level types are all populated) to minimize write contention. Also, if needed we can break up `ModuleTypeStore`, or use inner mutability and internal locking to have finer-grained locking within it.

I went with this version instead of rewriting to have the type arenas hold Arc to the types, because I am not totally convinced the Arc version will be better. With Arc every "read" turns into a write to the atomic reference count, which introduces overhead (which is really useless overhead for us, since ultimately we rely on the arenas for garbage collection). And so we will introduce contention on the atomic reference count even for reads of highly-used types. So for both versions we will have to be careful with our use of references. I think the Arc-free version is lower overhead and sets us up better for future optimization of the locking strategy, once we have more working code to optimize against.

Even if I turn out to be wrong about the above and eventually we decide to use Arc, I'd rather go with this for now and move on to type evaluation, and make the Arc change later when we can evaluate the effects better.

---

_Converted to draft by @carljm on 2024-04-25 20:43_

---

_Review requested from @MichaReiser by @carljm on 2024-04-25 20:48_

---

_Renamed from "[WIP] per-module arenas" to "[red-knot] per-module arenas" by @carljm on 2024-04-25 21:34_

---

_Comment by @github-actions[bot] on 2024-04-25 21:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 43 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/48a68f521ee1cb23a2aea43951a5000d2600d824/stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi#L10'>stubs/setuptools/pkg_resources/_vendored_packaging/specifiers.pyi:10:22:</a> PYI001 Name of private `TypeVar` must start with `_`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @carljm on 2024-04-25 22:50_

---

_Review requested from @BurntSushi by @carljm on 2024-04-25 22:50_

---

_Comment by @carljm on 2024-04-25 22:51_

Also requesting review from @BurntSushi, not necessarily to review all the code (though that would be awesome too!) but mostly just to double-check whether any of what I wrote in the summary sounds totally off base at a high level.

---

_Review requested from @AlexWaygood by @carljm on 2024-04-25 22:55_

---

_Comment by @carljm on 2024-04-25 22:56_

And @AlexWaygood, mostly just to observe that I switched unions and intersections from `HashSet` to `IndexSet`, which provides both O(1) containment checks and de-duplication, and preserves ordering. So we have the capability to distinguish order, both for UX purposes (I wanted it even just for stable tests of the union/intersection display code!), and if needed for actual type system purposes (I don't think we should, but now we can, if you convince me otherwise!)

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:64 on 2024-04-26 07:22_

Should this return a `ClassId` similar to `get_class` that returns a `ClassTypeRef`?

---

_@MichaReiser approved on 2024-04-26 07:33_

Nice!

> And so we will introduce contention on the atomic reference count even for reads of highly-used types.

I'm not sure I follow the reasoning here. Isn't the main contention the module lock that guards access to the type? And if so, isn't holding on to the lock for longer than necessary (for longer than incrementing a reference counter) causing worse contention? 

I think my main worry is that it's non obvious for callers that they're holding on to a lock, making it very easy to hold on to a lock for too-long (I just call this very expensive function and pass a reference to a &FunctionType). 

I wonder if Salsa solves some of this by making the accessors methods on the `Id` types. 

```
let function: FunctionId = // somehow get a function id
let name = function.name(db); // smolstr::Clone is O(1)
let parameters  = function.parameters(db); // We could intern `Parameters` into an arena so that `Parameters` is just an Id.

for parameter in parameters.iter(db) {
	let name = parameter.name(db);
}
```

I must admit, it's a bit awkward dealing with so many ids and I don't know what the overhead of all the hash table lookups is. 

But I'm happy to try this out and iterate on it later on. 

---

_@carljm reviewed on 2024-04-26 13:55_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:64 on 2024-04-26 13:55_

Yeah, that might actually make some things more convenient, I'll try it out and see how it works out.

---

_Comment by @carljm on 2024-04-26 15:52_

> Isn't the main contention the module lock that guards access to the type? And if so, isn't holding on to the lock for longer than necessary (for longer than incrementing a reference counter) causing worse contention?

I am really not sure how all this will play out in practice with our actual workload, so take all of this with a grain of salt! (Really my top-line motivation here was just to do the quickest thing that would work and move on, and re-evaluate once we can actually benchmark on real workloads and see how the contention plays out.)

Dashmap allows multiple simultaneous readers (they have to take a reader lock, but they don't contend with each other, only with writers). With the approach in this PR, there is no contention between readers, but readers may block writers for longer (however long a TypeRef is held). If we add Arc, then even readers have to contend over incrementing the atomic reference count (even if there is no writer). It may be that this is a non-issue in practice, I don't know. I just know that e.g. for CPython, which also does reference counting, contention over reference counts for hot shared objects is a huge performance issue if you try to run Python code concurrently and make reference counts atomic. And I know in general (just from searching on the topic) that Arc contention can be a big problem for scaling parallel Rust workloads too.

> I think my main worry is that it's non obvious for callers that they're holding on to a lock, making it very easy to hold on to a lock for too-long (I just call this very expensive function and pass a reference to a &FunctionType).

Yes, I'm also concerned that this will be a foot-gun.

> I wonder if Salsa solves some of this by making the accessors methods on the Id types.

Yes, I saw you did this with the `Module` type, and it's an alternative we could move towards if we need to. My main concerns about it are all the extra hashmap lookups if you need multiple things from a type all at once (as you mentioned), and the need to copy anything returned by these accessors (though this may not be an issue if it's mostly just small strings and other type IDs.)


---

_Comment by @carljm on 2024-04-26 15:56_

My other issue with using Arc here is similar to your objection to using a GC crate: it's just overhead that we don't actually need. We don't need reference counting, because we are using arenas to deallocate the types all at once, and IDs to refer between them. So all the work to update the reference counts (and any contention it causes) is just waste.

(Not pure waste, because it does provide a solution to the problem of callers taking temporary references to the type data and knowing they will keep it alive, without blocking writers to the arena. But it still feels like a big hammer for that problem, which should be solvable with just references and lifetimes.)

---

_Comment by @MichaReiser on 2024-04-26 16:37_

On the `Arc` overhead. I agree, it's neglectagble for e.g building the symbol table but it can become a problem if the object is used often. I think we want to explore a non-std `Arc` to get an implementation without `WeakRef` support. That will help but obviously doesn't mitigate the perf overhead entirely.

---

_Comment by @ibraheemdev on 2024-04-26 16:46_

Another option is using a different concurrent hash-table library, like `flurry`, where holding a read guard doesn't block writers, just potentially block memory from being freed.

---

_Comment by @carljm on 2024-04-26 18:05_

> I think we want to explore a non-std Arc to get an implementation without WeakRef support. That will help but obviously doesn't mitigate the perf overhead entirely.

Where would the savings come from here? It looks to me like weakrefs only really have a cost if you use them. You could save a tiny bit of memory in `ArcInner`, I guess (one `AtomicUsize`), but otherwise I'm not seeing where not supporting weakrefs would make Arc itself faster (or reduce refcount contention.)

> Another option is using a different concurrent hash-table library, like flurry, where holding a read guard doesn't block writers, just potentially block memory from being freed.

Ooh, thank you for pointing this out! I spent a few minutes yesterday looking for exactly this, but for some reason `flurry` didn't come up in my search. Definitely might consider this instead of DashMap for the type store.

---

_@carljm reviewed on 2024-04-26 18:28_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:64 on 2024-04-26 18:28_

Yeah this was a good call, done and pushed.

---

_Merged by @carljm on 2024-04-26 18:54_

---

_Closed by @carljm on 2024-04-26 18:54_

---

_Branch deleted on 2024-04-26 18:54_

---

_Label `internal` added by @carljm on 2024-04-26 20:05_

---
