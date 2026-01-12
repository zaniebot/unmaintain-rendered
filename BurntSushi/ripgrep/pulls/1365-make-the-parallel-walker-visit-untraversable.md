```yaml
number: 1365
title: Make the parallel walker visit untraversable directories before failing
type: pull_request
state: closed
author: jakubadamw
labels:
  - rollup
assignees: []
base: master
head: issue-1346
created_at: 2019-09-03T22:03:18Z
updated_at: 2020-02-17T22:16:40Z
url: https://github.com/BurntSushi/ripgrep/pull/1365
synced_at: 2026-01-12T18:23:13Z
```

# Make the parallel walker visit untraversable directories before failing

---

_@jakubadamw_

This PR fixes an inconsistency between the serial and the parallel
directory walkers around visiting a directory for which the user holds
insufficient permissions to descend into.

The serial walker does produce a successful entry for a directory that
it cannot descend into due to insufficient permissions. However, before
this change that has not been the case for the parallel walker, which
would produce an `Err` item not only when descending into a directory
that it cannot read from but also for the directory entry itself.

This change brings the behaviour of the parallel variant in line with
that of the serial one.

Fixes #1346.

---

_Comment by @jakubadamw on 2019-09-04 12:09_

I should probably note that the new test will always fail when running it as a root. Is that a problem?

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1365 on 2019-09-04 12:19_

Could you maybe add a short comment about the ordering here? e.g., Why `readdir` is being handled here instead of above when `work.read_dir()` is called?

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:2102 on 2019-09-04 12:22_

Hmm, yeah, tests should not fail depending on what user ran them. Maybe it would be good to check if the file is readable after chmodding it. If it is, then skip the test.

With that said, it would be nice if we could avoid bringing in `libc` and `unsafe` for this test. Is there some other way to test this? Nothing is coming to mind... Maybe using a pre-existing file that a normal user traditionally doesn't have access to? (And skipping the test if the file doesn't exist or is readable.)

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:2102 on 2019-09-04 12:22_

Please also make sure that lines are wrapped to 79 columns, inclusive.

---

_@BurntSushi requested changes on 2019-09-04 12:22_

Thanks for looking into this!

---

_@jakubadamw reviewed on 2019-09-04 16:36_

---

_Review comment by @jakubadamw on `ignore/src/walk.rs`:2102 on 2019-09-04 16:36_

@BurntSushi, I added the proposed check, though, of course, it'd be better if the harness allowed marking a test as skipped at runtime.

With regard to avoiding adding a dev-dependency on `libc` I don't have a good idea either. But now I made it a Linux-only dependency. Perhaps that's good enough?

If using `libc` is really an issue (even though it already is a regular dependency for some other ripgrep crates, though, in truth, not `ignore` itself), then I'll be happy to change this to try to walk /root and check if it is observing at least one valid entry (i.e. the one for the directory itself).

---

_@BurntSushi reviewed on 2019-09-04 17:19_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:2102 on 2019-09-04 17:19_

I probably care more about the extra `unsafe` to be honest. I'm mostly just trying to make sure things are as a tight as possible. This isn't a _huge_ deal, but I want to try for an alternative if it's feasible.

Just looking at my file system, maybe just trying look for `/etc/sudoers.d`? It's readable by `root`, but not by anyone else. So I think the logic might be something like this:

* Stat `/etc/sudoers.d` via `std::fs::metadata()`.
* If it errors, skip test.
* Read `/etc/sudoers.d`.
* If it succeeds, skip test.
* Try to traverse `/etc/sudoers.d` and run the test normally.

---

_Review comment by @jakubadamw on `ignore/src/walk.rs`:2102 on 2019-09-04 17:39_

@BurntSushi, alright, sounds like a plan! I pushed the proposed change. Without a doubt that made the test much shorter and simpler. Thanks for the review!

---

_@jakubadamw reviewed on 2019-09-04 17:39_

---

_@BurntSushi approved on 2019-09-04 17:41_

Okay, I think this LGTM. I'll merge this once I get CI back online. (I tried setting up GitHub Actions over the weekend, but [it's completely stuck](https://github.com/BurntSushi/ripgrep/actions).)

---

_Comment by @jakubadamw on 2019-09-04 17:48_

@BurntSushi, there's a button there for cancelling a workflow. Have you tried that? (You probably have and I'm asking a stupid question. 🙂)

---

_Comment by @BurntSushi on 2019-09-04 17:52_

> there's a button there for cancelling a workflow. Have you tried that? (You probably have and I'm asking a stupid question. slightly_smiling_face)

Hah, I wish. But yeah, that button does normally show up. In my case though, Actions seems to be stuck in some sort of initialization state where by that button doesn't show up. The jobs have been running for over 4 days too, which suggests timeout functionality doesn't work either.

I've sent mail to GitHub support but haven't heard back from them.

I've turned Travis and AppVeyor back on. Going to see if I can trigger it on this PR.

---

_Closed by @BurntSushi on 2019-09-04 17:52_

---

_Reopened by @BurntSushi on 2019-09-04 17:52_

---

_Comment by @treere on 2019-09-21 20:55_

there is a subtle bug and a performance issue. give me some minutes and I will explain.

---

_Review comment by @treere on `ignore/src/walk.rs`:1351 on 2019-09-21 20:58_

here there is an unnecessary syscall. I think that a clone is 99% better in performance

---

_Review comment by @treere on `ignore/src/walk.rs`:1365 on 2019-09-21 21:24_

I think that readdir shuold be handled after the next if (line 1376). just before the iteration on the folder content.

If the the maximum depth has been reached you should I risk to `self.quit_now()`? 

---

_@treere reviewed on 2019-09-21 21:25_

I think that there is a small bug and a little persormance issue. (one is inside a piece marked as resolved)

---

_Review comment by @tavianator on `ignore/src/walk.rs`:2102 on 2019-09-23 16:27_

Maybe `/root` (or more generally `~root`) is more ubiquitous than `/etc/sudoers.d`?

---

_@tavianator reviewed on 2019-09-23 16:27_

---

_@BurntSushi reviewed on 2020-02-17 18:21_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1351 on 2020-02-17 18:21_

I'm not sure I follow your comment here. There is exactly one call to `read_dir` both before and after this PR.

---

_Label `rollup` added by @BurntSushi on 2020-02-17 18:31_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
