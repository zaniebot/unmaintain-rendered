```yaml
number: 3627
title: Parallelize Resolver
type: pull_request
state: merged
author: ibraheemdev
labels:
  - performance
assignees: []
merged: true
base: main
head: prefetch-spawn
created_at: 2024-05-16T16:09:20Z
updated_at: 2024-05-17T16:26:46Z
url: https://github.com/astral-sh/uv/pull/3627
synced_at: 2026-01-10T14:32:20Z
```

# Parallelize Resolver

---

_Pull request opened by @ibraheemdev on 2024-05-16 16:09_

## Summary

This PR introduces parallelism to the resolver. Specifically, we can perform PubGrub resolution on a separate thread, while keeping all I/O on the tokio thread. We already have the infrastructure set up for this with the channel and `OnceMap`, which makes this change relatively simple. The big change needed to make this possible is removing the lifetimes on some of the types that need to be shared between the resolver and pubgrub thread. 

A related PR, https://github.com/astral-sh/uv/pull/1163, found that adding `yield_now`  calls improved throughput. With optimal scheduling we might be able to get away with everything on the same thread here. However, in the ideal pipeline with perfect prefetching, the resolution and prefetching can run completely in parallel without depending on one another. While this would be very difficult to achieve, even with our current prefetching pattern we see a consistent performance improvement from parallelism.

This does also require reverting a few of the changes from https://github.com/astral-sh/uv/pull/3413, but not all of them. The sharing is isolated to the resolver task.

## Test Plan

On smaller tasks performance is mixed with ~2% improvements/regressions on both sides. However, on medium-large resolution tasks we see the benefits of parallelism, with improvements anywhere from 10-50%.

```
./scripts/requirements/jupyter.in
Benchmark 1: ./target/profiling/baseline (resolve-warm)
  Time (mean ± σ):      29.2 ms ±   1.8 ms    [User: 20.3 ms, System: 29.8 ms]
  Range (min … max):    26.4 ms …  36.0 ms    91 runs
 
Benchmark 2: ./target/profiling/parallel (resolve-warm)
  Time (mean ± σ):      25.5 ms ±   1.0 ms    [User: 19.5 ms, System: 25.5 ms]
  Range (min … max):    23.6 ms …  27.8 ms    99 runs
 
Summary
  ./target/profiling/parallel (resolve-warm) ran
    1.15 ± 0.08 times faster than ./target/profiling/baseline (resolve-warm)
```
```
./scripts/requirements/boto3.in   
Benchmark 1: ./target/profiling/baseline (resolve-warm)
  Time (mean ± σ):     487.1 ms ±   6.2 ms    [User: 464.6 ms, System: 61.6 ms]
  Range (min … max):   480.0 ms … 497.3 ms    10 runs
 
Benchmark 2: ./target/profiling/parallel (resolve-warm)
  Time (mean ± σ):     430.8 ms ±   9.3 ms    [User: 529.0 ms, System: 77.2 ms]
  Range (min … max):   417.1 ms … 442.5 ms    10 runs
 
Summary
  ./target/profiling/parallel (resolve-warm) ran
    1.13 ± 0.03 times faster than ./target/profiling/baseline (resolve-warm)
```
```
./scripts/requirements/airflow.in 
Benchmark 1: ./target/profiling/baseline (resolve-warm)
  Time (mean ± σ):     478.1 ms ±  18.8 ms    [User: 482.6 ms, System: 205.0 ms]
  Range (min … max):   454.7 ms … 508.9 ms    10 runs
 
Benchmark 2: ./target/profiling/parallel (resolve-warm)
  Time (mean ± σ):     308.7 ms ±  11.7 ms    [User: 428.5 ms, System: 209.5 ms]
  Range (min … max):   287.8 ms … 323.1 ms    10 runs
 
Summary
  ./target/profiling/parallel (resolve-warm) ran
    1.55 ± 0.08 times faster than ./target/profiling/baseline (resolve-warm)
```

---

_Marked ready for review by @ibraheemdev on 2024-05-16 16:17_

---

_Comment by @charliermarsh on 2024-05-16 16:21_

Very cool!

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-16 16:21_

---

_Review requested from @konstin by @charliermarsh on 2024-05-16 16:21_

---

_Comment by @charliermarsh on 2024-05-16 16:21_

Tagging @konstin and @BurntSushi to review.

---

_Label `performance` added by @charliermarsh on 2024-05-16 16:21_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:6 on 2024-05-16 17:16_

Why the switch to an unbounded channel? I'm not usually a fan of these because it prevents back-pressure. Maybe use `crossbeam-channel` instead?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:16 on 2024-05-16 17:16_

Same as above here. Can we use bounded channels?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:196 on 2024-05-16 17:19_

What's the thinking behind splitting this type out? Maybe some code comments would help here.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:339 on 2024-05-16 17:20_

This doesn't seem sound to me? Is there an alternative approach here that doesn't require `unsafe`? A `transmute`, in my experience, is typically quite tricky to get right. Especially when erasing away a lifetime.

I wonder if it makes sense to first focus on removing the lifetime from `Solver`/`Resolver`. I imagine that would simplify things here.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:384 on 2024-05-16 17:23_

Can this comment be unpacked a bit more? Also, which bounded request channel is this referring to?

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:596 on 2024-05-16 17:26_

What is the difference/relationship between this type and `Resolver` and `SolverState`? I'm bouncing between them trying to find a difference. I see `Resolver` is generic over a `Provider` where this is not. Some code comments explaining what this type is and why it exists would help I think. (I think the code comments should, at minimum, answer the question of why there is duplication here.)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:339 on 2024-05-16 17:31_

