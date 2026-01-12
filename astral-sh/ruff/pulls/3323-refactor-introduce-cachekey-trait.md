```yaml
number: 3323
title: "refactor: Introduce `CacheKey` trait"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: cache-key
created_at: 2023-03-03T17:25:13Z
updated_at: 2023-03-03T18:29:51Z
url: https://github.com/astral-sh/ruff/pull/3323
synced_at: 2026-01-12T04:39:44Z
```

# refactor: Introduce `CacheKey` trait

---

_Pull request opened by @MichaReiser on 2023-03-03 17:25_

This PR introduces a new `CacheKey` trait for types that can be used as a cache key.

I'm not entirely sure if this is worth the "overhead", but I was surprised to find `HashableHashSet` and got scared when I looked at the time complexity of the `hash` function. These implementations must be extremely slow in hashed collections.

I then searched for usages and quickly realized that only the cache uses these `Hash` implementations, where performance is less sensitive.

This PR introduces a new `CacheKey` trait to communicate the difference between a hash and computing a key for the cache. The new trait can be implemented for types that don't implement `Hash` for performance reasons, and we can define additional constraints on the implementation:  For example, we'll want to enforce portability when we add remote caching support. Using a different trait further allows us not to implement it for types without stable identities (e.g. pointers) or use other implementations than the standard hash function.

## Test Plan

Ran 

```bash
 time ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache
 time ./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache 
```

To see if the second run runs faster and produces the same warnings. 



---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:102 on 2023-03-03 17:26_

TODO: Come up with a better name

---

_Review comment by @MichaReiser on `crates/ruff_macros/src/cache_key.rs`:103 on 2023-03-03 17:26_

Add tests

---

_@MichaReiser reviewed on 2023-03-03 17:27_

---

_Marked ready for review by @MichaReiser on 2023-03-03 18:08_

---

_Comment by @charliermarsh on 2023-03-03 18:09_

I like this!

---

_Review comment by @charliermarsh on `crates/ruff_cache/Cargo.toml`:7 on 2023-03-03 18:11_

I've been using this pattern in the other internal-only crates:

```toml
[package]
name = "ruff_cache"
version = "0.0.0"
publish = false
edition = { workspace = true }
rust-version = { workspace = true }
```

Thoughts?

---

_@charliermarsh reviewed on 2023-03-03 18:11_

---

_Review comment by @charliermarsh on `crates/ruff_cache/src/globset.rs`:4 on 2023-03-03 18:12_

I guess in some sense it could make sense for these implementations to go in (e.g.) `ruff`, or whichever crate actually needs to cache these types, but I think we can't do that due to the orphan rule?

---

_@charliermarsh reviewed on 2023-03-03 18:12_

---

_@MichaReiser reviewed on 2023-03-03 18:14_

---

_Review comment by @MichaReiser on `crates/ruff_cache/src/globset.rs`:4 on 2023-03-03 18:14_

Yeah, orphan rule strikes again ;) 

I thought about adding feature flags, but it felt overkill to add it for so little code. 

This is probably the main downside of this approach. `Hash` is implemented by many libraries. Implementing `CacheKey` for non-std types requires adding all of them to `ruff_cache` OR introducing new type wrappers. 

---

_@MichaReiser reviewed on 2023-03-03 18:14_

---

_Review comment by @MichaReiser on `crates/ruff_cache/Cargo.toml`:7 on 2023-03-03 18:14_

I should not have used the CLion standard template...

---

_@charliermarsh reviewed on 2023-03-03 18:15_

---

_Review comment by @charliermarsh on `crates/ruff_cache/src/globset.rs`:4 on 2023-03-03 18:15_

Yeah. We could add a flag for each "external library", right? Like this crate could have a `globset` flag?

I think it's fine. For the reasons you described, it's actually a good thing that we have to intentionally implement `CacheKey`. We should be wary of putting complex types in settings anyway, since they have to be user-editable, serializable, etc.

---

_@charliermarsh approved on 2023-03-03 18:16_

---

_Merged by @MichaReiser on 2023-03-03 18:29_

---

_Closed by @MichaReiser on 2023-03-03 18:29_

---

_Branch deleted on 2023-03-03 18:29_

---
