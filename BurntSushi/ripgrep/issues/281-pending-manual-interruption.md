```yaml
number: 281
title: "[PENDING] Manual interruption"
type: issue
state: closed
author: sergeevabc
labels:
  - bug
  - question
assignees: []
created_at: 2016-12-15T01:31:47Z
updated_at: 2024-02-01T00:28:47Z
url: https://github.com/BurntSushi/ripgrep/issues/281
synced_at: 2026-01-12T16:13:21Z
```

# [PENDING] Manual interruption

---

_@sergeevabc_

Let’s launch ```rg.exe 1``` and… What a colossal, incessant output!
But how could one interrupt the process assuming the launch was a mistake?
Ctrl+C in Windows is the expected way to go. But it does NOT work now. 

---

_Comment by @magikid on 2016-12-18 14:39_

I came across this bug on Ubuntu this morning as well.  I ran `rg --files ./*` in my home directory.  When I realized that it was going to print every file in my home directory, I tried to `ctrl-c` to kill it but that was ignored.  Luckily `ctrl-z` was still recognized so I could manually kill the process.

Do you think this is related to #200? 

---

_Comment by @BurntSushi on 2016-12-18 16:19_

It is not related to #200.

In the case of `--files` at least, I understand the bug. The problem is that the `^C` handler tries to acquire a lock on `stdout` to clear color codes, but the way `--files` is currently written is that it holds that lock for the entirety of its operation. This shouldn't be too hard to fix.

I can't reproduce this under normal search operation though (on Linux). I'll need to explicitly try it on Windows.

---

_Label `bug` added by @BurntSushi on 2016-12-18 16:19_

---

_Label `question` added by @BurntSushi on 2016-12-18 16:19_

---

_Comment by @BurntSushi on 2016-12-24 15:49_

`^C` seems to work fine inside of `mintty`, but I can indeed reproduce this issue inside of `cmd.exe`. 

---

_Comment by @BurntSushi on 2016-12-24 17:50_

I would like to remove the `^C` handling in ripgrep, whose only purpose is to reset color codes. This is a nice convenience because if you `^C` in the middle of writing colored text, the terminal will take on whatever style was enabled at that point. There are many problems with this though:

1. It doesn't work on Windows at all. Firstly, the `^C` handling itself doesn't work inside MSYS2 terminals. Secondly, while the `^C` handling works in a normal Windows console, it isn't actually capable of resetting anything since there is no equivalent "clear" operation like there is in ANSI terminals.
2. Even when all of the above works, it is still imperfect in ANSI terminals. Namely, the normal process for writing to `stdout` acquires a lock to make throughput as fast as possible, but resetting the terminal also requires writing to `stdout`. Therefore, there must be some kind of synchronization between printing to stdout and resetting the terminal, which is not ideal. If we "just ignore" this problem, then you wind with the problem of not being able to kill ripgrep while it's searching a single large file, which I think is really terrible UX.

All told, my feeling is that getting the color reset handling right just isn't worth it. If you run into the problem, it's easy to fix by simply running:

```
$ echo -ne "\033[0m"
```

cc @lambda 

---

_Closed by @BurntSushi on 2016-12-24 17:53_

---

_Comment by @hacst on 2017-01-20 21:19_

This effect is kinda unfortunate. Triggered it the first time I tried rg.

FYI: Calling ```color``` in cmd.exe will reset the colors back to how they were on console startup.


---

_Comment by @BurntSushi on 2017-01-20 22:06_

I understand it is unfortunate. I'm open to better ideas.

---

_Comment by @seabadger on 2017-03-28 12:47_

Perhaps a silly question, but can't you just have an atexit handler that always resets colors, and the ctrl-c just ensures regular processing is interrupted s. t. exit() is called (after releasing any resources)?  

---

_Comment by @BurntSushi on 2017-03-28 13:21_

@seabadger Could you address this particular point from my comment above?

> Namely, the normal process for writing to stdout acquires a lock to make throughput as fast as possible, but resetting the terminal also requires writing to stdout. Therefore, there must be some kind of synchronization between printing to stdout and resetting the terminal, which is not ideal.

In particular, this is exactly the problem:

> the ctrl-c just ensures regular processing is interrupted 

