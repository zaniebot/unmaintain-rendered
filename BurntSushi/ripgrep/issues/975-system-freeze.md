```yaml
number: 975
title: System freeze 
type: issue
state: closed
author: wagnerand
labels:
  - question
assignees: []
created_at: 2018-07-10T12:42:56Z
updated_at: 2018-07-23T19:57:25Z
url: https://github.com/BurntSushi/ripgrep/issues/975
synced_at: 2026-01-12T16:13:22Z
```

# System freeze 

---

_@wagnerand_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.8.1
-SIMD -AVX
```

#### How did you install ripgrep?

`brew install ripgrep`

#### What operating system are you using ripgrep on?

macOS 10.13.6

#### Describe your question, feature request, or bug.

I use `rg` to search on a USB 3.1 SDD drive connected to my Macbook Pro 15" (2017). The SSD contains over 700GB of data, mostly small code files and binaries, like pictures.

Pretty much any search I issue on this corpus makes the system unresponsive a few minutes after I started it. The system hangs last from a few seconds to several minutes and appear every few minutes, with seemingly increasing frequency over time.
Sometimes, I just see the mac-like *beachball*, sometimes I can still switch windows and scroll, but most interactions aren't executed right away, instead it seems they're put into some queue and then executed after the system is unblocked.

Example search: `rg -F files.com -i -uu`

---

_Comment by @BurntSushi on 2018-07-10 13:06_

At a high level, this kind of issue isn't something I can reproduce, so it's unlikely to get fixed without someone who _can_ reproduce it digging into this and debugging it on their own.

With that said, I have some tips that might help you debug it:

* Do you have your system's swap enabled? From the symptoms you describe, it _sounds_ like your system is churning through memory. That is, if ripgrep (or your system's page cache) is eating up all your memory and stealing it from other applications, then it's possible your application's memory use is spilling over to disk via swap. When you use those applications again, it needs to read memory back from disk, which can be quite slow!
* What kind of memory usage is ripgrep itself using? I don't use macOS, but you'll need to use whatever tooling there helps you discover this kind of information. `htop` is an easy way of doing this for long lived processes. Look at the `RES` column (short for "resident" memory). You should also note the `SHR` column as well, especially if it gets large. The `VIRT` column is likely uninteresting for this diagnosis.
* Is ripgrep just eating up all of your CPU? This is a banal explanation, but it's possible. Consider forcing ripgrep to only use a single thread with the `-j1` flag. You should also try this experiment if memory usage is a problem, since the memory usage profile for ripgrep is different between single threaded and multi-threaded mode.
* Another set of flags to try is `--no-mmap` and `--mmap`. Namely, try one search with `--no-mmap` and another with `--mmap`, and see how that impacts system performance along with memory usage. They should, in theory, have different memory usage profiles. Compare them with `-j1` and without `-j1` as well.
* How do other similar tools perform? What happens if you run `ggrep -F files.com -i -r` instead? (Where `ggrep` is GNU grep and is available via brew on macOS if I recall correctly.)

Also, you say that your SSD contains 700GB, but that this includes binary data like pictures. ripgrep should skip most of that.

---

_Comment by @wagnerand on 2018-07-10 15:03_

Thank for your prompt reply!

System swap is enabled, but that doesn't seem to be the issue. Swap remained small and stable for all cases where I could reproduce the system freeze. (about 350mb, which is fairly low).

RES memory also doesn't seem to be an issue. I saw it spiking up to 800mb, but usually it's much lower than that, 300-600mb. Neither `htop` nor Apple's *Activity Monitor* show SHR.

ripgrep is eating some CPU, but not all of it. I have a quad-core i7. All, load is distributed more or less evenly across the cores. What I found interesting is that, whenever the system freezes, *all* processes are going into *Paging* status (W) and the CPU usage drops, except for Core 0, which is at 100% capacity used solely by the `kernel_task` process.

Using `--no-mmap` was the only thing that seems to avoid the freeze. Maybe `-j1` too, but I need the parallelism because otherwise searches are taking forever.

Is `--no-mmap` significantly slower than `--mmap` for my use-case?

---

_Comment by @wagnerand on 2018-07-10 15:08_

Uhm, I guess I was too fast. I also freezes when using `--no-mmap`. Just took a little longer.

---

_Comment by @BurntSushi on 2018-07-10 15:17_

Thanks for the detailed response. Based on the information you've given, my best guess is that swap is indeed the problem, or that your system is somehow not coping well with the additional memory usage. You also haven't said how much RAM your machine actually has, which makes it a bit difficult to guess here. Moreover, the _amount_ of swap your system is using isn't really relevant here, what matters is that it's being used, and also, _what_ is using it. Just because only 350MB is being used doesn't mean that isn't where all of your applications' memory has been paged to. If an applications memory is put into swap, then the decrease in CPU usage and overall slugginess is a _classic_ symptom of that.

Note that memory usage here doesn't just mean what ripgrep uses. It's also about what the OS decides to put into your system's I/O cache, which is getting put through its paces here by searching a large amount of content.

> Maybe -j1 too, but I need the parallelism because otherwise searches are taking forever.

I mean, you only have a quad core. At best, you're going to get a 4x speed up (maybe 5x with HT), so I wonder what the difference is between "taking forever" and "taking forever divided by 4." Moreover, we are trying to **debug** the issue here. Let's not mix trying to find the cause with possible solutions. Could you please try experimenting with `-j1` and the combinations of `--mmap` and `--no-mmap`?

> Is `--no-mmap` significantly slower than `--mmap` for my use-case?

When searching a directory, `--no-mmap` is the default. `--mmap` in theory shouldn't be significantly slower, but it depends.

---

_Comment by @wagnerand on 2018-07-10 15:23_

> You also haven't said how much RAM your machine actually has, which makes it a bit difficult to guess here.

Oops, I meant to add that. The system has 16G of RAM, during all testing, usage never exceeded 8G.

> If an applications memory is put into swap, then the decrease in CPU usage and overall slugginess is a classic symptom of that.

I know what you are saying but this is different. The system freezes entirely, at some point, the only thing that I can do is moving the mouse around. Applications that get swapped shouldn't freeze the entire system for minutes.

> Could you please try experimenting with -j1 and the combinations of --mmap and --no-mmap?

Please see my added comment. It looks like `--no-mmap` vs `--mmap` doesn't make much of a difference. I can retry with `-j1` though.

---

_Comment by @BurntSushi on 2018-07-10 15:28_

> Applications that get swapped shouldn't freeze the entire system for minutes.

The operative word here is "shouldn't." In my experience, this is not at all what actually happens. I'm speaking from my own experience with using Linux with swap enabled, or even Linux without swap enabled but before the OOM killer can work its magic. "waiting multiple minutes for the system to unclog itself" is _exactly_ what I've experienced.

In any case, it's pointless to debate this. Please remember what I said in my initial response to you:

> At a high level, this kind of issue isn't something I can reproduce, so it's unlikely to get fixed without **someone who can reproduce it digging into this and debugging it on their own.**

All I can do is give you my advice and some debugging techniques. You are free to reject my experience. I could in fact be wrong! But I'm telling you what it is I believe based on the data available to me.

---

_Comment by @wagnerand on 2018-07-10 15:30_

Got it, thanks! I am happy to dig into this and debug myself, but I guess I would need some guidance for that.

---

_Comment by @BurntSushi on 2018-07-10 15:37_

I'm not sure there are any specific code changes that need to be made to ripgrep. The next step I would personally try if I were you would be to attempt to refine the swap hypothesis. The most obvious way to do that, IMO, is to disable swap and see what happens. If things work better, then we have pretty compelling evidence that it's swap. If things don't work better, then it's possible that swap isn't the problem, but I think it may be difficult to draw a strong conclusion. At least, I don't feel comfortable doing so because I'm not familiar with how macOS systems behave in those scenarios.

If things do work better without swap enabled, then the hypothesis there is that your application's memory would never get paged out and as a result, the size of your I/O cache would be more forcefully limited. The I/O cache size is unlikely to impact the performance of ripgrep in this case, since if you are indeed actually searching 700GB (although that isn't actually clear to me, since I don't know what ripgrep is skipping due to binary data detection), then the I/O cache size is unlikely to be a factor since ripgrep is going to be blocked on disk anyway.

---

_Comment by @wagnerand on 2018-07-10 17:12_

I disabled swapping, and tried with and without memory maps, without any luck. The system freezes shortly after starting rg.

---

_Comment by @BurntSushi on 2018-07-10 17:18_

Sadly, I'm out of ideas. I've tried reproducing this on a very large directory of media files (enabling binary search), and ripgrep behaves as expected: fairly low memory usage and low CPU usage because it's disk bound.

I doubt there is much more I can do without the ability to reproduce your problem. You're probably on your own.

Keep in mind that you have still left out important data:

* The impact of using `-j1`.
* The impact of similar tools such as `ag` and/or `ggrep`.

---

_Comment by @cyrossignol on 2018-07-12 00:58_

This really sounds like a hardware, kernel module (driver), or power/temperature management issue, possibly related to the specific SSD or its USB driver. I seems very unlikely that ripgrep causes the problem directly.

As suggested, invoking `rg` with `-j1` may help by limiting ripgrep to one core so it generates less heat or concurrent IO (I seem to remember an issue with Macs preemptively throttling all non-system processes under certain conditions to keep the temperature lower). 

Running `ag` with enough workers to exercise all the cores may help to support this theory if we see the same behavior. 

---

_Label `question` added by @BurntSushi on 2018-07-12 17:03_

---

_Comment by @BurntSushi on 2018-07-22 13:10_

I don't think there's anything actionable on my end here. In theory, there are more debugging steps that could be performed to diagnose this issue, but at this point, it's up to the OP.

---

_Closed by @BurntSushi on 2018-07-22 13:10_

---

_Comment by @wagnerand on 2018-07-23 19:05_

Sorry, work has kept me more than busy.

Running with `-j1` has similar effects, although much less impactful. The system hangs are usually less than a second.
I didn't have time to test with `ag` or `ggrep` yet.

Would you like me to keep updating this issue?

---

_Comment by @BurntSushi on 2018-07-23 19:57_

More data is appreciated. If there is anything actionable on my end, I can
reopen, but I do suspect this debugging task is largely on your hands.

On Mon, Jul 23, 2018, 15:05 Andreas Wagner <notifications@github.com> wrote:

> Sorry, work has kept me more than busy.
>
> Running with -j1 has similar effects, although much less impactful. The
> system hangs are usually less than a second.
> I didn't have time to test with ag or ggrep yet.
>
> Would you like me to keep updating this issue?
>
> —
> You are receiving this because you modified the open/close state.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/975#issuecomment-407167185>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34oRHONAqDiD4M39fbjO8FKu1apWNks5uJh6VgaJpZM4VJVP9>
> .
>


---
