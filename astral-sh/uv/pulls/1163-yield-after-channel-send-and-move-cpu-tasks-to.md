```yaml
number: 1163
title: Yield after channel send and move cpu tasks to thread
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/yield-after-send
created_at: 2024-01-29T10:37:37Z
updated_at: 2024-02-02T17:18:26Z
url: https://github.com/astral-sh/uv/pull/1163
synced_at: 2026-01-12T16:04:28Z
```

# Yield after channel send and move cpu tasks to thread

---

_@konstin_

## Summary

Previously, we were blocking operations that could run in parallel. We would send request through our main requests channel, but not yield so that the receiver could only start processing requests much later than necessary. We solve this by switching to the async `tokio::sync::mpsc::channel`, where send is an async functions that yields.

Due to the increased parallelism cache deserialization and the conversion from simple api request to version map became bottlenecks, so i moved them to `spawn_blocking`. Together these result in a 30-60% speedup for larger warm cache resolution. Small cases such as black already resolve in 5.7 ms on my machine so there's no speedup to be gained, refresh and no cache were to noisy to get signal from.

Note for the future: Revisit the bounded channel if we want to produce requests from `process_request`, too, (this would be good for prefetching) to avoid deadlocks.

## Details

We can look at the behavior change through the spans:

```
RUST_LOG=puffin=info TRACING_DURATIONS_FILE=target/traces/jupyter-warm-branch.ndjson cargo run --features tracing-durations-export --bin puffin-dev --profile profiling -- resolve jupyter 2> /dev/null
```

Below, you can see how on main, we have discrete phases: All (cached) simple api requests in parallel, then all (cached) metadata requests in parallel, repeat until done. The solver is mostly waiting until it has it's version map from the simple API query to be able to choose a version. The main thread is blocked by process requests.

In the PR branch, the simple api requests succeeds much earlier, allowing the solver to advance and also to schedule more prefetching. Due to that `parse_cache` and `from_metadata` became bottlenecks, so i moved them off the main thread (green color, and their spans can now overlap because they can run on multiple threads in parallel). The main thread isn't blocked on `process_request` anymore, instead it has frequent idle times. The spans are all much shorter, which indicates that on main they could have finished much earlier, but a task didn't yield so they weren't scheduled to finish (though i haven't dug deep enough to understand the exact scheduling between the process request stream and the solver here).

**main**