(FYI, `atexit` specifically is a red herring.)

---

_Comment by @seabadger on 2017-03-28 14:49_

First -- a disclaimer: I may be coming at this from the point of ignorance, as (1) I've just discovered this project and only looked at small parts of the code so far, (2) don't know Rust so may be missing something even for the parts that I did look at, and (3) don't know what solutions were already considered and discarded beyond the one in this issue. So my apologies if I'm mentioning something obvious and/or obviously wrong.

That said: if I understood correctly, the implementation that was removed by commit  de5cb7d22e00c18dfce082a68b0cf45924b56e7f did the work (cleanup + exit) directly in the signal handler, which indeed required this kind of synchronization.

Would it be feasible to instead defer the actual work to a safer point? 
E.g., the signal handler would set a boolean flag; the worker threads would eventually see the flag is set and terminate.
Once all the workers terminate, presumably no further output is being produced, so cleanup (i.e., color reset) can be safely performed.

This assumes:
- worker threads are not detached, so there's a join point after which the cleanup can be performed (I see that at least some threads are joined, but I don't know if that's true for all cases).
- the flag check in the workers is inexpensive (or can be made inexpensive by only doing it relatively infrequently).

Hope I didn't miss something glaringly obvious...

(and you're right about ```atexit```, of course).




---

_Comment by @BurntSushi on 2017-03-28 15:09_

Thanks for the ideas!

> the flag check in the workers is inexpensive (or can be made inexpensive by only doing it relatively infrequently).

Right. This is what the "there must be some kind of synchronization between printing to stdout and resetting the terminal" part is about. It would have to be pushed all the way down into the input buffer handling itself. It would not be nice, and I can almost guarantee that it will be a source of bugs of the form "I hit `^C` and ripgrep took a few seconds to quit."

---

_Comment by @seabadger on 2017-03-28 15:31_

(To be fair, a large recursive grep or 'find | xargs grep'  would not necessarily respond to Control-C instantaneously (speaking from experience) -- so at least from the user's perspective, I think I would find this acceptable; can't speak for others, of course. But the effect on the code structure remains, so here I'll defer to you)

How about another approach that (I think) addresses both issues: a wrapper.
Basically, the parent process (which could be rg itself):
- sets up a signal handler (used by parent only)
- forks a child process (which would remove the signal handler)
- waits for child to terminate
- cleans up

The parent's signal handler would simply pass the signal on to the child process. The child process should behave exactly as rg behaves today.

Cons:
- cost of fork(). Unless rg is run in  a tight loop, or the binary size grows significantly, I hope this would not be very noticeable (am I missing common use cases?)

Pros:
- avoids the 'slow to respond' problem
- should not be very intrusive with respect to most of the code.



---

_Comment by @BurntSushi on 2017-03-28 15:37_

@seabadger Hmmmmmmmm..... I like that. I can't immediately think of a counter-argument to that. My only caveat is that it would be nice if it worked on Windows, although I suppose it wouldn't have to be a hard requirement. (It could be something that only works on Unixish systems.)

---

_Comment by @BurntSushi on 2017-03-28 15:38_

One other point: I don't have good intuition as to the overhead of a single `fork` call, but I do expect ripgrep to be used in some `xargs` pipelines occasionally, which could suffer from that overhead, but it feels like it'd be awfully small to me.

---

_Reopened by @BurntSushi on 2017-03-31 09:16_

---

_Comment by @DoumanAsh on 2017-03-31 11:00_

@BurntSushi After a brief look at code what i can see is that std get locked once rg runs in one thread. Is it actually necessary to lock stdout in one thread runner?
P.s. i'm still trying to get hand of multi-threaded code

---

_Comment by @BurntSushi on 2017-03-31 11:10_

@DoumanAsh Could you please read this thread? I think the current best solution is a fork-exec that was written up here https://github.com/BurntSushi/ripgrep/issues/281#issuecomment-289808716 --- I just don't know if it works on Windows.

> Is it actually necessary to lock stdout in one thread runner?

Yes. Locking `stdout` prevents interleaving.

---

_Comment by @DoumanAsh on 2017-04-02 14:01_

@BurntSushi I started implementing  with `command` instead of `fork` but it seems to be rather ugly (at least to me)
https://github.com/DoumanAsh/ripgrep/commit/0d141ec4f81617ab89bff7ec1c809ee862b4b104

It is rather difficult to detect whether you are invoked by user or yourself unless i specify some hidden option.
I think maybe it would be better to spent some efforts into researching Windows's fork replacement

---

_Comment by @BurntSushi on 2017-04-02 14:03_

@DoumanAsh Yeah, a hidden flag my unfortunately be necessary.

---

_Comment by @seabadger on 2017-04-02 14:14_

Is it possible to find out the name of your parent process? If so, that might be an  alternative to a hidden option (ot equivalently hidden environment variable) 

---

_Comment by @DoumanAsh on 2017-04-02 14:28_

@seabadger I'm not aware of any way. The only thing we can reliable to find is zero argument of our process (name of binary or how it has been started) but if you have any hints...?

**UPD:** But we could start Command with special environment variable which actually might be better

---

_Comment by @DoumanAsh on 2017-04-02 15:06_

@seabadger I went with setting particular environment variable. It should reliable enough for us to use as chance of collision is pretty small.

PR #281

---

_Comment by @jethrogb on 2017-04-10 06:26_

The child process sounds like a nice solution, but it's kind of a big hammer for a small problem. Why not just ignore the lock on stdout and write the terminal color reset code directly to fd 1?

---

_Comment by @BurntSushi on 2017-04-10 08:24_

If that actually works, then I would be okay with it. termcolor would need
to support that type of access directly. The fundamental problem is that
your solution isn't actually guaranteed to work.

On Apr 10, 2017 2:26 AM, "jethrogb" <notifications@github.com> wrote:

> The child process sounds like a nice solution, but it's kind of a big
> hammer for a small problem. Why not just ignore the lock on stdout and
> write the terminal color reset directly to fd 1?
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/281#issuecomment-292860321>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34ki_oLPcNORhtkj2T7bZz5N1FA9iks5rucuIgaJpZM4LNnaR>
> .
>


---

_Comment by @DoumanAsh on 2017-04-10 09:54_

@jethrogb Your solution would not work because, at least on windows, your handler of Crtl-C is called in a separate thread(from main thread) and it is highly likely that main thread is able to print some more colors before being actually terminated

---

_Comment by @BurntSushi on 2017-04-10 10:38_

@DoumanAsh I think @jethrogb's proposal relies on it *not* being likely that the main thread will print more colors. That's what I meant by "it's not guaranteed to work." However, it may work well in practice. The vast majority of printer execution is spent writing the actual results, and not tweaking the color configuration. (In any case, Windows is a red herring. I imagine @jethrogb's fix would work equally well on both Unix and Windows.)

---

_Comment by @jethrogb on 2017-04-10 17:18_

We discussed a quit flag before, I think that idea was shot down because the lock is being held somewhere high up the call chain, but if we only care about "not printing" and not about "releasing the lock", maybe using a quit flag is ok again.

Alternatively, you can call TerminateThread/pthread_kill on all other threads in the signal handler.

---

_Comment by @sergeevabc on 2017-07-20 23:11_

Issue was created about 0.3.2, tested 0.5.2 right now, still not fixed alas.

---

_Comment by @BurntSushi on 2017-07-20 23:17_

The original bug is fixed because ripgrep no longer does ^C handling.

---

_Comment by @sergeevabc on 2017-07-20 23:22_

Steps to reproduce:
1. Put ```rg.exe``` into some folder with a lot of files.
2. Launch ```rg 1```.
3. Press Ctrl+C (or Ctrl+Break) to interrupt output.
Result: nothing happens, output proceeds with no opportunity to stop.
Expected: output is interrupted right away, focus goes back to command line (e.g. [Sift][1] respects that).

[1]: https://sift-tool.org/ 

---

_Comment by @BurntSushi on 2017-10-22 02:01_

I've finally gotten a chance to test this on Windows and this is definitely fixed. I can `^C` ripgrep in either PowerShell or cygwin and it reliably stops.

---

_Closed by @BurntSushi on 2017-10-22 02:01_

---

_Comment by @th1000s on 2024-02-01 00:28_

FYI, the saga continues, see #2727 - now with even more syscalls! :)


---
