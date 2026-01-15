```yaml
number: 3010
title: "ignore: gracefully quit worker threads upon panic in ParallelVisitor"
type: pull_request
state: closed
author: cosmicexplorer
labels: []
assignees: []
base: master
head: parallel-panic-quit
created_at: 2025-03-06T04:03:41Z
updated_at: 2026-01-15T19:30:39Z
url: https://github.com/BurntSushi/ripgrep/pull/3010
synced_at: 2026-01-15T20:00:48Z
```

# ignore: gracefully quit worker threads upon panic in ParallelVisitor

---

_@cosmicexplorer_

*Fixes #3009.*

# Problem
`WalkParallel::visit()` will nondeterministically hang if the `ParallelVisitor::visit()` implementation panics. This also occurs when providing a closure to `WalkParallel::run()`. Minimal repro is provided in #3009:

```rust
        WalkBuilder::new(path)
            .build_parallel()
            .run(|| Box::new(|_| panic!("oops!")));
```

The above code will nondeterministically hang, because of an infinite loop when no new work is available: https://github.com/BurntSushi/ripgrep/blob/de4baa10024f2cb62d438596274b9b710e01c59b/crates/ignore/src/walk.rs#L1695-L1707

# Solution
1. Check the `quit_now` flag in our wait loop.
2. Catch any panic in the `run()` method and set `quit_now` before propagating the panic.

**Breaking change:** In order to ensure soundness, we also enforce that the filter method provided to `WalkBuilder#filter_entry()` is `UnwindSafe`. Users can circumvent this by wrapping in `AssertUnwindSafe` as needed.

# Result
The added test `panic_in_parallel()` always succeeds instead of hanging.

---

_Renamed from "correctly quit out of busy loops if a ParallelVisitor panics to avoid a hang" to "ignore: correctly quit out of busy loops if a ParallelVisitor panics to avoid a hang" by @cosmicexplorer on 2025-03-06 04:06_

---

_Renamed from "ignore: correctly quit out of busy loops if a ParallelVisitor panics to avoid a hang" to "ignore: gracefully quit worker threads upon panic in ParallelVisitor" by @cosmicexplorer on 2025-03-06 04:07_

---

_@BurntSushi reviewed on 2025-08-17 21:05_

---

_Review comment by @BurntSushi on `crates/ignore/src/walk.rs`:915 on 2025-08-17 21:05_

Unfortunately, I think this is plausibly too big of a breaking change for me to stomach in a semver compatible release. And I don't have the bandwidth to do a semver incompatible release right now. Which kind of makes this PR stuck.

One possible way to get this unstuck is to _add_ a new API that avoid this problem and deprecate the old one. But I'd only want to go this route if it was a very small addition that doesn't significantly complicate the implementation. I'm not sure if such a solution exists.

Otherwise, I'm inclined to leave this bug open for now, and just something to address in the next semver incompatible release.

---

_Comment by @BurntSushi on 2025-08-17 21:05_

Closing, but keeping #3010 open.

---

_Closed by @BurntSushi on 2025-08-17 21:05_

---

_Comment by @BurntSushi on 2025-08-17 21:06_

Also, thank you for your work on this! Even though I ended up not taking this PR, your work will be something worth building on for `ignore 0.5`.

---

_Comment by @cosmicexplorer on 2025-11-27 13:34_

Wanted to confirm here that:
- I always love working with you and your projects,
- this is exactly the choice I would make as a maintainer,
- and thanks so much for your careful and thorough reply!

And I am super glad to hear `ignore 0.5` is on the horizon! 

I would like you to know that as usual, it is extremely difficult to improve upon the kind of work you do. I have tried to argue with your comments in my head and largely failed. I have spent months investigating this, and I have had to introduce some immense complexity in order to make a pretty minor use case more robust. Great work.

I'm also looking at the API, and while I'm not done with mine yet, I would absolutely recommend considering the use of [`ops::ControlFlow`](https://doc.rust-lang.org/std/ops/enum.ControlFlow.html), either internally or in the API. I think using `ControlFlow` may contain strictly more information than the untagged stop token. Personally, I have also separated the worker threads by category, and given each category individually configurable thread counts. I don't know if that will help performance, but I *do* think that the definition of "done" differs based upon whether the current node is a directory or not, which I think may justify slightly different looping techniques.

I also think the deadlock that occurred here is a symptom of a more general misalignment of the threading model to the input distribution. I think work stealing + *recursive* data generation (so each work item can always produce more) is necessarily prone to this deadlock. I do not have a proof of this, and I also do not have a more appropriate answer myself yet. I *do* understand that work stealing is a good approach to avoid having a single very large directory fill up its own queue. I *also* wonder whether there is any benefit to maximizing thread locality.

Finally, I was informed about a very widely available syscall `getdents()`, which is provided in Linux and BSD stdlibs, but not macOS, which writes multiple directory entries into a provided buffer. There is even a `posix_getdents()` from POSIX 2024, and musl supports it, but I have been having a very difficult time getting it accepted to the rust libc crate: https://github.com/rust-lang/libc/pull/4522.

I am actually very confident that supporting `getdents()` will produce a drastic performance improvement for `fs::read_dir()`, so I'm going to try demonstrating that today. If I can demonstrate its utility with benchmarks, I will probably ping you again, and ask you to help support my quest to get this syscall into the stdlib.

`ignore 0.5` may be able to just call upon `fs::read_dir()`, but for pipelining it can be nice to control the buffer size. Note that the alignment of the output is (1) very stable (2) ridiculous.
https://codeberg.org/cosmicexplorer/deep-link/src/commit/1ea3eba5d599d8c48ea56816b7103b12ee49d505/d-major/readdir-sys/src/getdents.rs#L101-L188

Again, very impressed by your engineering judgement, and I appreciate when you write a comment about decisions you *didn't* take too. Keep it up!!

---
