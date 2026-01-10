```yaml
number: 948
title: "Box `PrioritizedDistribution`"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: konsti/box-prioritized-dist
created_at: 2024-01-17T13:45:01Z
updated_at: 2024-01-19T09:44:42Z
url: https://github.com/astral-sh/uv/pull/948
synced_at: 2026-01-10T15:39:02Z
```

# Box `PrioritizedDistribution`

---

_Pull request opened by @konstin on 2024-01-17 13:45_

On top of https://github.com/astral-sh/puffin/pull/947, we can also box `PrioritizedDistribution`.

In a simple benchmark, this seems to slightly improve performance when comparing only this commit to main, even though the benchmark is too noisy to establish significance:

```
$ hyperfine --warmup 30 --runs 300 "target/profiling/main-dev resolve meine_stadt_transparent" "target/profiling/puffin-dev resolve meine_stadt_transparent"
  Benchmark 1: target/profiling/main-dev resolve meine_stadt_transparent
    Time (mean ± σ):      83.6 ms ±   2.0 ms    [User: 77.7 ms, System: 20.0 ms]
    Range (min … max):    81.4 ms …  98.2 ms    300 runs

    Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

  Benchmark 2: target/profiling/puffin-dev resolve meine_stadt_transparent
    Time (mean ± σ):      80.8 ms ±   2.2 ms    [User: 75.4 ms, System: 19.5 ms]
    Range (min … max):    78.6 ms …  98.6 ms    300 runs

    Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.

  Summary
    target/profiling/puffin-dev resolve meine_stadt_transparent ran
      1.03 ± 0.04 times faster than target/profiling/main-dev resolve meine_stadt_transparent
```

The effect on type sizes however is considerable ([downstack PR](https://gist.github.com/konstin/38e6c774db541db46d61f1d4ea6b498f) vs. [this PR](https://gist.github.com/konstin/003a77fe7d7d246b0d535e3fc843cb36)):

```patch
--- branch.txt  2024-01-17 14:26:01.826085176 +0100
+++ boxed-prioritized-dist.txt  2024-01-17 14:25:57.101900963 +0100
@@ -1,19 +1,3 @@
-9264 alloc::collections::btree::node::InternalNode<pep440_rs::version::Version, distribution_types::PrioritizedDistribution> align=8
-   9168 data
-     96 edges
-
-9264 alloc::collections::btree::node::InternalNode<pep440_rs::Version, distribution_types::PrioritizedDistribution> align=8
-   9168 data
-     96 edges
-
-9168 alloc::collections::btree::node::LeafNode<pep440_rs::version::Version, distribution_types::PrioritizedDistribution> align=8
-   9064 vals
-     88 keys
-
-9168 alloc::collections::btree::node::LeafNode<pep440_rs::Version, distribution_types::PrioritizedDistribution> align=8
-   9064 vals
-     88 keys
-
 8992 tokio::sync::mpsc::block::Block<hyper::client::dispatch::Envelope<http::request::Request<reqwest::async_impl::body::ImplStream>, http::response::Response<hyper::body::body::Body>>> align=8
    8960 values
      32 header
@@ -74,10 +58,23 @@
          40 __tracing_attr_span
      64 variant Unresumed, Returned, Panicked

+5648 {async fn body@crates/puffin-client/src/registry_client.rs:224:5: 224:30} align=8
+   5647 variant Suspend0
+       5576 __awaitee align=8
+         40 __tracing_attr_span
```

---

_@BurntSushi reviewed on 2024-01-17 13:49_

What about putting the `Box` inside of `PrioritizedDistribution`? Then consumers don't need to care about it.

---

_Comment by @BurntSushi on 2024-01-17 13:50_

Oh, the other concern I had here was on the boxed futures. It seems like it might be easy for those `boxed()` calls to disappear accidentally. Maybe just a quick comment on each one to say `// reduces stack size` or something?

Also, how did you pick the futures to box?

---

_Comment by @konstin on 2024-01-17 13:56_

> What about putting the Box inside of PrioritizedDistribution? Then consumers don't need to care about it.

Good question, my thinking was that this still allows using the plain `PrioritizedDistribution` if you don't want the `Box` without enforcing the allocation?

> Also, how did you pick the futures to box?

I went from the largest future to their callees until i arrived at one that was large not due to it's callee but due to variant/data and then boxed this one at all of its callsites. It's not an exact methodology, but it seems to have worked. I'm still having trouble understanding the numbers from top-type-sizes and the exact construction of future sizes.

I had also considered splitting some of the large methods into helper methods for easier debugging and maybe more boxing spots but wanted to avoid the churn.

---

_Comment by @BurntSushi on 2024-01-17 14:12_

> Good question, my thinking was that this still allows using the plain `PrioritizedDistribution` if you don't want the `Box` without enforcing the allocation?

Right. But I don't think it matters. A `PrioritizedDistribution` is a huge type with a whole mess of things inside of it, including _many_ allocations. Adding one more is what I think of as "marginal." As in, it's unlikely to make a meaningful difference given its definition today.

> I went from the largest future to their callees until i arrived at one that was large not due to it's callee but due to variant/data and then boxed this one at all of its callsites. It's not an exact methodology, but it seems to have worked. I'm still having trouble understanding the numbers from top-type-sizes and the exact construction of future sizes.

Gotya. Seems sensible to me I think. It does seem a little tricky. (And sorry, I added the comment about boxed futures on the wrong PR.)

---

_@BurntSushi approved on 2024-01-18 13:49_

---

_Merged by @konstin on 2024-01-19 09:44_

---

_Closed by @konstin on 2024-01-19 09:44_

---

_Branch deleted on 2024-01-19 09:44_

---
