```yaml
number: 601
title: "Does WalkParallel::run need to be blocking?"
type: issue
state: closed
author: sharkdp
labels: []
assignees: []
created_at: 2017-09-08T18:51:51Z
updated_at: 2017-09-09T08:11:01Z
url: https://github.com/BurntSushi/ripgrep/issues/601
synced_at: 2026-01-12T16:13:22Z
```

# Does WalkParallel::run need to be blocking?

---

_@sharkdp_

Currently, there is the following loop at the end of the `WalkParallel::run` method:

https://github.com/BurntSushi/ripgrep/blob/beb010d004a79eedb8baa6f4eab21532e83a0ef0/ignore/src/walk.rs#L944-L946

When generating a `WalkParallel` over a directory with many entries in [fd](https://github.com/sharkdp/fd) (see [this PR](https://github.com/sharkdp/fd/pull/41)), this loop - and therefore, the `run`-method-call - takes several *seconds* to finish.

I tried to simply remove them and it seems to work fine (for my use case :smile:). So without understanding the details, I'm just going to ask: are these calls necessary? Does `run` have to be blocking?

---

_Comment by @BurntSushi on 2017-09-08 21:05_

Those thread join calls synchronize the completion of all worker threads. Without some synchronization step, there is no guarantee that the worker threads will terminate before the program terminates.

This sounds like an XY problem to me honestly. What problem are you trying to solve?

---

_Comment by @sharkdp on 2017-09-09 08:11_

Thank you for the explanation!

> This sounds like an XY problem to me honestly.

You were right, I figured out a way to solve this on my side.

> What problem are you trying to solve?

I have a `sync::mpsc::channel` and each thread that is spawned by `WalkParallel::run` is sending results into the channel. I was calling `run` from the main thread and wanted to collect the results on the receiver-side of the channel in the main thread as well. Now, I'm spawning another thread for the receiver (before calling `WalkParallel::run`).

---

_Closed by @sharkdp on 2017-09-09 08:11_

---
