```yaml
number: 1410
title: "Support borowing data in `ignore`s `WalkParallel`"
type: issue
state: closed
author: epage
labels: []
assignees: []
created_at: 2019-10-27T01:33:13Z
updated_at: 2020-02-17T22:16:37Z
url: https://github.com/BurntSushi/ripgrep/issues/1410
synced_at: 2026-01-12T16:13:23Z
```

# Support borowing data in `ignore`s `WalkParallel`

---

_@epage_

I'd prefer to have my `WalkParallel` closures take references.

In general, this would improve the usability of `WalkParallel` because it takes a lot of work to change code from a trivial case to a non-trivial case (getting everything in `Arc`. etc). or from serial to parallel.

The way this is impacting me is that this will simplify my code because I can share a lot more code between my parallel (throwing everything in `Arc`) and non-parallel (directly using references).

---

_Comment by @epage on 2019-10-27 01:38_

I started implementing #469 to make it easier for me to work through the errors.  I'm at the point of dealing with the threads.  The way the threads are used, Rust can't know that the threads will all join by the end of the function call, requiring a `'static` lifetime.  Using `crossbeam` for scoped-threads should make this problem go away.

I wanted to check-in before going too much further.  It looks like [`scope`](https://docs.rs/crossbeam-utils/0.6.6/crossbeam_utils/thread/fn.scope.html) is in `crossbeam-utils` which is a dependency of `crossbeam-channels`, so this shouldn't add any new dependency to the full dependency tree.

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
