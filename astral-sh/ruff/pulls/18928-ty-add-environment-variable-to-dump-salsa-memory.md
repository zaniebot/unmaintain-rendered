```yaml
number: 18928
title: "[ty] Add environment variable to dump Salsa memory usage stats"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: ibraheem/memory-usage-dump
created_at: 2025-06-24T23:53:06Z
updated_at: 2025-06-26T21:34:49Z
url: https://github.com/astral-sh/ruff/pull/18928
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Add environment variable to dump Salsa memory usage stats

---

_@ibraheemdev_

## Summary

Setting `TY_MEMORY_REPORT=full` will generate and print a memory usage report to the CLI after a `ty check` run:

```
=======SALSA STRUCTS=======
`Definition`                                       metadata=7.24MB   fields=17.38MB  count=181062
`Expression`                                       metadata=4.45MB   fields=5.94MB   count=92804
`member_lookup_with_policy_::interned_arguments`   metadata=1.97MB   fields=2.25MB   count=35176
...
=======SALSA QUERIES=======
`File -> ty_python_semantic::semantic_index::SemanticIndex`
    metadata=11.46MB  fields=88.86MB  count=1638
`Definition -> ty_python_semantic::types::infer::TypeInference`
    metadata=24.52MB  fields=86.68MB  count=146018
`File -> ruff_db::parsed::ParsedModule`
    metadata=0.12MB   fields=69.06MB  count=1642
...
=======SALSA SUMMARY=======
TOTAL MEMORY USAGE: 577.61MB
    struct metadata = 29.00MB
    struct fields = 35.68MB
    memo metadata = 103.87MB
    memo fields = 409.06MB
```

Eventually, we should integrate these numbers into CI in some form. The one limitation currently is that heap allocations in salsa structs (e.g. interned values) are not tracked, but memoized values should have full coverage. We may also want a peak memory usage counter (that accounts for non-salsa memory), but that is relatively simple to profile manually (e.g. `time -v ty check`) and would require a compile-time option to avoid runtime overhead.

Depends on https://github.com/salsa-rs/salsa/pull/925.

---

_Review requested from @carljm by @ibraheemdev on 2025-06-24 23:53_

---

_Label `internal` added by @ibraheemdev on 2025-06-24 23:53_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-06-24 23:53_

---

_Label `ty` added by @ibraheemdev on 2025-06-24 23:53_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-06-24 23:53_

---

_Review requested from @dcreager by @ibraheemdev on 2025-06-24 23:53_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-06-24 23:53_

---

_Review requested from @dhruvmanila by @ibraheemdev on 2025-06-24 23:53_

---

_@ibraheemdev reviewed on 2025-06-24 23:54_

---

_Review comment by @ibraheemdev on `Cargo.toml`:83 on 2025-06-24 23:54_

This is my personal fork of the `get-size2` crate, which adds some missing features and fixes some inaccuracies. I opened PRs for all of these on the original repository, so we should be able to switch once those are upstreamed.

---

_Comment by @github-actions[bot] on 2025-06-24 23:59_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-25 00:07_

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

_@dhruvmanila reviewed on 2025-06-25 04:18_

---

_Review comment by @dhruvmanila on `crates/ty/src/lib.rs`:162 on 2025-06-25 04:18_

I think it would be more useful that this constructs a report in `String` or any other data structure that implements `Display` so that it can be used in the ty server as well. The idea would be to provide a custom request or using a command that the client can request to get this information and is displayed in the editor. For example, we could have a custom endpoing `ty/salsaMemory` that would return this content. It's similar to how rust-analyzer implements various debugging endpoints like https://rust-analyzer.github.io/book/contributing/lsp-extensions.html#analyzer-status.

---

_Review comment by @MichaReiser on `Cargo.toml`:141 on 2025-06-25 06:02_

We should update this version before merging

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:312 on 2025-06-25 06:06_

Can't we derive the implementation for `GetSize` instead, considering that it will be derived on the final struct (that only wraps the id)?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:148 on 2025-06-25 06:06_

What's the reason for ignoring the notebook variant here? 

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:162 on 2025-06-25 06:09_

Agree, I think returning a custom struct that implements `Display` would be useful so that we can use it in the LSP too

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/expression.rs`:66 on 2025-06-25 06:13_

Same as for `File`. Can't we derive `GetSize` here because it would be applied on the `Expression(salsa::Id)` type?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/place.rs`:449 on 2025-06-25 06:13_

