```yaml
number: 1935
title: Progress bar for search
type: issue
state: closed
author: elazarl
labels:
  - wontfix
assignees: []
created_at: 2021-07-13T18:17:40Z
updated_at: 2021-07-15T15:02:08Z
url: https://github.com/BurntSushi/ripgrep/issues/1935
synced_at: 2026-01-12T16:13:24Z
```

# Progress bar for search

---

_@elazarl_

#### View progress when searching large files

When searching a large file, one would like to view the progress of the search, to estimate when it is done.

For example

```
rg --progress needle haystacks/*.txt >results
[===>             ] 32/64 files, 5G/50G Worked for: 3:00min ETA: 5:00min
```

This is sort-of achieavable with something like

```
pv haystacks/*.txt | rg needle
```

However, you lose file names, line numbers, and also lose the ability to `mmap` the file. Also, I assume the piping of the data can hurt performance in some cases (e.g. tmpfs), but I'm not entirely sure.

While this is a nice feature for ripgrep, it's even more essential for `libripgrep`, where progress might be essential for correctness. So adding, e.g., progress report to `Sink` would be even more beneficial.

---

_Comment by @BurntSushi on 2021-07-13 18:23_

This isn't really a practical thing to do unfortunately. A progress bar implies some notion of when or after how much work the search will complete. But this is very specifically not ever known, because ripgrep processes files in an incremental fashion as it descends the directories provided to it to search. Your progress bar doesn't stop there either. It doesn't just want the number of files, but also the amount of disk space searched. That also requires a stat call for every single file, which has non-trivial overhead.

Overall, ripgrep just is not designed in a way that makes it easy to accrue all search candidates in advance, and _then_ search them. It isn't designed that way because that isn't the fastest way to execute a search. Therefore, adding a progress bar would require adding that design _in addition_ to what ripgrep currently does to avoid a performance regression for all searches. That makes this feature quite complex and a maintenance burden for me in particular.

> While this is a nice feature for ripgrep, it's even more essential for `libripgrep`, where progress might be essential for correctness.

Can you unpack this? It isn't clear to me what you mean here, particularly with respect to the phrase "essential for correctness."

---

_Label `question` added by @BurntSushi on 2021-07-13 18:24_

---

_Comment by @BurntSushi on 2021-07-13 18:27_

> While this is a nice feature for ripgrep, it's even more essential for `libripgrep`, where progress might be essential for correctness. So adding, e.g., progress report to `Sink` would be even more beneficial.

Also, this is a categorically different request than the first half of the issue. The progress bar you showed as an example has a granularity at the file level, but this request is asking for a granularity of the bytes inside of each file. This is also not going to happen, because the only robust way to implement it is to hook into the regex engine, and ripgrep does not have its own bespoke regex engine. And no general purpose regex engine is going to provide said hook.

---

_Comment by @BurntSushi on 2021-07-13 18:31_