Ah okay nice, that is exactly what you did in subsequent commits. Perfect. My bad for reviewing commit-by-commit. At a glance, it looked like that might be helpful given the size of the PR, but it ended up confusing me.

---

_@BurntSushi reviewed on 2024-05-16 17:34_

This is pretty awesome. I think this largely makes sense to me overall. I am a little concerned about the switch to unbounded channels/streams though. I convinced us a while back to switch _from_ unbounded channels _to_ bounded channels. I believe this was my argument: https://github.com/astral-sh/uv/pull/1163#discussion_r1473155361

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:596 on 2024-05-16 17:43_

The `Resolver` includes state that is required by the `fetch` task. Because it's also the constructor for the `Solver`, it needs to hold the solver state initially in the `Option<SolverState>`. In `resolve` (which consumes the `Resolver`), that state is then taken and given to the `Solver` task, which is run in the thread. This avoids cloning items that don't actually need to be shared.

Thinking about it now.. I think the `Resolver` can just construct the `Solver` in `new`.

---

_@ibraheemdev reviewed on 2024-05-16 17:43_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:384 on 2024-05-16 17:44_

This is an old comment, I didn't touch any of the `fetch` code. It's referring to the channel between the prefetcher and solver, I'll update it to make it clearer.

---

_@ibraheemdev reviewed on 2024-05-16 17:44_

---

_@ibraheemdev reviewed on 2024-05-16 17:45_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/mod.rs`:339 on 2024-05-16 17:45_

Yeah, I initially did this just to test out the performance affects without having to make all the lifetime changes. Didn't mean for it to be merged in this state :sweat_smile: 

---

_@ibraheemdev reviewed on 2024-05-16 17:48_

---

_Review comment by @ibraheemdev on `crates/uv-resolver/src/resolver/batch_prefetch.rs`:6 on 2024-05-16 17:48_

I can switch back to bounded channels, tokio has a `blocking_send` method. Now that things are running in parallel I don't see a huge benefit of the solver blocking on the prefetcher unless it's necessary, but I agree that having some backpressure is a good idea.

---

_Review requested from @BurntSushi by @ibraheemdev on 2024-05-16 19:09_

---

_Comment by @zanieb on 2024-05-17 02:05_

Love to see a 50% improvement :)

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:19 on 2024-05-17 02:07_

Just for my knowledge, is this the typical naming scheme for this pattern?


---

_@zanieb reviewed on 2024-05-17 02:07_

---

_Comment by @konstin on 2024-05-17 09:19_

Amazing work!

Do you know why it gets so much faster, i.e. how the solver is blocking? I've been looking at the spans, but i don't really understand why the prefetches don't get queued anyway on main.

Airflow, main vs. PR:

![main](https://github.com/astral-sh/uv/assets/6826232/b7c2de41-d4e8-45e1-b323-b2a23e053b09)

![PR](https://github.com/astral-sh/uv/assets/6826232/21ab014e-bfdb-4b52-89e4-5171ddcca6f7)



---

Here's some perf number from my machine:

```
jupyter:
  Time (mean ± σ):      15.9 ms ±   1.3 ms    [User: 14.1 ms, System: 21.0 ms]
  Time (mean ± σ):      15.4 ms ±   1.1 ms    [User: 14.1 ms, System: 20.1 ms]
boto3:
  Time (mean ± σ):     383.5 ms ±   3.3 ms    [User: 343.5 ms, System: 60.8 ms]
  Time (mean ± σ):     325.3 ms ±   3.8 ms    [User: 351.7 ms, System: 62.0 ms]
airflow:
  Time (mean ± σ):     192.6 ms ±   3.9 ms    [User: 172.5 ms, System: 151.3 ms]
  Time (mean ± σ):     147.0 ms ±   3.3 ms    [User: 168.4 ms, System: 146.6 ms]
```

---

_@konstin approved on 2024-05-17 09:29_

---

_@BurntSushi reviewed on 2024-05-17 11:32_

---

_Review comment by @BurntSushi on `crates/uv-interpreter/src/environment.rs`:19 on 2024-05-17 11:32_

I usually use `Foo` and `FooInner` personally. I don't think I've seen `SharedFoo` much? I like adding a suffix personally. So I'd prefer `FooShared` (or whatever). But I don't have a strong opinion.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/index.rs`:20 on 2024-05-17 13:10_

Since the outer type isn't `pub(crate)`, should the fields be? I think it should be okay to un-export them.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/index.rs`:30 on 2024-05-17 13:10_

I think the comment needs updating. (And the one above too.)

---

_@BurntSushi approved on 2024-05-17 13:12_

Love it. I like the lifetime removal a lot too. Nice work.

---

_@zanieb reviewed on 2024-05-17 13:16_

---

_Review comment by @zanieb on `crates/uv-interpreter/src/environment.rs`:19 on 2024-05-17 13:16_

Cool thanks! `Inner` makes a bit more sense to me.

---

_Comment by @ibraheemdev on 2024-05-17 15:35_

@konstin The elapsed user time doesn't change up, and if you look at the profile for the resolver thread you'll see a lot of time spent in pubgrub, which suggests that the prefetches may have been queued on the single-threaded version but we simply didn't have enough time to get to them, or if we did they took away from the solver. My hunch is that the solver and prefetcher were fighting for time slices.

---

_@ibraheemdev reviewed on 2024-05-17 15:38_

---

_Review comment by @ibraheemdev on `crates/uv-interpreter/src/environment.rs`:19 on 2024-05-17 15:38_

I sort of avoid `Inner` because it feels like a catch-all naming convention. A suffix seems slightly better for readability, I'll switch to that.

---

_Merged by @ibraheemdev on 2024-05-17 15:47_

---

_Closed by @ibraheemdev on 2024-05-17 15:47_

---
