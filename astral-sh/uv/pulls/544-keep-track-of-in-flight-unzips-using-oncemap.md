```yaml
number: 544
title: "Keep track of in flight unzips using `OnceMap`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: track-in-flight-unzips
created_at: 2023-12-04T14:28:43Z
updated_at: 2023-12-08T20:18:13Z
url: https://github.com/astral-sh/uv/pull/544
synced_at: 2026-01-10T15:44:44Z
```

# Keep track of in flight unzips using `OnceMap`

---

_Pull request opened by @konstin on 2023-12-04 14:28_

I saw warnings when we were e.g. unzipping wheel and setuptools in two tasks at the same time. We now keep track of in flight unzips.

This introduces a `OnceMap` abstraction which we also use in the resolver. 

---

_@konstin reviewed on 2023-12-04 14:30_

---

_Review comment by @konstin on `crates/puffin-dispatch/src/lib.rs`:40 on 2023-12-04 14:30_

This looks like a use case for a parallel datastructure, but otoh this is more than fine for the couple of hundreds unzips with have maximum.

---

_@konstin reviewed on 2023-12-04 14:30_

---

_Review comment by @konstin on `crates/puffin-installer/src/unzipper.rs`:39 on 2023-12-04 14:30_

@charliermarsh Do we want to run the unzips in parallel or is there a reason why we don't?

---

_@charliermarsh reviewed on 2023-12-04 14:32_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:39 on 2023-12-04 14:32_

We do one unzip at a time, but the unzips themselves are parallelized. I believe I profiled parallelizing the unzips (there are some issues around this) and it showed no improvement.

---

_@charliermarsh reviewed on 2023-12-04 14:33_

Does this work across recursive resolutions, and between the top-level resolution and any source distribution resolutions?

---

_@charliermarsh reviewed on 2023-12-04 14:34_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:49 on 2023-12-04 14:34_