Same, can we derive `GetSize` here?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/predicate.rs`:95 on 2025-06-25 06:13_

...and in all other places

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:259 on 2025-06-25 06:14_

What's the reason that we use an empty implementation for `UseDefMap` given that most its fields are heap allocated?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:429 on 2025-06-25 06:15_

Can't we put the derive on line 416 (same for other bitflags)?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/util/get_size.rs`:10 on 2025-06-25 06:18_

The documentation doesn't make it entirely clear to me how an `Arc` is counted. From the code, my guess is that it counts the size of the arced value (it does double count if the arc is shared)

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:151 on 2025-06-25 06:22_

It would be nice if:

* We would print a condenced memory report when running with `-vv` 
* We would print the full memory report when running with `-vvv`

You can get the verbosity from `args.verbosity`. 

I would probably skip the environment variable for now. If we don't, make sure to add it here https://github.com/astral-sh/ty/blob/main/docs/reference/env.md

---

_Review comment by @MichaReiser on `Cargo.toml`:83 on 2025-06-25 06:23_

Would it make sense to create a fork in the astral org instead?

---

_@MichaReiser approved on 2025-06-25 06:25_

This is great

I've a few smaller nits. The only downside of the design is that it's very easy to forget the `heap_size` attribute on a salsa query which will result in under counting. That makes me wonder if we should change the design in salsa so that the `stack` and `heap_size` is reported separately for each query (we can show a total as well) and the `heap_size` would be `Unknown` if the `heap_size` attribute isn't set. This would make it more appearant where `heap_size` attributes are missing (compared to, ah, this query doesn't allocate much)

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-25 08:04_

---

_@ibraheemdev reviewed on 2025-06-25 17:46_

---

_Review comment by @ibraheemdev on `Cargo.toml`:83 on 2025-06-25 17:46_

The PRs were merged so we can just use the released version now.

---

_@ibraheemdev reviewed on 2025-06-25 17:47_

---

_Review comment by @ibraheemdev on `crates/ruff_db/src/files.rs`:312 on 2025-06-25 17:47_

Unfortunately `salsa::Id` does not implement `GetSize`, and we have no way of putting an `ignore` attribute on that field.

---

_@ibraheemdev reviewed on 2025-06-25 17:50_

---

_Review comment by @ibraheemdev on `crates/ruff_db/src/source.rs`:148 on 2025-06-25 17:50_

The `RawNotebook` has a lot of `serde_json::Value` types, I wasn't sure it was worth the effort to add a custom implementation for that given that we are not likely to be profiling the memory usage of the notebook schema.

---

_@ibraheemdev reviewed on 2025-06-25 17:50_

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/semantic_index/use_def.rs`:259 on 2025-06-25 17:50_

This was a mistake, fixed.

---

_Review comment by @ibraheemdev on `crates/ty_python_semantic/src/types.rs`:429 on 2025-06-25 17:51_

The internal bitflags type does not implement `GetSize`.

---

_@ibraheemdev reviewed on 2025-06-25 17:51_

---

_@ibraheemdev reviewed on 2025-06-25 18:34_

---

_Review comment by @ibraheemdev on `crates/ty/src/lib.rs`:151 on 2025-06-25 18:34_

I think having to run `-vvv` is a little difficult because of the amount of tracing logs you have to wait for :) I kept the environment variable but added `short` and `full` options. We might eventually have to add an option for mypy primer that keeps the diff less sensitive to minor changes.

---

_@MichaReiser reviewed on 2025-06-25 20:21_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/source.rs`:148 on 2025-06-25 20:21_

Fair enough. It could make sense extending the TODO comment to why it's currently excluded.

---

_@dhruvmanila reviewed on 2025-06-26 03:43_

---

_Review comment by @dhruvmanila on `crates/ty/src/lib.rs`:467 on 2025-06-26 03:43_

Given that we're returning a struct, I don't think this is required now?

```suggestion
```

---

_Review comment by @MichaReiser on `Cargo.toml`:141 on 2025-06-26 06:17_

```suggestion
salsa = { git = "https://github.com/ibraheemdev/salsa", rev = "e53c2ab7" }
```
```suggestion
salsa = { git = "https://github.com/salsa-rs/salsa.git", rev = "0666e2018bc35376b1ac4f98906f2d04d11e5fe4" }
```

---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:465 on 2025-06-26 06:20_

Nit: It might be helpful if we can call this in tests too (e.g. mdtests). Therefore, I'd suggest making this a method on `ProjectDatabase`

---

_@MichaReiser approved on 2025-06-26 06:22_

---

_Merged by @ibraheemdev on 2025-06-26 21:27_

---

_Closed by @ibraheemdev on 2025-06-26 21:27_

---

_Branch deleted on 2025-06-26 21:27_

---
