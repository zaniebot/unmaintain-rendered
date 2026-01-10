```yaml
number: 10214
title: "refactor: Use `QualifiedName` for `Imported::call_path` "
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: qualified-name-lifetimes
created_at: 2024-03-03T18:56:38Z
updated_at: 2024-03-06T08:56:00Z
url: https://github.com/astral-sh/ruff/pull/10214
synced_at: 2026-01-10T22:47:01Z
```

# refactor: Use `QualifiedName` for `Imported::call_path` 

---

_Pull request opened by @MichaReiser on 2024-03-03 18:56_

## Summary

When you try to remove an internal representation leaking into another type and end up rewriting a simple version of `smallvec`. 

The goal of this PR is to replace the `Box<[&'a str]>` with `Box<QualifiedName>` to avoid that the internal `QualifiedName` representation leaks (and it gives us a nicer API too). However, doing this when `QualifiedName` uses `SmallVec` internally gives us all sort of funny lifetime errors. I was lost but @BurntSushi  came to rescue me. He figured out that `smallvec` has a variance problem which is already tracked in https://github.com/servo/rust-smallvec/issues/146

To fix the variants problem, I could use the smallvec-2-alpha-4 or implement our own smallvec. I went with implementing our own small vec for this specific problem. It obviously isn't as sophisticated as smallvec (only uses safe code), e.g. it doesn't perform any size optimizations, but it does its job.

Other changes:

* Removed `Imported::qualified_name` (the version that returns a `String`). This can be replaced by calling `ToString` on the qualified name.
* Renamed `Imported::call_path` to `qualified_name` and changed its return type to `&QualifiedName`. 
* Renamed `QualifiedName::imported` to `user_defined` which is the more common term when talking about builtins vs the rest/user defined functions.


## Test plan

`cargo test`



---

_Label `internal` added by @MichaReiser on 2024-03-04 20:22_

---

_Comment by @codspeed-hq[bot] on 2024-03-04 20:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/qualified-name-lifetimes)

### Merging #10214 will **not alter performance**

<sub>Comparing <code>qualified-name-lifetimes</code> (9b04828) with <code>main</code> (d441338)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-03-04 20:41_

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

_Renamed from "qualified name lifetimes" to "refactor: Use `QualifiedName` for `Imported::call_path` " by @MichaReiser on 2024-03-04 22:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:80 on 2024-03-05 08:46_

Inlined from `format_qualified_name_segments`. We no longer need `format_qualified_name_segments` because the code that used `Box<[&'astr]>` can now call into this display implementation.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:172 on 2024-03-05 08:48_

Inlined from `collect_segments` (see separate commit)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:303 on 2024-03-05 08:49_

I rewrote this to no longer require recursion (which creates new `SegmentsVec`s internally, which seems unnecessary when we know that we need a `Vec` anyway).

---

_Review requested from @BurntSushi by @MichaReiser on 2024-03-05 09:07_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-03-05 09:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/binding.rs`:586 on 2024-03-05 09:08_

It would be nice if we could return `QualifiedNameRef` or similar from here because the module name is a qualified name too. But I consider this out of the scope of this PR.

---

_@MichaReiser reviewed on 2024-03-05 09:08_

---

_Marked ready for review by @MichaReiser on 2024-03-05 09:08_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-03-05 09:08_

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/name.rs`:531 on 2024-03-05 12:28_

It's possible [`arrayvec`](https://docs.rs/arrayvec/latest/arrayvec/) could simplify things a bit here, although it [may have the same issue as `smallvec`](https://github.com/bluss/arrayvec/issues/96).

---

_Review comment by @BurntSushi on `crates/ruff_python_ast/src/name.rs`:804 on 2024-03-05 12:29_

Nice tests. :)

---

_@BurntSushi approved on 2024-03-05 12:30_

w00t! LGTM!

---

_@MichaReiser reviewed on 2024-03-06 08:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:531 on 2024-03-06 08:46_

Unfortunately, the `arrayvec` crate doesn't support the most complex operation, extending from an Iterator. That's why I think its use is limited. The main advantage I see is that it avoids initializing unused memory. 

---

_@MichaReiser reviewed on 2024-03-06 08:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/name.rs`:531 on 2024-03-06 08:55_

I gave it a quick try. Funny enough, it has a variance issue, but it manifests differently than smallvec 

---

_Merged by @MichaReiser on 2024-03-06 08:55_

---

_Closed by @MichaReiser on 2024-03-06 08:55_

---

_Branch deleted on 2024-03-06 08:56_

---