![jupyter-warm-main](https://github.com/astral-sh/puffin/assets/6826232/693c53cc-1090-41b7-b02a-a607fcd2cd99)

**PR**

![jupyter-warm-branch](https://github.com/astral-sh/puffin/assets/6826232/33435f34-b39b-4b0a-a9d7-4bfc22f55f05)

## Benchmarks

```
$ hyperfine --warmup 3 "target/profiling/main-dev resolve jupyter" "target/profiling/branch-dev resolve jupyter"
Benchmark 1: target/profiling/main-dev resolve jupyter
  Time (mean ± σ):      29.1 ms ±   0.7 ms    [User: 22.9 ms, System: 11.1 ms]
  Range (min … max):    27.7 ms …  32.2 ms    103 runs
 
Benchmark 2: target/profiling/branch-dev resolve jupyter
  Time (mean ± σ):      18.8 ms ±   1.1 ms    [User: 37.0 ms, System: 22.7 ms]
  Range (min … max):    16.5 ms …  21.9 ms    154 runs
 
Summary
  target/profiling/branch-dev resolve jupyter ran
    1.55 ± 0.10 times faster than target/profiling/main-dev resolve jupyter

$ hyperfine --warmup 3 "target/profiling/main-dev resolve meine_stadt_transparent" "target/profiling/branch-dev resolve meine_stadt_transparent"
Benchmark 1: target/profiling/main-dev resolve meine_stadt_transparent
  Time (mean ± σ):      37.8 ms ±   0.9 ms    [User: 30.7 ms, System: 14.1 ms]
  Range (min … max):    36.6 ms …  41.5 ms    79 runs
 
Benchmark 2: target/profiling/branch-dev resolve meine_stadt_transparent
  Time (mean ± σ):      24.7 ms ±   1.5 ms    [User: 47.0 ms, System: 39.3 ms]
  Range (min … max):    21.5 ms …  28.7 ms    113 runs
 
Summary
  target/profiling/branch-dev resolve meine_stadt_transparent ran
    1.53 ± 0.10 times faster than target/profiling/main-dev resolve meine_stadt_transparent

$ hyperfine --warmup 3 "target/profiling/main pip compile scripts/requirements/home-assistant.in" "target/profiling/branch pip compile scripts/requirements/home-assistant.in"
Benchmark 1: target/profiling/main pip compile scripts/requirements/home-assistant.in
  Time (mean ± σ):     229.0 ms ±   2.8 ms    [User: 197.3 ms, System: 63.7 ms]
  Range (min … max):   225.8 ms … 234.0 ms    13 runs
 
Benchmark 2: target/profiling/branch pip compile scripts/requirements/home-assistant.in
  Time (mean ± σ):      91.4 ms ±   5.3 ms    [User: 289.2 ms, System: 176.9 ms]
  Range (min … max):    81.0 ms … 104.7 ms    32 runs
 
Summary
  target/profiling/branch pip compile scripts/requirements/home-assistant.in ran
    2.50 ± 0.15 times faster than target/profiling/main pip compile scripts/requirements/home-assistant.in
```

---

_@charliermarsh reviewed on 2024-01-29 12:47_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-29 12:47_

Are you able to explain what this is doing / why it works?

---

_@konstin reviewed on 2024-01-29 13:14_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-29 13:14_

The channel sending is sync, so my theory is that the task doesn't yield until all the work for a certain section (e.g. (pre-)fetch all version maps) is done and thereby blocks the receiving end from taking over (the requests and the solver are both on the main thread as far as i can tell). I've confirmed that there is a gap between e.g. a prefetch request being sent and it being received/executed.

You can see this in the span blocks for warm cache jupyter on main:

![main](https://github.com/astral-sh/puffin/assets/6826232/c1a734fa-bee7-4459-84e9-880251848fb6)

We shouldn't have to wait till all the simple api requests are done to start the metadata requests.

In comparison, on this branch everything happens concurrently:

![Screenshot_from_2024-01-29_00-08-49.png](https://github.com/astral-sh/puffin/assets/6826232/a9a86084-ac75-4cf0-9da7-c2989c2c5026)

The spans are also shorter, which makes sense when they were previously only missing the chance to complete because another task was blocking the thread.

---

_@charliermarsh reviewed on 2024-01-29 13:20_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-29 13:20_

Great, thank you!

---

_@konstin reviewed on 2024-01-29 18:33_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-29 18:33_

I've updated the OP with better plots and a proper description how to read them.

---

_Comment by @konstin on 2024-01-30 11:38_

> [!WARNING]
> <b>This pull request is not mergeable via GitHub because a downstack PR is open. Once all requirements are satisfied, merge this PR as a stack <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1163?utm_source=stack-comment-downstack-mergeability-warning" >on Graphite</a>.</b>
> <a href="https://graphite.dev/docs/merge-pull-requests">Learn more</a>

Current dependencies on/for this PR:
* `main`
  * **PR #1183** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1183?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a> 
    * **PR #1163** <a href="https://app.graphite.dev/github/pr/astral-sh/puffin/1163?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/puffin/1163?utm_source=stack-comment).

---

_Marked ready for review by @konstin on 2024-01-30 12:24_

---

_Review requested from @BurntSushi by @konstin on 2024-01-30 12:25_

---

_Review requested from @charliermarsh by @konstin on 2024-01-30 12:25_

---

_Renamed from " Yield after channel send, causing waitmap deadlock" to "Yield after channel send and move cpu tasks to thread" by @konstin on 2024-01-30 12:30_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-30 14:35_

@konstin Can you move your lovely comment into the source here? Obviously the images won't be able to make it in, but even just the text would be very helpful.

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-30 14:42_

It is still quite weird to me that you need an explicit `yield_now()` here. Basically, it should be just like a `std::thread::yield_now()`, which means it should be incredibly rare to use it.

Have you tried using tokio's bounded channels instead? Its `send` operation is async: https://docs.rs/tokio/latest/tokio/sync/mpsc/struct.Sender.html#method.send

(I would generally rather we use bounded channels with some kind of reasonable capacity everywhere, since they provide back-pressure. But it's hard to point to concrete specifics here, and is kind of hand-wavy.)

---

_@BurntSushi reviewed on 2024-01-30 14:43_

The `home-assistant` improvement is epic.

---

_Comment by @charliermarsh on 2024-01-30 15:16_

Woah, 2.5x? That's insane. I didn't even notice that.

---

_@charliermarsh reviewed on 2024-01-30 15:16_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-30 15:16_

We can try with bounded channels, seems straightforward?

---

_Comment by @charliermarsh on 2024-01-30 20:26_

@konstin - I see a 1.5x speedup for me, how is your warm `home-assistant` so fast?

```text
❯ python -m scripts.bench --puffin-path ./target/release/puffin --puffin-path ./target/release/main --benchmark resolve-warm scripts/requirements/home-assistant.in --min-runs 25
Benchmark 1: ./target/release/puffin (resolve-warm)
  Time (mean ± σ):     170.2 ms ±  11.3 ms    [User: 272.1 ms, System: 606.6 ms]
  Range (min … max):   148.5 ms … 195.1 ms    25 runs

Benchmark 2: ./target/release/main (resolve-warm)
  Time (mean ± σ):     243.4 ms ±   4.8 ms    [User: 210.9 ms, System: 206.4 ms]
  Range (min … max):   237.7 ms … 255.0 ms    25 runs

Summary
  './target/release/puffin (resolve-warm)' ran
    1.43 ± 0.10 times faster than './target/release/main (resolve-warm)'
```

Surprisingly, I don't really see any speedup for the cold benchmarks.

---

_@charliermarsh reviewed on 2024-01-30 20:26_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-30 20:26_

@konstin - I defer to you to try this out, but seems like it could be a good solution if it also lets us remove the yields.

---

_Comment by @konstin on 2024-01-30 21:36_

No idea, but the difference remains after rebase with more runs. Could this be an os scheduler or fs performance thing?

```console
# From konsti/yield-after-send with unambiguous `gt up`
$ gt down && cargo build --profile profiling && mv target/profiling/puffin-dev target/profiling/main-dev && mv target/profiling/puffin target/profiling/main && gt up && cargo build --profile profiling && mv target/profiling/puffin-dev target/profiling/branch-dev && mv target/profiling/puffin target/profiling/branch
$ hyperfine --warmup 3 --runs 100 "target/profiling/main pip compile scripts/requirements/home-assistant.in" "target/profiling/branch pip compile scripts/requirements/home-assistant.in"
Benchmark 1: target/profiling/main pip compile scripts/requirements/home-assistant.in
  Time (mean ± σ):     225.0 ms ±   2.0 ms    [User: 195.7 ms, System: 61.5 ms]
  Range (min … max):   222.6 ms … 238.0 ms    100 runs
 
Benchmark 2: target/profiling/branch pip compile scripts/requirements/home-assistant.in
  Time (mean ± σ):      92.7 ms ±   5.6 ms    [User: 302.4 ms, System: 174.8 ms]
  Range (min … max):    80.7 ms … 110.9 ms    100 runs
 
Summary
  target/profiling/branch pip compile scripts/requirements/home-assistant.in ran
    2.43 ± 0.15 times faster than target/profiling/main pip compile scripts/requirements/home-assistant.in
```

I'd be interested how the differences between main and this PR look for other people!


---

_Comment by @konstin on 2024-01-30 21:36_

Cold runs fluctuate between 1000ms and 2000ms, they are far too volatile for me to do any comparative benchmarking. The span structure looks very similar main and this PR, so i expect no change. You can see that we often have to wait for fetching the metadata of a specific version to start prefetches, the network idling in between. While the request parallelism seems to be well used in general (This is good!), i also see room for improvement, for lower gains though than the original.

Proposal: We try to speculate about the versions we would pick for other packages and prefetch as if we the solver was deciding on whichever package arrived first (instead of in a deterministic order, as we actually do), speculating that the resolution in arbitrary order is the same as the one in deterministic order. The solver itself remains deterministic, if the guess was wrong we only made an unnecessary request; we could consider cancelling them if they turned out to be wrong. The are different designs, such as sharing the partial solution or forking the pubgrub state with non-deterministic decision and rebasing them whenever deterministic decisions are made.

An interesting property would be to prefetch packages deep in the dependency tree: Let us define jupyter as a dependency of order 0, and jupyter requiring notebook makes notebook an order 1 dependency, then (given i did the graph traversal right) pycparser is a order 6 dep and types-python-dateutil (elsewhere in the tree) an order 7 dep (Fig. 4). The branch and main examples both have to wait on pycparser as last and second to last choose version case, in the right side end of the plot where we're not doing much except waiting for those packages (Fig. 1 and 2). If we could prefetch in a way that those deps can get fetch earlier, we could avoid this nearly-idle tail. This becomes even more apparent with `--refresh`, where the last two, almost isolated requests are pycparser and types-python-dateutil (Fig. 3).

I find it surprising that we spend way more time waiting for metadata than for versions, even though the average duration of simple api and metadata requests would have expect it the other way round. I guess this is because there are more version maps already present when we need them?

For `--refresh` specifically, we can avoid the slow start (Fig. 3, left side) by loading the (possibly stale), using it to start prefetches and then discarding it.

An easy option is to review the size of the `buffer_unordered`, there might be easy wins in there, at least as long as we ask the pypi people before making a 100 parallel request by default. 

**Fig 1: Cold main**

![image](https://github.com/astral-sh/puffin/assets/6826232/7a659523-1c88-4201-856e-b9b5356fd1c4)

**Fig 2: Cold branch**

![image](https://github.com/astral-sh/puffin/assets/6826232/04804758-d152-4b0f-877d-95a8b304bc98)

**Fig 3: Refresh branch**

![image](https://github.com/astral-sh/puffin/assets/6826232/f466b7f5-b19a-45c8-827f-6ef50ac93758)

**Fig 4: Dependency graph**

![jupyter_graphviz](https://github.com/astral-sh/puffin/assets/6826232/d2be6a62-1a51-47c4-b255-4132a83144ec)


---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/resolver/mod.rs`:9 on 2024-01-30 21:39_

Could we remove this rename? Either use the names directly, or the fully-qualified names? I just find this a little more confusing as a reader.

---

_@charliermarsh reviewed on 2024-01-30 21:39_

---

_@konstin reviewed on 2024-01-30 21:40_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:9 on 2024-01-30 21:40_

I'll make them consistently fully qualified (there's too many slightly different channels)

---

_Comment by @BurntSushi on 2024-01-30 22:01_

I'm seeing a 1.5x improvement:

```
$ hyperfine -w5 --cleanup 'rm -rf /home/andrew/.cache/puffin' "puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" "puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" ; A kang
Benchmark 1: puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
 ⠏ Performing warmup runs         ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ ETA 00:00:00 ^C
(.venv) [andrew@duff puffin]$ rm -rf ~/.cache/puffin/
(.venv) [andrew@duff puffin]$ hyperfine -w5 --cleanup 'rm -rf /home/andrew/.cache/puffin' "puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" "puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null" ; A kang
Benchmark 1: puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     243.7 ms ±   2.0 ms    [User: 227.1 ms, System: 81.9 ms]
  Range (min … max):   240.2 ms … 246.7 ms    12 runs

Benchmark 2: puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
  Time (mean ± σ):     158.4 ms ±  22.0 ms    [User: 407.0 ms, System: 335.3 ms]
  Range (min … max):   127.0 ms … 199.2 ms    18 runs

Summary
  puffin-test pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null ran
    1.54 ± 0.21 times faster than puffin-main pip compile ~/astral/tmp/reqs/home-assistant-reduced.in -o /dev/null
```

Pretty awesome!

I don't have any hypotheses for the discrepancy. @konstin How many CPU cores do you have? If you have a lot, and since this PR is improving parallelism efficiency, that might explain why you're seeing a greater improvement. (Although I kind of have a lot of cores too. I have 16 physical cores and 24 logical cores (i.e., 8 of my physical cores are hyper-threaded.)

---

_Comment by @charliermarsh on 2024-01-30 22:02_

The actual raw numbers are kind of crazy to me though, like Konsti's is 92.7ms!

---

_Comment by @konstin on 2024-01-30 22:10_

Thank you @BurntSushi, that's the answer! I'm using an i9-13900K with 8 performance cores, 16 efficiency cores and 32 threads. Limiting the number of cores reduces the speedup (output cropped for readability):

```console
$ taskset -c 0-3 hyperfine --warmup 3 "target/profiling/main pip compile scripts/requirements/home-assistant.in" "target/profiling/branch pip compile scripts/requirements/home-assistant.in"
1: Time (mean ± σ):     229.8 ms ±   1.4 ms    [User: 198.1 ms, System: 60.4 ms]
2: Time (mean ± σ):     170.9 ms ±   6.1 ms    [User: 335.1 ms, System: 127.7 ms]
target/profiling/branch pip compile scripts/requirements/home-assistant.in ran
  1.34 ± 0.05 times faster than target/profiling/main pip compile scripts/requirements/home-assistant.in
$ taskset -c 0-7 hyperfine --warmup 3 "target/profiling/main pip compile scripts/requirements/home-assistant.in" "target/profiling/branch pip compile scripts/requirements/home-assistant.in"
1: Time (mean ± σ):     224.4 ms ±   1.3 ms    [User: 194.0 ms, System: 56.6 ms]
2: Time (mean ± σ):     112.6 ms ±   5.7 ms    [User: 301.4 ms, System: 140.0 ms]
target/profiling/branch pip compile scripts/requirements/home-assistant.in ran
  1.99 ± 0.10 times faster than target/profiling/main pip compile scripts/requirements/home-assistant.in
$ taskset -c 0-15 hyperfine --warmup 3 "target/profiling/main pip compile scripts/requirements/home-assistant.in" "target/profiling/branch pip compile scripts/requirements/home-assistant.in"
1: Time (mean ± σ):     222.4 ms ±   1.5 ms    [User: 191.7 ms, System: 56.8 ms]
2: Time (mean ± σ):      88.6 ms ±   1.9 ms    [User: 276.0 ms, System: 137.6 ms]
target/profiling/branch pip compile scripts/requirements/home-assistant.in ran
  2.51 ± 0.06 times faster than target/profiling/main pip compile scripts/requirements/home-assistant.in
$ taskset -c 0-31 hyperfine --warmup 3 "target/profiling/main pip compile scripts/requirements/home-assistant.in" "target/profiling/branch pip compile scripts/requirements/home-assistant.in"
1: Time (mean ± σ):     226.3 ms ±   1.8 ms    [User: 197.7 ms, System: 59.3 ms]
2: Time (mean ± σ):      90.4 ms ±   5.4 ms    [User: 294.2 ms, System: 174.1 ms]
target/profiling/branch pip compile scripts/requirements/home-assistant.in ran
  2.50 ± 0.15 times faster than target/profiling/main pip compile scripts/requirements/home-assistant.in
```

---

_@konstin reviewed on 2024-01-31 16:12_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-31 16:12_

@BurntSushi Why is a bounded channel better, and what would a reasonable channel size be? Put differently, what the advantage stopping the producer, given that we know our problem is finite and reasonably sized?

---

_@BurntSushi reviewed on 2024-01-31 16:55_

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-01-31 16:55_

I don't have anything concrete to say in the context of puffin other than the hand-wavy "it provides back-pressure." As for why back-pressure is important, I'll point to the following:

* https://lucumr.pocoo.org/2020/1/1/async-pressure/
* https://medium.com/@jayphelps/backpressure-explained-the-flow-of-data-through-software-2350b3e77ce7
* https://ferd.ca/handling-overload.html

That is, the point of bounded channels isn't really that they "stop," but rather, that they react to things downstream that have gotten clogged. Namely, if a bounded channel stops, that's usually only a symptom of either a capacity that is too small or something downstream that has gotten overloaded. In the former case, we fix it by tuning the capacity. In the latter case, well, [it's a feature and not a bug that the bounded channel has stopped sending more input](https://www.youtube.com/watch?v=K3axU2b0dDk) into something that is already overloaded.

As for what a reasonable capacity would be... I'm not sure. It's not that dissimilar from `Vec::with_capacity`. Usually starting with a small (or even zero) capacity is good enough. But one can experimentally arrive at a different figure. Maybe start with the number of cores? (Or just hard-code 16, since that's probably only a little more than what most folks have, and a number smaller than the number of cores doesn't mean that not all cores will be used.) Just a guess.

Basically, my position is the bounded channels should be the default, because they---hand-wavily---lead to overall better holistic properties of the system. The provide a means for back-pressure. Unbounded channels have no mechanism for back-pressure, and I think they are generally an anti-pattern unless there is some specific motivation for them.

Also, [a nice historical anecdote.](http://web.archive.org/web/20160811222801/http://www.eros-os.org/pipermail/e-lang/2003-January/008183.html) ("async" channels are unbounded and "sync" channels are bounded.)

---

_Label `performance` added by @zanieb on 2024-02-02 17:01_

---

_@konstin reviewed on 2024-02-02 17:17_

---

_Review comment by @konstin on `crates/puffin-resolver/src/resolver/mod.rs`:420 on 2024-02-02 17:17_

Switched to `tokio::sync::mpsc::channel` with a bound size of 50.

---

_Merged by @konstin on 2024-02-02 17:18_

---

_Closed by @konstin on 2024-02-02 17:18_

---

_Branch deleted on 2024-02-02 17:18_

---
