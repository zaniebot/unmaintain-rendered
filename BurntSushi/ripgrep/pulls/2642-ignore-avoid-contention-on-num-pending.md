```yaml
number: 2642
title: "ignore: Avoid contention on num_pending"
type: pull_request
state: closed
author: tavianator
labels:
  - rollup
assignees: []
base: master
head: num-pending-contention
created_at: 2023-10-30T20:04:40Z
updated_at: 2023-12-19T18:45:02Z
url: https://github.com/BurntSushi/ripgrep/pull/2642
synced_at: 2026-01-12T18:23:14Z
```

# ignore: Avoid contention on num_pending

---

_@tavianator_

Previously, every worker would increment the shared num_pending count on
every new work item, and decrement it after finishing them, leading to
lots of contention.  Now, we only track the number of workers actively
running, so there is no contention except when workers go to sleep or
wake up.


---

_Comment by @tavianator on 2023-10-30 20:10_

Quick benchmark results (updated):

```console
tavianator@tachyon $ hyperfine -w2 fd-{before,after}" -j24 -u '^$' ~/code/bfs/bench/corpus/chromium"
Benchmark 1: fd-before -j24 -u '^$' ~/code/bfs/bench/corpus/chromium
  Time (mean ± σ):     297.6 ms ±  10.9 ms    [User: 3453.3 ms, System: 3379.4 ms]
  Range (min … max):   279.9 ms … 313.3 ms    10 runs
 
Benchmark 2: fd-after -j24 -u '^$' ~/code/bfs/bench/corpus/chromium
  Time (mean ± σ):     231.6 ms ±   6.9 ms    [User: 1568.7 ms, System: 3660.1 ms]
  Range (min … max):   224.6 ms … 248.9 ms    12 runs
 
Summary
  fd-after -j24 -u '^$' ~/code/bfs/bench/corpus/chromium ran
    1.29 ± 0.06 times faster than fd-before -j24 -u '^$' ~/code/bfs/bench/corpus/chromium
tavianator@tachyon $ hyperfine -w2 fd-{before,after}" -j48 -u '^$' ~/code/bfs/bench/corpus/chromium"
Benchmark 1: fd-before -j48 -u '^$' ~/code/bfs/bench/corpus/chromium
  Time (mean ± σ):     226.1 ms ±  17.9 ms    [User: 4597.6 ms, System: 4339.5 ms]
  Range (min … max):   210.0 ms … 260.0 ms    13 runs
 
Benchmark 2: fd-after -j48 -u '^$' ~/code/bfs/bench/corpus/chromium
  Time (mean ± σ):     199.6 ms ±  17.7 ms    [User: 2232.0 ms, System: 5361.9 ms]
  Range (min … max):   185.8 ms … 238.4 ms    15 runs
 
Summary
  fd-after -j48 -u '^$' ~/code/bfs/bench/corpus/chromium ran
    1.13 ± 0.13 times faster than fd-before -j48 -u '^$' ~/code/bfs/bench/corpus/chromium
```

---

_Review comment by @BurntSushi on `crates/ignore/src/walk.rs`:1687 on 2023-10-30 20:51_

Can you add a comment explaining the termination logic here?

---

_Review comment by @BurntSushi on `crates/ignore/src/walk.rs`:1697 on 2023-10-30 20:58_

So I did like how previously there were methods with decentish names that made call sites a bit more descriptive. Maybe something like this:

```
// Inactivates a single worker and
// returns the total number of remaining
// active workers.
fn worker_inactivate(&mut self) -> bool { ... }

// Activates a single inactive worker.
fn worker_activate(&mut self) { ... }
```

---

_@BurntSushi requested changes on 2023-10-30 20:59_

Thanks! I'll need to think on this one. I've had problems with termination in the past and this changes the logic around that.

---

_@tavianator reviewed on 2023-10-30 21:18_

---

_Review comment by @tavianator on `crates/ignore/src/walk.rs`:1687 on 2023-10-30 21:18_

I updated the comment with a correctness argument in d6f850f57436301fea602a5b4419ffacb464bcc9

---

_Review comment by @tavianator on `crates/ignore/src/walk.rs`:1697 on 2023-10-30 21:18_

Good idea, done in d6f850f57436301fea602a5b4419ffacb464bcc9

---

_@tavianator reviewed on 2023-10-30 21:18_

---

_Comment by @tavianator on 2023-10-30 21:21_

> Thanks! I'll need to think on this one. I've had problems with termination in the past and this changes the logic around that.

Understandable!  I am reasonably sure of the correctness now that I've strengthened the memory orderings to acquire/release.  But it's not a formal proof, and I haven't tried too hard to break it either.

---

_Comment by @BurntSushi on 2023-10-30 21:21_

Oh, also, what about the relaxed loads? It looks like they should be fine to me here, but I wonder if Acquire/Release are necessary. I think that's probably why I used `SeqCst` originally (because I didn't think through what was actually required).

---

_Comment by @tavianator on 2023-10-30 21:31_

> Oh, also, what about the relaxed loads? It looks like they should be fine to me here, but I wonder if Acquire/Release are necessary. I think that's probably why I used `SeqCst` originally (because I didn't think through what was actually required).

I *think* it was still correct with relaxed accesses due to other orderings imposed by the deque implementation, but it's easier to argue correctness with acquire/release and they're not performance-critical any more so I switched them.

---

_@tmccombs approved on 2023-10-31 15:08_

---

_@BurntSushi approved on 2023-11-21 23:29_

OK, I've tried this out:

1. I think I buy that this is a correct change.
2. I have actually tried it on a number of corpora and everything looks OK.
3. I haven't been able to observe any reliable performance difference. Nevertheless, I buy the argument here.

Thank you!!!

---

_Label `rollup` added by @BurntSushi on 2023-11-21 23:29_

---

_Closed by @BurntSushi on 2023-11-21 23:39_

---

_Branch deleted on 2023-12-19 18:45_

---
