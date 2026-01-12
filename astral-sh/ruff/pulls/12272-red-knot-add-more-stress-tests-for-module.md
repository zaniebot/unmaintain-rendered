```yaml
number: 12272
title: "[red-knot] Add more stress tests for module resolver invalidation"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: module-resolver-salsa-bug
created_at: 2024-07-10T11:48:01Z
updated_at: 2024-07-10T14:43:19Z
url: https://github.com/astral-sh/ruff/pull/12272
synced_at: 2026-01-12T15:55:40Z
```

# [red-knot] Add more stress tests for module resolver invalidation

---

_@AlexWaygood_

## Summary

This PR adds a stress test to the module resolver to check that adding and removing files at various points in the module-resolution heirarchy invalidates the Salsa cache.

~~The test currently fails. Deleting the first-party and stdlib functools modules should invalidate the cache, meaning that functools now resolves to the functools module in site-packages. That's not the case, however: after deleting those two files, the module resolver appears to still be resolving the module to the deleted src/functools.py file.~~

~~I've tried to look into what might be causing this, but I haven't had any luck in figuring it out. Curious if @MichaReiser or @carljm have any ideas :/~~

## Test Plan

`cargo test -p red_knot_module_resolver`


---

_Label `red-knot` added by @AlexWaygood on 2024-07-10 11:48_

---

_Review requested from @carljm by @AlexWaygood on 2024-07-10 11:48_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-07-10 11:48_

---

_Comment by @codspeed-hq[bot] on 2024-07-10 11:53_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/module-resolver-salsa-bug)

### Merging #12272 will **not alter performance**

<sub>Comparing <code>module-resolver-salsa-bug</code> (2f8e0ea) with <code>module-resolver-salsa-bug</code> (787f857)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_@MichaReiser reviewed on 2024-07-10 12:02_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1074 on 2024-07-10 12:02_

Any `memory_file_system` operations don't go through salsa. In this case, salsa doesn't know that you deleted the files. We have to tell it that the files changed on the file system. 

In the CLI, a file watcher will take care of this, in tests, at least for now, we have to do this manually by either using `file.touch(db)` or `File::touch_path(db, path)`

---

_@AlexWaygood reviewed on 2024-07-10 12:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1074 on 2024-07-10 12:03_

🤦 of course. I was looking for places where I was using the file-system APIs in the module resolver instead of going through Salsa, but I obviously need to do the same in the test as well...

---

_Renamed from "Failing test to illustrate Salsa invalidation bug in module resolver" to "[red-knot] Add a stress test for module resolver invalidation" by @AlexWaygood on 2024-07-10 12:16_

---

_Comment by @github-actions[bot] on 2024-07-10 12:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1052 on 2024-07-10 12:37_

The assertion here are good, but they don't assert that the query isn't re-executed. 

There are some helpers that allow you to verify if a query did execute again (you might have to move them) that you can call in addition to the assertions you already have.

https://github.com/astral-sh/ruff/blob/ac04380f360b1fca84435fbbf3154f61e551a871/crates/red_knot_python_semantic/src/types.rs#L388-L393

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1063 on 2024-07-10 12:38_

It could make sense to split each of these blocks (cases) into its own tests. But I don't feel strongly about it.

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1075 on 2024-07-10 12:39_

You can use `write_file` here
```suggestion
        db.write_file(&stdlib_versions_path, "")
            .unwrap();
```

---

_@MichaReiser approved on 2024-07-10 12:39_

---

_@AlexWaygood reviewed on 2024-07-10 12:42_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1052 on 2024-07-10 12:42_

Hmm, where would you move these to? `ruff_db`?

---

_@MichaReiser reviewed on 2024-07-10 12:44_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1052 on 2024-07-10 12:44_

Probably yes. Let's hope they never depend on the specific DB type

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1055 on 2024-07-10 13:19_

@MichaReiser am I doing this right? It... doesn't pass for me locally, but I'm not sure if that's because there's a bug in the module resolver or (probably more likely?) I'm using the test function incorrectly...

---

_@AlexWaygood reviewed on 2024-07-10 13:19_

---

_@MichaReiser reviewed on 2024-07-10 13:33_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1055 on 2024-07-10 13:33_

You want to call `db.clear_salsa_events` before starting the mutations to remove the events from the previous computation. 

You also want to move the assertion after your new `resolve_module` call (that should be cached). 

Another way to think about it:
* Clear the events that you're not interested in. That's mainly the events from creating a "warm" salsa cache
* Execute whatever query you want to assert if it gets executed (or not)
* Call the assertion method to verify that the query was executed, or not

```rust
				// Adding a file to site-packages does not invalidate the query,
        // since site-packages takes lower priority in the module resolution...
        let site_packages_functools_path = site_packages.join("functools.py");
        db.write_file(&site_packages_functools_path, "f: int")
            .unwrap();
				db.clear_salsa_events();
	        
				let functools_module = resolve_module(&db, functools_module_name.clone()).unwrap();
        assert_eq!(functools_module.search_path(), stdlib);
        assert_eq!(
            Some(functools_module.file()),
            system_path_to_file(&db, &stdlib_functools_path)
        );

        let events = db.take_salsa_events();
        assert_will_not_run_function_query::<resolve_module_query, _, _>(
            &db,
            |res| &res.function,
            &ModuleNameIngredient::new(&db, functools_module_name.clone()),
            &events,
        );
```

---

_@AlexWaygood reviewed on 2024-07-10 13:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1055 on 2024-07-10 13:37_

I see. Is it then worth renaming the test function from `assert_will_run_function_query()` to `assert_function_query_was_not_run()`?

---

_@MichaReiser reviewed on 2024-07-10 13:45_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:1055 on 2024-07-10 13:45_

Yeah, I think using past tense in the function name makes sense. I should have thought more about it but I was just happy when the generic madness finally compiled :laughing: 

---

_@AlexWaygood reviewed on 2024-07-10 14:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:1063 on 2024-07-10 14:00_

At this stage, I guess I agree, it's quite a monster now 😄

---

_Renamed from "[red-knot] Add a stress test for module resolver invalidation" to "[red-knot] Add more stress tests for module resolver invalidation" by @AlexWaygood on 2024-07-10 14:30_

---

_Merged by @AlexWaygood on 2024-07-10 14:34_

---

_Closed by @AlexWaygood on 2024-07-10 14:34_

---

_Branch deleted on 2024-07-10 14:34_

---
