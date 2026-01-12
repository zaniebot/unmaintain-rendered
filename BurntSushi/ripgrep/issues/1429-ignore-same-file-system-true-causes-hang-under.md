```yaml
number: 1429
title: "ignore: `.same_file_system(true)` causes hang under Darwin"
type: issue
state: closed
author: FallenWarrior2k
labels: []
assignees: []
created_at: 2019-11-14T22:38:53Z
updated_at: 2019-11-15T00:20:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1429
synced_at: 2026-01-12T16:13:23Z
```

# ignore: `.same_file_system(true)` causes hang under Darwin

---

_@FallenWarrior2k_

#### What version of ignore are you using?

0.4.10

#### What operating system are you using ignore on?

Travis CI's `x86_64-apple-darwin` target

#### Describe your question, feature request, or bug.

I tried adding support for stopping at file system boundaries to [sharkpd/fd](https://github.com/sharkdp/fd) by using `WalkBuilder`'s `.same_file_system()` method.
I tested it locally (Arch Linux with 5.3.11-1-ck-haswell), both with tests and by manually playing around with test inputs, and it achieved the intended result. However, after pushing it with the intention to open a PR against the fd repo, I got a notification from Travis that the build had failed.
As can be seen [here](https://travis-ci.com/FallenWarrior2k/fd/jobs/256772331) and [here](https://travis-ci.com/FallenWarrior2k/fd/jobs/256772340), the corresponding test times out on Darwin on both stable and 1.31.0.

#### If this is a bug, what are the steps to reproduce the behavior?

I personally do not have a Mac device so I cannot repro the bug locally, but if you have a Mac, checking out [FallenWarrior2k/fd](https://github.com/FallenWarrior2k/fd/tree/same-fs) at branch `same-fs` and running the tests should (probably? hopefully?) be enough to trigger the bug.
More specifically, I was looking for `/dev/null/` starting at `/` with `.same_file_system(true)`, which resulted in empty output on all other platforms.

#### If this is a bug, what is the actual behavior?

The program hangs when using `walk_builder.same_file_system(true)`.

#### If this is a bug, what is the expected behavior?

ignore should either succeed or return an error in case the operation is not supported.


---

_Comment by @BurntSushi on 2019-11-14 22:46_

Thanks for the bug report. While I do have a mac that I use for testing, I'm unlikely to look at this unless a small reproduction can be provided. I don't have the bandwidth to debug an entire application like fd. Sorry.

---

_Comment by @FallenWarrior2k on 2019-11-15 00:20_

Okay, after creating a pretty minimal repro, [Travis results](https://travis-ci.com/FallenWarrior2k/ignore-darwin-hang-repro) show that it's apparently not an actual hang, but just insanely slow in Travis CI's `os: osx` environments.
I'll be closing this as it's not a bug, but rather a (presumably external) performance issue that's probably not even related to `ignore` itself.

---

_Closed by @FallenWarrior2k on 2019-11-15 00:20_

---
