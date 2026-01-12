```yaml
number: 1337
title: "ignore: rework inter-thread messaging"
type: pull_request
state: closed
author: zsugabubus
labels:
  - rollup
assignees: []
base: master
head: ignore-rework-messaging
created_at: 2019-08-02T12:56:43Z
updated_at: 2020-02-20T21:09:41Z
url: https://github.com/BurntSushi/ripgrep/pull/1337
synced_at: 2026-01-12T18:23:13Z
```

# ignore: rework inter-thread messaging

---

_@zsugabubus_

Change the meaning of `Quit` message. Now it means terminate. The final
"dance" is unnecessary, because by the time quitting begins, no thread
will ever spawn a new `Work`.

Replace that heuristic spin-loop, with blocking receive.

Optimize use of atomic operations.

---

_Comment by @BurntSushi on 2019-08-02 12:58_

Thanks. This will take me a while to get to, because I'll need to carefully review this to make sure it's correct. It is very very easy to introduce subtle bugs into this code, and more importantly, those bugs can be hard to catch.

---

_Comment by @zsugabubus on 2019-08-02 12:59_

I tried to explain every subtle details in comments, to make sure it's correct what I do.

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1474 on 2019-08-02 13:17_

Nice. This is clever. I think I buy it.

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1489 on 2019-08-02 13:17_

Why the `map_err` here instead of an `unwrap`?

---

_@BurntSushi reviewed on 2019-08-02 13:19_

Nice nice. I think I buy this change. Before merging it, I'm going to try to test this as best I can.

---

_@zsugabubus reviewed on 2019-08-02 13:27_

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1489 on 2019-08-02 13:27_

A few lines below I wrote an `unreachable!()` assertion that also covers `try_recv()` and this way `recv()` errors. So you will get a meaningful error message when panicking (although it can never occur (currently)).

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1489 on 2019-08-02 13:33_

Although that `uncreachable!()` statement below is unnecessary, and we could use `unwrap` everywhere, I just wanted to explicitly distinguish `Err(TryRecvError::Empty)` from `Err(TryRecvError::Disconnected)` cases. In the future, maybe `Disconnected` brach will be the quit.

---

_@zsugabubus reviewed on 2019-08-02 13:33_

---

_@BurntSushi reviewed on 2019-08-02 13:34_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1489 on 2019-08-02 13:34_

Right, yeah, I saw that. But then it becomes harder to distinguish between "channel somehow got disconnected from a bug" and "this particular blocking `recv` failed." That is, forwarding the error like this strips information from the inevitable backtrace as far as I can tell. I'd rather see it panic closer to where the error occurred.

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1489 on 2019-08-02 13:35_

Yeah I like the idea of distinguishing the error cases. That part is good. :)

---

_@BurntSushi reviewed on 2019-08-02 13:35_

---

_Comment by @zsugabubus on 2019-08-02 13:37_

I also expect some minimal(?) speedups, because that two expensive atomic operations are gone before `return Some(work)`.

---

_Comment by @BurntSushi on 2019-08-02 13:41_

Keep in mind that atomic operations are themselves likely quite inexpensive compared with processing the work itself, so I'd be surprised to see any performance changes here. Other than perhaps latency being better with quitting, but it's hard to reason about that.

My longer term plans for this code is to either find a way to switch to `rayon`, or be more principled about work stealing and work distribution.

---

_@zsugabubus reviewed on 2019-08-02 13:48_

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1489 on 2019-08-02 13:48_

> I'd rather see it panic closer to where the error occurred

I have no so much experience in Rust. It seems very reasonable.

On the other hand, what if you want to call `drop` on the channel intentionally and you want it to quit instead of panicking. Then it's better to forward the error instead of `unwrap`. And I think, knowing which `(try_)recv` panicked is secondary. The most important information is that it hase been disconnected.

But it's hard to reason about, which would be the better choice, because it cannot occur now.

---

_@BurntSushi reviewed on 2019-08-02 13:52_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1489 on 2019-08-02 13:52_

> And I think, knowing which (try_)recv panicked is secondary. The most important information is that it hase been disconnected.

If you use `recv().unwrap()` instead of `recv().map_err(...)`, then we get both. Where as with the `map_err`, you only know that the channel was disconnected.

> But it's hard to reason about, which would be the better choice, because it cannot occur now.

Right, _unless_ there is a bug. So basically, this is about choosing a failure mode that provides the most information and is thus easier to debug, if and when it does occur.

---

_@BurntSushi approved on 2020-02-17 21:56_

I'm finally getting around to merging this! It will be coming in as part of #1486. 

I reviewed this again, tested it and tried to break it. But it seems solid. All in all, I think it is much much more elegant than what was here before. So, thank you very much!

---

_Label `rollup` added by @BurntSushi on 2020-02-17 21:59_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Comment by @zsugabubus on 2020-02-17 22:22_

Thanks!

---

_Branch deleted on 2020-02-17 22:57_

---

_@BurntSushi reviewed on 2020-02-19 22:16_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1479 on 2020-02-19 22:16_

@zsugabubus So I unfortunately had to revert this PR. While I wasn't able to reproduce any problems locally, I was observing transient CI failures that could only be explained by the parallel directory traversal accidentally dropping work. After staring at this patch for a bit, I think I convinced myself that the problem is a race between when `value` is populated from the blocking receive here, the call to `self.resume()` and the call to `self.waiting()` above.

