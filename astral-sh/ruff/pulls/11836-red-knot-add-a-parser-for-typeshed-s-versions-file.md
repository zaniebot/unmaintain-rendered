```yaml
number: 11836
title: "[red-knot] Add a parser for typeshed's VERSIONS file"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: typeshed-versions
created_at: 2024-06-11T14:06:16Z
updated_at: 2024-06-12T12:00:39Z
url: https://github.com/astral-sh/ruff/pull/11836
synced_at: 2026-01-12T15:55:39Z
```

# [red-knot] Add a parser for typeshed's VERSIONS file

---

_@AlexWaygood_

Work towards https://github.com/astral-sh/ruff/issues/11653. This will be necessary for resolving standard-library modules.

---

_Review requested from @carljm by @AlexWaygood on 2024-06-11 14:06_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-11 14:06_

---

_Comment by @AlexWaygood on 2024-06-11 14:08_

I'm not wedded to this being merged now, FWIW: I'm happy to wait until we've sorted out the vendored filesystem stuff first. In some ways, it would make more sense to wait until that's all sorted out, and then integrate this properly; as it is, this is all basically unused right now. I'm mainly filing the PR now to show progress, and I did it now because it was nice and small and self-contained.

---

_Label `red-knot` added by @MichaReiser on 2024-06-11 14:09_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:877 on 2024-06-11 14:12_

I think I would create a custom error here instead of just using `anyhow`. Especially considering that the errors are know ahead of time. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:887 on 2024-06-11 14:13_

It would be nice if we could avoid collecting the items here. You could use `split_once('-')` and then do a `!contains('-')` on the second part to ensure it doesn't contain any other `-`.

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:915 on 2024-06-11 14:15_

Same here, Consider using `split_once` or taking the parts one by one from the iterator over collecting.

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:786 on 2024-06-11 14:15_

I'll leave it to you to port this later because it won't be part of my module resolver refactor.

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:814 on 2024-06-11 14:17_

Is it more common that the top module is limited to a version or that it is a submodule (or, can't say because it is a mixture of both)? I'm asking because it could allow us to start iterating from the more common path.

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:823 on 2024-06-11 14:17_

```suggestion
        let mut map = FxHashMap::default();
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:826 on 2024-06-11 14:18_

Nit: 
```suggestion
            let line_number = OneIndexed::from_zero_indexed(line_number);
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:833 on 2024-06-11 14:19_

Same here. Can we avoid allocating here by using `split_once` or consuming the iterator part by part. 


---

_@MichaReiser reviewed on 2024-06-11 14:20_

Nice. I thikn we shoud try to reduce the number of `collect` calls (we now have multiple collects per line and I don't think they're necessary). Other than that, I think that's good to go.

---

_Comment by @github-actions[bot] on 2024-06-11 14:25_

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

_@AlexWaygood reviewed on 2024-06-11 15:48_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:826 on 2024-06-11 15:48_

where do I need to import this from?

---

_@AlexWaygood reviewed on 2024-06-11 15:49_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:823 on 2024-06-11 15:49_

Argh, I'm sure I tried this and for some reason it didn't work... but it does now 🙃

---

_Comment by @codspeed-hq[bot] on 2024-06-11 15:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/typeshed-versions)

### Merging #11836 will **not alter performance**

<sub>Comparing <code>typeshed-versions</code> (84dc926) with <code>main</code> (60ea72a)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser reviewed on 2024-06-11 15:59_

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:826 on 2024-06-11 15:59_

Ah, It's in `ruff_source_file`. That might not be available and maybe the dependency isn't worth it, considering that the index use is so local here (I'm kind of strict on using it when passing around column and line numbers across modules).

---

_@AlexWaygood reviewed on 2024-06-11 16:06_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:826 on 2024-06-11 16:06_

Ah I see. I think probably not worth the inter-crate dependency just for this. Though that may change when I port this over to `ruff_python_semantic`.

---

_@AlexWaygood reviewed on 2024-06-11 16:08_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:814 on 2024-06-11 16:08_

The most common situation is that there's an exact match in `VERSIONS` -- you're looking to find out the versions `pathlib` exists on, and `VERSIONS` has an entry called `pathlib` in it.

The second most common situation is probably that you're looking up a submodule and the submodule itself doesn't exist in `VERSIONS` but its parent package does -- e.g. you're looking up `email.utils`, which doesn't exist in `VERSIONS`, but `email` does. But I'm not sure we can optimize that, because if `email.utils` _also_ exists in `VERSIONS` as _well_ as `email`, we _have_ to give that entry precedence over the entry for `email`.

---

_@AlexWaygood reviewed on 2024-06-11 17:17_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:877 on 2024-06-11 17:17_

Done in https://github.com/astral-sh/ruff/pull/11836/commits/f7c32383a8b22ff28acf21b9a2e6c0eeb7f8ef9d. Oof, that's a lot more code!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-06-11 18:02_

---

_Comment by @AlexWaygood on 2024-06-11 19:51_

Wondering if this should be a separate module that `module.rs` depends on... it currently uses `ModuleName`, but it should be pretty simple to get rid of that, in which case it wouldn't have dependencies on any of the rest of `red_knot`.

---

_@carljm approved on 2024-06-11 23:23_

Looks reasonable to me!

This does seem pretty self-contained from the rest of the module resolver, so I'd probably be inclined to put it into its own (sub-)module.


---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:893 on 2024-06-12 06:15_

I would use `new().unwrap()` here. I'm pretty sure the compiler is able to to figure out that this is never 0
```suggestion
            let line_number = NonZeroUsize::new(line_number.saturating_add(1)).unwrap();
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:890 on 2024-06-12 06:15_

Nit: Maybe name this `line_index` to make it clear that it is 0 based

```suggestion
        for (line_index, line) in s.lines().enumerate() {
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/module.rs`:1268 on 2024-06-12 06:19_

I recommend copying over the current VERSIONS file specifically for the test and use that instead. That also allows you to explicitly assert the len. 

You could then change your test and simply compare if `display(parsed) == input`

---

_@MichaReiser approved on 2024-06-12 06:19_

> Wondering if this should be a separate module that module.rs depends on... it currently uses ModuleName, but it should be pretty simple to get rid of that, in which case it wouldn't have dependencies on any of the rest of red_knot.

I would probably split it into its own submodule but we can also do this when porting it to `ruff_python_semantic` (or `red_knot_python_semantic`)

---

_@AlexWaygood reviewed on 2024-06-12 08:12_

---

_Review comment by @AlexWaygood on `crates/red_knot/src/module.rs`:1268 on 2024-06-12 08:12_

I think it's important that we do have some tests that check we're able to parse the actual VERSIONS file we have checked in, that we'll be using as a source of truth at runtime. But agreed that it doesn't make for a great unit test; I'll rework this 

---

_Merged by @AlexWaygood on 2024-06-12 11:44_

---

_Closed by @AlexWaygood on 2024-06-12 11:44_

---

_Branch deleted on 2024-06-12 11:44_

---
