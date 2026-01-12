```yaml
number: 947
title: "Reduce stack usage by boxing `File` in `Dist`, `CachePolicy` and large futures"
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: konsti/set-explicit-thread-size
head: konsti/reduce-stack-usage
created_at: 2024-01-17T13:15:52Z
updated_at: 2024-01-19T09:29:11Z
url: https://github.com/astral-sh/uv/pull/947
synced_at: 2026-01-12T16:04:18Z
```

# Reduce stack usage by boxing `File` in `Dist`, `CachePolicy` and large futures

---

_@konstin_

Windows has a default stack size of 1MB, which makes puffin often fail with stack overflows. The PR reduces stack size by three changes:

* Boxing `File` in `Dist`, reducing the size from 496 to 240.
* Boxing the largest futures.
* Boxing `CachePolicy`

## Method

Debugging happened on linux using https://github.com/astral-sh/puffin/pull/941 to limit the stack size to 1MB. Used ran the command below.

```
RUSTFLAGS=-Zprint-type-sizes cargo +nightly build -p puffin-cli -j 1 > type-sizes.txt && top-type-sizes -w -s -h 10 < type-sizes.txt > sizes.txt
```

The main drawback is top-type-sizes not saying what the `__awaitee` is, so it requires manually looking up with a future with matching size.

When the `brotli` features on `reqwest` is active, a lot of brotli types show up. Toggling this feature however seems to have no effect. I assume they are false positives since the `brotli` crate has elaborate control about allocation. The sizes are therefore shown with the feature off.

## Results

The largest future goes from 12208B to 6416B, the largest type (`PrioritizedDistribution`, see also #948) from 17448B to 9264B. Full diff: https://gist.github.com/konstin/62635c0d12110a616a1b2bfcde21304f

For the second commit, i iteratively boxed the largest file until the tests passed, then with an 800KB stack limit looked through the backtrace of a failing test and added some more boxing.

Quick benchmarking showed no difference:

```console
$ hyperfine --warmup 2 "target/profiling/main-dev resolve meine_stadt_transparent" "target/profiling/puffin-dev resolve meine_stadt_transparent" 
Benchmark 1: target/profiling/main-dev resolve meine_stadt_transparent
  Time (mean ± σ):      49.2 ms ±   3.0 ms    [User: 39.8 ms, System: 24.0 ms]
  Range (min … max):    46.6 ms …  63.0 ms    55 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Benchmark 2: target/profiling/puffin-dev resolve meine_stadt_transparent
  Time (mean ± σ):      47.4 ms ±   3.2 ms    [User: 41.3 ms, System: 20.6 ms]
  Range (min … max):    44.6 ms …  60.5 ms    62 runs
 
  Warning: Statistical outliers were detected. Consider re-running this benchmark on a quiet system without any interferences from other programs. It might help to use the '--warmup' or '--prepare' options.
 
Summary
  target/profiling/puffin-dev resolve meine_stadt_transparent ran
    1.04 ± 0.09 times faster than target/profiling/main-dev resolve meine_stadt_transparent
```

---

_Review requested from @BurntSushi by @konstin on 2024-01-17 13:15_

---

_Review requested from @charliermarsh by @konstin on 2024-01-17 13:15_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/lib.rs`:207 on 2024-01-17 13:23_

Do you think it's worth having these? I have a few tests like this in various crates, but I usually regret them because they can change easily. (Whenever the type definitions or changed, or even compiling on different targets with a different pointer size.) What I usually try to do instead is express them as an inequality. That is, we probably don't care if any these types get _smaller_ in size, but maybe we want to be flagged if they get bigger. Still, even then, I'd use these assertions sparingly personally.

---

_@BurntSushi reviewed on 2024-01-17 13:26_

What do you think about boxing `BuiltDist` and `SourceDist` instead? That should bring the size of `Dist` down considerably. You can even leave the box on `File`.

At least for the boxes inside types like `File`, those look like marginal allocations to me. (That is, `File` already has a bunch of tiny little allocations in it.) The main other possible impact is the overall effect of an extra pointer dereference, but usually that has to be in some very critical section to have much of an effect. But still a good idea to run benchmarks.

---

_@konstin reviewed on 2024-01-17 13:43_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:207 on 2024-01-17 13:43_

Any recommendations how to express just that `Dist` shouldn't become larger, at least not without somebody explicitly raising the limit? `static_assertions` doesn't have a macro for that.

---

_@BurntSushi reviewed on 2024-01-17 13:46_

---

_Review comment by @BurntSushi on `crates/distribution-types/src/lib.rs`:207 on 2024-01-17 13:46_

I wouldn't bother with `static_assertions` for this personally. I would write a normal unit test with `assert!(std::mem::size_of::<Dist>() <= N)`.

---

_@charliermarsh reviewed on 2024-01-17 14:16_

---

_Review comment by @charliermarsh on `crates/distribution-types/src/lib.rs`:207 on 2024-01-17 14:16_

The PR summary says Dist was reduced to 420 bytes. Is that a typo in the summary?

---

_@konstin reviewed on 2024-01-17 14:22_

---

_Review comment by @konstin on `crates/distribution-types/src/lib.rs`:207 on 2024-01-17 14:22_

Yes, fixed

---

_Comment by @konstin on 2024-01-17 17:26_

> What do you think about boxing BuiltDist and SourceDist instead? That should bring the size of Dist down considerably. You can even leave the box on File.

This way ...
```rust
#[derive(Debug, Clone)]
pub enum Dist {
    Built(Box<BuiltDist>),
    Source(Box<SourceDist>),
}
```
... or this ways?
```rust
#[derive(Debug, Clone)]
pub struct Dist(Box<DistInner>);

#[derive(Debug, Clone)]
pub enum DistInner {
    Built(BuiltDist),
    Source(SourceDist),
}
```

It changes all our `Dist` matches, but if we have consensus i'll do it.

---

_Comment by @BurntSushi on 2024-01-17 18:52_

Hmmm yes I see the conundrum. Given those choices, and given the fact that you can't pattern match through a `Box` (bah), I'd probably go with the `DistInner` approach since `BuiltDist` and `SourceDist` are themselves both enums.

But... Looking at the types again, I think what I might do instead is:

1. Make all struct types opaque. So that's `RegistryBuiltDist`, `DirectUrlBuiltDist`, `PathBuiltDist`, `RegistrySourceDist`, `DirectUrlSourceDist`, `GitSourceDist` and `PathSourceDist`.
2. Expose their fields as public getter methods.
3. For all the types above, add a box inside of them, probably requiring an `Inner` type for each.

This way, I think for the most part pattern matching won't be as annoying/awkward. Some might need to change in places where we match on the struct fields, but it should be pretty straight-forward to tweak those.

With that said, this is a larger and annoying refactor. So if this PR solves the immediate issue, I'd be happy punting on this for now.

---

_Renamed from "Reduce stack usage by boxing `File` in `Dist` and large futures" to "Reduce stack usage by boxing `File` in `Dist`, `CachePolicy` and large futures" by @konstin on 2024-01-18 11:50_

---

_Comment by @konstin on 2024-01-18 11:53_

Yeah i'd prefer to move ahead without refactoring `Dist` more, the payoff from that refactor looks much smaller than the other boxing.

---

_@BurntSushi approved on 2024-01-18 13:51_

---

_Merged by @konstin on 2024-01-19 09:29_

---

_Closed by @konstin on 2024-01-19 09:29_

---

_Branch deleted on 2024-01-19 09:29_

---