In particular, I believe there is a possible interleaving where `self.waiting() == 1` is true but there is actually still more work to be done. Once `value = ...;` is unblocked, it is possible for another worker to think that it was the last one running before `self.resume()` is called. This in turn causes the `Quit` domino effect to kick in, which in turn is guaranteed to cause all other workers to stop even if there is still outstanding work to be done.

I believe I had similar issues in my initial implementation of parallel traversal, and that was what provoked the introduction of the "waiting" and "quitting" dance. AIUI, you probably would need something similar here to fix the race, I think.

Using blocking semantics however is still probably better than the implementation I have now, which inserts sleeps in order to avoid burning the CPU. Nevertheless, I'm going to stick with the old implementation for now unless you want to take another crack at it.

---

_@zsugabubus reviewed on 2020-02-20 00:27_

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1479 on 2020-02-20 00:27_

But new works can’t only originate from a (running) worker? (Did something change since or I had some fatal assumptions?)

If every other worker is blocked at that `recv`, they surely won’t add anything… and before the last one goes to sleep (blocking) can safely start killing its brothers (initiate quitting).

The thing you talk about could only occur only if something other than a worker would start pushing new works, no (*)?
This is the only code segment that accesses `tx` from outside, but it surely finishes before workers start at all (it just puts on root directories):
https://github.com/BurntSushi/ripgrep/blob/72f5a25435f3dad1c69d16010b0aa796f2523f70/ignore/src/walk.rs#L1140-L1144

~*: Or if there is a bug(?) in channels implementation and makes `try_recv` reporting no work (empty), even though there would we works; and then randomly wakes up a blocking `recv` instead. (Is it possible?)~ No.

---

_@zsugabubus reviewed on 2020-02-20 00:36_

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1479 on 2020-02-20 00:36_

On the other hand, I took a short glimpse into the documentation of channels, and if everything is true, we could completely eliminate `resume()`s and `waiting()`s by [`is_empty()`](https://docs.rs/crossbeam-channel/0.4.0/crossbeam_channel/struct.Receiver.html#method.is_empty). So instead `self.waiting() == 1`, `self.rx.is_empty()` could be written simply.

---

_@BurntSushi reviewed on 2020-02-20 00:54_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1479 on 2020-02-20 00:54_

So first I want to say that since reverting it, all transient failures I was observing have completely stopped. (The nature of the transient failures was such that the parallel traverser was returning only a subset of file paths that it should be returning. So it was either skipping some file paths or it was stopping early.) So I think, at least from my perspective, there is clear evidence that there is a problem with this change.

The key to my thinking behind the problem is that there is an _unsynchronized_ race between the `self.waiting() == 1` check and `value = ...;` being unblocked and `self.resume()` being called. Between those statements, there exists a point in time in which there is work remaining in a worker, but where that worker is currently regarded as `waiting.` All that needs to happen for that to occur is this:

1. Consider that all workers, except for one, are `waiting`.
2. The last remaining worker finds one more job to do and sends it on the channel.
3. One of the previously `waiting` workers wakes up from the job that the last running worker sent, but `self.resume()` has not been called yet.
4. The last worker, from (2), calls `get_work` and sees that the channel has nothing on it, so it executes `self.waiting() == 1`. Since the worker in (3) hasn't called `self.resume()` yet, `self.waiting() == 1` evaluates to true.
5. This sets off a chain reaction that stops all workers, despite that fact that (3) got more work (which could itself spawn more work).

---

_@BurntSushi reviewed on 2020-02-20 00:57_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1479 on 2020-02-20 00:57_

We definitely cannot use `self.rx.is_empty()`. The fact that the channel is empty does not imply that the traversal has finished. It could simply be that a worker is running and is _about_ to produce more work.

---

_@zsugabubus reviewed on 2020-02-20 01:22_

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1479 on 2020-02-20 01:22_

Cried and thinking a bit and I thought of something. Really hope not too big bullshit: Increment a counter before `tx.send`s and decrement it when we have completely finished with the work; basically a counter for “works pending”. 


---

_@BurntSushi reviewed on 2020-02-20 02:01_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1479 on 2020-02-20 02:01_

Possibly... I'd have to think more carefully about it and don't have the energy at the moment. My initial instinct is that it sounds like it might have the same problem though.

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1479 on 2020-02-20 02:38_

Some thoughts to make it clearer:
- The last running worker processes the last work. Counter is 1. (1 pending.)
- It adds a new work (W), but before it do so, it increments the counter by one (2 pending); so it ensures that `try_recv` + quit testing won’t initiate a quitting–like before–because even if (W) has been finished–counter will be >=1. And now it doesn’t matter which blocking or non-blocking `recv` will be woken up and when, because worker won’t exit only if it got an empty response and no pending works.
- Here, no pending work really mean that `rx.recv` will block indefinitely.
- The order of increments and decrements doesn’t count until they are well-paired (increment first, then decrement). The only value we care is 0. After reaching zero it never will be >0 (because the well-ordering).

If implemented well, it sounds obvious to me in plain English: If there are no pending works, there are nothing to do. We can quit now.

It was a bit hard to explain because my limited English skills and because it’s a tautology.

---

_@zsugabubus reviewed on 2020-02-20 02:38_

---

_@BurntSushi reviewed on 2020-02-20 20:56_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1479 on 2020-02-20 20:56_

I think I get it! And then we can remove the `waiting`/`resume` stuff entirely. Great. I have it implemented in #1491. 

---

_Review comment by @zsugabubus on `ignore/src/walk.rs`:1479 on 2020-02-20 21:09_

I’m glad I could help fix my mistake.

---

_@zsugabubus reviewed on 2020-02-20 21:09_

---
