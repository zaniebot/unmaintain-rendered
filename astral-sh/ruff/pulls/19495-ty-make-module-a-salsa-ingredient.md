```yaml
number: 19495
title: "[ty] Make `Module` a Salsa ingredient"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
assignees: []
merged: true
base: main
head: ag/module-ingredient
created_at: 2025-07-22T19:02:10Z
updated_at: 2025-07-23T13:46:42Z
url: https://github.com/astral-sh/ruff/pull/19495
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Make `Module` a Salsa ingredient

---

_Pull request opened by @BurntSushi on 2025-07-22 19:02_

We want to write queries that depend on `Module` for caching. While it
seems it can be done without making `Module` an ingredient, it seems it
is [best practice to do so].

[best practice to do so]: https://github.com/astral-sh/ruff/pull/19408#discussion_r2215867301


---

_Review requested from @carljm by @BurntSushi on 2025-07-22 19:02_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-22 19:02_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-22 19:02_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-22 19:02_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-07-22 19:02_

---

_Review request for @dcreager removed by @BurntSushi on 2025-07-22 19:02_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-22 19:02_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-07-22 19:02_

---

_Review request for @AlexWaygood removed by @BurntSushi on 2025-07-22 19:02_

---

_Label `ty` added by @AlexWaygood on 2025-07-22 19:02_

---

_Comment by @BurntSushi on 2025-07-22 19:04_

@MichaReiser RE https://github.com/astral-sh/ruff/pull/19408#discussion_r2215867301

What would be the downside of leaving `Module` as it was and _not_ making it an ingredient? Is it that dependency tracking in Salsa won't be as good as it would be with this change? I'm trying to figure out what would go wrong if we kept the status quo. (I'm not arguing in favor of the status quo, this is for my own edification.)

---

_Comment by @github-actions[bot] on 2025-07-22 19:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-22 19:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @carljm on 2025-07-22 20:11_

I'm curious, given that `File:Module` is `1:1`, why we need to make queries cache based on `Module` instead of based on its `File`? AFAIK we've mostly just based everything around `File`.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/module.rs`:105 on 2025-07-23 05:54_

```suggestion
    fn all_submodules_inner(self, db: &'db dyn Db) -> Option<Vec<Name>> {
```


We no longer need the `dummy` argument here because `Module` is now a `salsa::tracked`

---

_@MichaReiser approved on 2025-07-23 06:01_

> What would be the downside of leaving Module as it was and not making it an ingredient? I

The main downside is that `all_submodule_names` requires the `()` dummy argument because its (otherwise) only argument (`self`) isn't a salsa ingredient. What this does behind the scene is that salsa creates an ad-hoc interned struct with two fields: `dummy: ()` and `module: self`. 

> I'm curious, given that File:Module is 1:1, why we need to make queries cache based on Module instead of based on its File? AFAIK we've mostly just based everything around File.


I must admit, I didn't look at `all_submodule_names` in detail and you're right, we could restructure that function so that only the portion operating on `File` would be the salsa query. That would remove the need to make `Module` a salsa query today. 

It being a salsa struct would still be beneficial if we need queries operating on namespace packages or queries using other `Module` fields that can't be as easily restructured as `all_submodule_names`. 

@BurntSushi has probably more insight here on whether we want to extend `all_submodule_names` to namespace packages in the future or if there are other queries that need to operate at the module level

---

_Comment by @BurntSushi on 2025-07-23 12:53_

> @BurntSushi has probably more insight here on whether we want to extend `all_submodule_names` to namespace packages in the future or if there are other queries that need to operate at the module level

I am not 100% certain about this, but I _suspect_ so. I'm thinking about queries for symbols exported by modules (for auto import). If I'm wrong about this and `Module` is unnecessarily a Salsa ingredient, what kind of downsides are we looking at? Extra memory usage? If it isn't too bad, I think I'd prefer to keep it as an ingredient for now.

I've pushed up another commit that does restructure the code to just use `File` as an input to a query function.

---

_Comment by @MichaReiser on 2025-07-23 13:40_

> If I'm wrong about this and Module is unnecessarily a Salsa ingredient, what kind of downsides are we looking at? Extra memory usage?

* Accessors are slightly slower as they require a db lookup and they're a bit more annoying to use
* Salsa has to track additional metadata for each tracked struct. I don't expect this to be a huge problem here as there are relatively few modules (it's not in the millions)

> I am not 100% certain about this, but I suspect so. I'm thinking about queries for symbols exported by modules (for auto import).

Makes sense. The main question is if they could be over `File` instead of `Module`. But I'm fine merging this as is.

---

_Merged by @BurntSushi on 2025-07-23 13:46_

---

_Closed by @BurntSushi on 2025-07-23 13:46_

---

_Branch deleted on 2025-07-23 13:46_

---
