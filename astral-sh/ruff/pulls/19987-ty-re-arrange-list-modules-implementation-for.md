```yaml
number: 19987
title: "[ty] Re-arrange \"list modules\" implementation for Salsa caching"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/import-module-caching
created_at: 2025-08-19T11:35:57Z
updated_at: 2025-08-20T14:41:48Z
url: https://github.com/astral-sh/ruff/pull/19987
synced_at: 2026-01-12T15:56:52Z
```

# [ty] Re-arrange "list modules" implementation for Salsa caching

---

_@BurntSushi_

This basically splits `list_modules` into a higher level "aggregation"
routine and a lower level "get modules for one search path" routine.
This permits Salsa to cache the lower level components, e.g., many
search paths refer to directories that rarely change. This saves us
interaction with the system.

This did require a fair bit of surgery in terms of being careful about
adding file roots. Namely, now that we rely even more on file roots
existing for correct handling of cache invalidation, there were several
spots in our code that needed to be updated to add roots (that we
weren't previously doing). This feels Not Great, and it would be better
if we had some kind of abstraction that handled this for us. But it
isn't clear to me at this time what that looks like.

Note that this PR builds on top of #19883.

---

_Review requested from @MichaReiser by @BurntSushi on 2025-08-19 11:36_

---

_Comment by @BurntSushi on 2025-08-19 11:37_

This PR is based on top of #19883. It has a test failure that is _not_ present in #19883:

```
$ cargo test -p ty_python_semantic --lib -- module_resolver::list::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query
    Finished `test` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running unittests src/lib.rs (target/debug/deps/ty_python_semantic-0f1aad274f1110a3)

running 1 test
test module_resolver::list::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query ... FAILED

failures:

---- module_resolver::list::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query stdout ----

thread 'module_resolver::list::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query' panicked at /home/andrew/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/interned.rs:774:9:
Data was not interned in the latest revision for its durability.
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    module_resolver::list::tests::deleting_file_from_higher_priority_search_path_invalidates_the_query

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 292 filtered out; finished in 0.04s

error: test failed, to rerun pass `-p ty_python_semantic --lib`
```

---

_Comment by @github-actions[bot] on 2025-08-19 11:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-19 11:39_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @BurntSushi on 2025-08-19 11:40_

(Sorry, this PR is on top of #19883, _not_ #19975.)

---

_Label `server` added by @AlexWaygood on 2025-08-20 09:20_

---

_Label `ty` added by @AlexWaygood on 2025-08-20 09:20_

---

_Marked ready for review by @BurntSushi on 2025-08-20 12:55_

---

_Review requested from @carljm by @BurntSushi on 2025-08-20 12:55_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-08-20 12:55_

---

_Review requested from @sharkdp by @BurntSushi on 2025-08-20 12:55_

---

_Review requested from @dcreager by @BurntSushi on 2025-08-20 12:55_

---

_Review request for @dcreager removed by @BurntSushi on 2025-08-20 12:55_

---

_Review request for @carljm removed by @BurntSushi on 2025-08-20 12:55_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-08-20 12:55_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/module_resolver/list.rs`:48 on 2025-08-20 13:01_

```suggestion
/// List all available top-level modules in the given `SearchPath`.
```

and maybe make the same changes above. I recall being confused while reviewing the initial implementation, as I incorrectly assumed it would walk directories recursively to find all modules when it only searches for the top-level modules

---

_@MichaReiser approved on 2025-08-20 13:04_

Your initial design makes adding caching look easy. Nice job!

I assume you tested the cache invalidation with some LSP? 

@Gankra maybe something you want to take a look at too

---

_@BurntSushi reviewed on 2025-08-20 14:10_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/module_resolver/list.rs`:48 on 2025-08-20 14:10_

Makes sense!

---

_Comment by @BurntSushi on 2025-08-20 14:26_

Oh, and here's a demo of the caching working:

https://github.com/user-attachments/assets/ba5ad560-3f19-4b3c-8a77-6d1908bebd11



---

_Merged by @BurntSushi on 2025-08-20 14:41_

---

_Closed by @BurntSushi on 2025-08-20 14:41_

---

_Branch deleted on 2025-08-20 14:41_

---