(Nit: I'd prefer this below `impl Unzipper`, so that the struct and its implementation are uninterrupted and at the top of the file.)

---

_Comment by @charliermarsh on 2023-12-04 14:38_

Is this one instance of a more general problem? Where else might we run into this? E.g., when building source distributions, or from Git? Can we come up with a more general solution?

---

_Comment by @konstin on 2023-12-04 14:47_

> Does this work across recursive resolutions, and between the top-level resolution and any source distribution resolutions?

yes

> Is this one instance of a more general problem? Where else might we run into this? E.g., when building source distributions, or from Git? Can we come up with a more general solution?

This affects all operation through `BuildDispatch`, we can add the same locks around all operations that might run in parallel:
* `BuildDispatch::build_source`
https://github.com/astral-sh/puffin/blob/9806901a1603d062c70c14e523ae9058bdb32675/crates/puffin-dispatch/src/lib.rs#L233-L238
* `DistFinder` (or refactor to pass in distributions from resolve, so we don't have to do query something we just queried)
https://github.com/astral-sh/puffin/blob/9806901a1603d062c70c14e523ae9058bdb32675/crates/puffin-dispatch/src/lib.rs#L152
* `DistributionDatabase`
https://github.com/astral-sh/puffin/blob/9806901a1603d062c70c14e523ae9058bdb32675/crates/puffin-dispatch/src/lib.rs#L164

---

_Comment by @charliermarsh on 2023-12-04 15:07_

What about, e.g., fetching metadata and storing it in the cache? Will that work safely if run in parallel for the same package?

---

_Comment by @konstin on 2023-12-04 15:08_

Current dependencies on/for this PR:
* main
  * **PR #543** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/543?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #545** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/545?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #548** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/548?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
      * **PR #544** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/544?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/544?utm_source=stack-comment).

---

_@charliermarsh reviewed on 2023-12-04 15:13_

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:33 on 2023-12-04 15:13_

Should we try to add a struct around this? All the types feel like implementation details. I'd prefer something with a dedicated entry-like API.

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:33 on 2023-12-04 15:13_

Not that the API is so great, but I had something like this in the Resolvo branch, where the wait maps and in-flight structs are internal details of the API: https://github.com/astral-sh/puffin/pull/404/files#diff-6570ee60ef7249b15819a9796189f1098aaf2e0d8f2e3639fa6261a1f10a7401R143

---

_@charliermarsh reviewed on 2023-12-04 15:13_

---

_Comment by @konstin on 2023-12-04 16:10_

> What about, e.g., fetching metadata and storing it in the cache? Will that work safely if run in parallel for the same package?

Safely: Yes, fixed the remaining issues in (#546). We can optimize this by merging the `ResolverProvider` from #517 into the `BuildContext`, so the cached client is shared between recursive resolutions. Is that a change you're interested in?

---

_Renamed from "Keep track of in flight unzips" to "Keep track of in flight unzips using `OnceMap`" by @konstin on 2023-12-06 14:15_

---

_@konstin reviewed on 2023-12-06 14:16_

---

_Review comment by @konstin on `crates/puffin-distribution/src/download.rs`:71 on 2023-12-06 14:16_

There were two impl blocks while we only need one

---

_Comment by @konstin on 2023-12-06 14:22_

> Should we try to add a struct around this? All the types feel like implementation details. I'd prefer something with a dedicated entry-like API.

I introduced `OnceMap`, applying it in the resolver. I can split the 2 commits (in flight unzips and `OnceMap` in resolver) into two PRs if it makes more sense.

![image](https://github.com/astral-sh/puffin/assets/6826232/2e1389e4-bb1c-4fce-a7e2-f63080091b08)



---

_@charliermarsh reviewed on 2023-12-06 16:44_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:462 on 2023-12-06 16:44_

I don't love that the insertions are all now async, is that strictly required?

---

_Review comment by @charliermarsh on `crates/puffin-installer/src/unzipper.rs`:91 on 2023-12-06 16:44_

Should we move this outside of `unzip`? Avoid going through the whole `tokio::task::spawn_blocking` just for this?

---

_@charliermarsh reviewed on 2023-12-06 16:44_

---

_@konstin reviewed on 2023-12-06 16:55_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:462 on 2023-12-06 16:55_

Same, but the locks need to be std locks or we break a send guarantee we need

---

_@charliermarsh reviewed on 2023-12-06 17:01_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver.rs`:462 on 2023-12-06 17:01_

Previously we didn't have any locks around the inflight stuff (I think?), was that a mistake?

---

_@charliermarsh reviewed on 2023-12-06 17:40_

---

_Review comment by @charliermarsh on `crates/puffin-traits/src/in_flight.rs`:17 on 2023-12-06 17:40_

`FxHashSet`, to match the existing implementation?

---

_Review comment by @charliermarsh on `crates/puffin-traits/src/in_flight.rs`:58 on 2023-12-06 17:40_

It could be worth taking `&K` here and only cloning if the item isn't already in the set.

---

_@charliermarsh reviewed on 2023-12-06 17:40_

---

_@konstin reviewed on 2023-12-08 09:14_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver.rs`:462 on 2023-12-08 09:14_

It worked because we had the waitmap and the in flight hashset split, passing the hashmap around as `&mut _`. I don't think we can replicate this while using an abstraction over this.

---

_@konstin reviewed on 2023-12-08 09:35_

---

_Review comment by @konstin on `crates/puffin-traits/src/in_flight.rs`:58 on 2023-12-08 09:35_

Getting fancy with type signatures
```rust
    pub async fn register<Q>(&self, key: &Q) -> bool
    where
        K: Borrow<Q>,
        Q: ?Sized + Hash + Eq + ToOwned<Owned = K>,
```

---

_Review comment by @charliermarsh on `crates/puffin-traits/src/in_flight.rs`:15 on 2023-12-08 14:53_

I find it odd that this is in `puffin-traits` but I don't know of a better home :)

---

_@charliermarsh approved on 2023-12-08 14:53_

---

_Merged by @charliermarsh on 2023-12-08 20:18_

---

_Closed by @charliermarsh on 2023-12-08 20:18_

---

_Branch deleted on 2023-12-08 20:18_

---