If you have a UI on top of ripgrep (e.g., in a text editor), it is in theory possible to build a progress bar at the granularity of a file on top of ripgrep. ripgrep would need to expose a flag for [enabling an option to emit JSON `begin`/`end` messages for each file even when there are no matches in that file](https://docs.rs/grep-printer/0.1.6/grep_printer/struct.JSONBuilder.html#method.always_begin_end). But once available, ripgrep will print something after every file it searches, which would give you a way to update the progress bar. Of course, the UI would first need to figure out how many files (and possibly how much disk storage) ripgrep would search, which probably means running `rg --files <args>` first. So... a lot of work, but possible.

---

_Comment by @elazarl on 2021-07-13 18:55_

Re possibility. Can't you descent and search for all relevant files before starting to work, in case user asks for progress bar? Sometimes user is willing to pay with throughput to see progress and get ETA (e.g., overall search would take one minute longer, for displaying progress through the one hour of search)

Maybe I'm missing something, but if all files are known in advance, and their sizes are known, wouldn't knowing `bytes scanned/total bytes to scan` give as a coarse grain evaluation of how much work is left?

Re hook for regex engine. Let's say we won't provide that functionality for multiline. Can't we get bytes processed estimation from the line iterator?

I might be entirely missing something here, but please do enlighten me.

---

_Comment by @elazarl on 2021-07-13 18:58_

Re correctness, I'm sometimes required to stop, serialize my state, and continue later. This can be possible if `Sink` is getting some coarse grained file seek information from time to time. Maybe I was exaggerating with "correctness", but that's an example for more essential feature than "just" a nicer UI.

---

_Comment by @BurntSushi on 2021-07-13 18:59_

> Can't you descent and search for all relevant files before starting to work, in case user asks for progress bar?

That's exactly what I considered in my comment.

> Maybe I'm missing something, but if all files are known in advance, and their sizes are known, wouldn't knowing bytes scanned/total bytes to scan give as a coarse grain evaluation of how much work is left?

Again, I addressed this.

> Re hook for regex engine. Let's say we won't provide that functionality for multiline. Can't we get bytes processed estimation from the line iterator?

I don't want to "not provide it" for multi-line. And the line iterator is the slow approach for searching.

> Re correctness, I'm sometimes required to stop, serialize my state, and continue later. This can be possible if Sink is getting some coarse grained file seek information from time to time. Maybe I was exaggerating with "correctness", but that's an example for more essential feature than "just" a nicer UI.

Yeah, that's not a good fit for ripgrep. This implies a total redesign of how libripgrep works. You're better off building something bespoke if you need this.

---

_Closed by @BurntSushi on 2021-07-13 18:59_

---

_Label `question` removed by @BurntSushi on 2021-07-13 19:00_

---

_Label `wontfix` added by @BurntSushi on 2021-07-13 19:00_

---

_Comment by @elazarl on 2021-07-15 11:25_

Sorry to bother, but another idea popped to my mind:

The `Sink` API would expose reading method to the sink. Which could be file descriptor, or mmap address.

Then, with a different OS specific package, the Sink could trace the progress of the search regardless of the implementation. In Linux this can be done by tracing major PF in the mmap'ed file via /proc/self/smap, or, watching the file descriptor progress in the given file.

This is a minor change that doesn't seem to break anything.

What do you think?

---

_Comment by @BurntSushi on 2021-07-15 11:42_

The caller is responsible for creating the `Sink` and is _also_ responsible for handing the input to the searcher. For example, there is a [`Searcher::search_file`](https://docs.rs/grep-searcher/0.1.8/grep_searcher/struct.Searcher.html#method.search_file) API. So you should be able to provide the file descriptor at least to your `Sink` impl. Getting the mmap address is a little trickier, but could be exposed as a method on `Searcher`. (Every `Sink` method gets a `&Searcher` provided to it.)

---

_Comment by @elazarl on 2021-07-15 12:04_

@BurntSushi however, the searcher can (and I want him) to choose the strategy automatically, it'd also mmap the file for me, which is also desirable.

The searcher is the domain expert, I don't want to decide for him how to handle the search. I just want him to let me know how he decided to search.

Isn't that a relatively small and undistruptive change?

---

_Comment by @BurntSushi on 2021-07-15 12:15_

> however, the searcher can (and I want him) to choose the strategy automatically, it'd also mmap the file for me, which is also desirable.

That's what `search_file` does.

> Isn't that a relatively small and undistruptive change?

Can you stop saying this please? It's a leading question. And you repeating this in a variety of different forms comes across as _really_ pushy. More to the point, you've only described your API addition as an idea. You haven't actually written down what specific concrete API you want. On top of all of that, I did suggest adding something to the `Searcher` API, but you seem to have ignored that.

---

_Comment by @elazarl on 2021-07-15 12:37_

@BurntSushi I apologize, I probably misunderstood you.

I thought you were suggesting the caller would provide either a file descriptor or an mmap file, but not a path like he can do today.

To which I said the functionality of choosing the right strategy automatically is a feature of the library the user might still want to use, even if he wants to measure progress.

I reread what you wrote but still didn't understand your suggested solution.
Can you please elaborate on your suggestion? I really did try not to ignore what you said.

I didn't write an API yet, since I thought to discuss the high level details beforehands. If you think that'd make things clearer - I'll try to write a concrete API.

And I apologize for my question.
I was genuinely trying to understand if there's something complicated in the suggested approach (e.g., I was expecting an answer like "no, actually it's complicated b/c of X Y Z", and I apologize if I sounded offensive. Didn't mean to.

---

_Comment by @elazarl on 2021-07-15 12:40_

(And on personal note, I really enjoy `libripgrep` and `ripgrep`, I marvel at the engineering and effort. And this is actually part of my motivation to try and contribute. I really try not to offend and to discuss changes respectfully, I might have failed doing that, and I apologize if that was the case)

---

_Comment by @BurntSushi on 2021-07-15 12:53_

Thanks.

> I reread what you wrote but still didn't understand your suggested solution.
> Can you please elaborate on your suggestion? I really did try not to ignore what you said.

From your comment above, you asked to make the file descriptor or the mmap address available to `Sink`. The file descriptor part of this request is already resolved today. Because you can create a `File` and hand that off to a `Searcher`. The searcher may still use mmap in this case though, so with the current API, you cannot tell whether the `File` is being used with standard `read` syscalls or if it was used to back a mmap.

What I suggested was possibly adding a method to the `Searcher` API that returns the mmap address. e.g., `fn mmap_raw(&self) -> Option<*const u8>`. If that returns a `Some`, then you know the searcher is using a memory map. I hope that helps you understand my idea better.

Note that I just realized that this isn't quite enough. A `Searcher` has three ways of searching: 1) incrementally using `read` syscalls, 2) via memory map or 3) by reading the entire file on to the heap. So your idea covers (1) and (2), but not (3).

---

_Comment by @elazarl on 2021-07-15 13:17_

The API you suggested is more or less what I had in mind, it's just that IMHO instead of "hardcoding" a specific method, a clearer API would be `raw_read_method` that returns an enum of possible methods.

This would allow using `path` to start the search, and still have access to the file descriptor chosen without explicitly opening it, and would allow adding other strategies later.

But that's not a really big difference.

Regarding 3, this is indeed trickier, but can you please elaborate on what exactly happens in (3):

1. Do you start searching while reading the entire file?
2. What is the heuristic used to decide on using (3) can it happen for really large files?

---

_Comment by @BurntSushi on 2021-07-15 13:25_

>     1. Do you start searching while reading the entire file?

The entire contents of the file is put on to the heap. Only then is the data searched. I don't see any way to measure progress of that search at the OS level.

> What is the heuristic used to decide on using (3) can it happen for really large files?

The most precise way of saying it is: "when neither (1) nor (2) can be used." It's a catch-all.

Memory maps have plenty of cases where they can't or won't be used.

In general, incremental reading can always be used. But, it can't when multi-line mode is enabled. So if memory maps can't be used for one of a number of reasons and multi-line search is enabled, then the whole file is read on to the heap.

---

_Comment by @elazarl on 2021-07-15 14:13_

1. If that is the case, I _think_ you can get a pretty good progress estimation with CPU performance counters, in linux this can be achieved with `perf_event_open`, which can measure RAM throughput which should be dominated by data, if the file is pretty long.

An alternative approach which does not require perf counters, is putting a trap pages checkpoint every few MBs, handling the segfault by mprotecting and continuing. While this might give a very small (and tunable) throughput hit, it's a pretty good measure.

(For very short files, measurement is probably not worth the effort anyhow).

This is probably relevant for multigigabyte files which are read to the heap, but I'm not sure how common this case is.

How does ripgrep know if it has enough memory to read the entire file to the heap?

I think I'll start tackling files by measuring how much time does it take to search a large file that fits the heap on a typical system, and then we can understand how relevant this case is.

---

_Comment by @elazarl on 2021-07-15 14:15_

I think that a reasonable design rational for enabling this feature is:

1. libripgrep will _not_ provide any hooks or change its internal implementation.
2. If possible libripgrep will provide low level information to assist user with measuring the search. This could be the mmap'd range, the file descriptor or the memory range to which the file was read entirely.

---

_Comment by @BurntSushi on 2021-07-15 14:19_

> How does ripgrep know if it has enough memory to read the entire file to the heap?

It doesn't. Just like it doesn't know if it has enough memory to fit any single line on the heap. GNU grep has the same limitation. If either tool doesn't have enough memory, then at least on Linux, the OOM killer comes into play.

>     2\. If possible libripgrep will provide low level information to assist user with measuring the search. This could be the mmap'd range, the file descriptor or the memory range to which the file was read entirely.

This seems plausible to me. I would want to better understand (and possibly document) how this information can be used to measure progress.

---

_Comment by @elazarl on 2021-07-15 14:49_

> > How does ripgrep know if it has enough memory to read the entire file to the heap?
> 
> It doesn't. Just like it doesn't know if it has enough memory to fit any single line on the heap. GNU grep has the same limitation. If either tool doesn't have enough memory, then at least on Linux, the OOM killer comes into play.

What would happen if someone tries to grep a 40gb file, which cannot be mmap'd?

Would `ripgrep` prefer copy-to-heap than reading fd, since it enables multiline, and then would try to read the entire file and fail? I _think_ I'm misunderstanding something here.

> 
> This seems plausible to me. I would want to better understand (and possibly document) how this information can be used to measure progress.

Here's what I suggest as a way forward, I'll open an RFP PR with implementation of measuring progress with mmap, say, as an example code, and you'll have a look and let me know if it seems plausible. If it looks OK I'll move forward implementing it with file descriptor. I'll try to ask a friend to help with similar implementation for Windows to make sure we're not too Linux centric.

Does that make sense?


---

_Comment by @BurntSushi on 2021-07-15 15:02_

> Would `ripgrep` prefer copy-to-heap than reading fd, since it enables multiline, and then would try to read the entire file and fail?

Correct. It's either that, or don't do mulit-line search correctly.

> Here's what I suggest as a way forward, I'll open an RFP PR with implementation of measuring progress with mmap, say, as an example code, and you'll have a look and let me know if it seems plausible. If it looks OK I'll move forward implementing it with file descriptor. I'll try to ask a friend to help with similar implementation for Windows to make sure we're not too Linux centric.

I think so.

---
