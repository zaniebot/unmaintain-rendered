```yaml
number: 2768
title: "perf: Use custom allocator"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: perf/custom-allocator
created_at: 2023-02-11T16:21:29Z
updated_at: 2023-02-15T11:38:37Z
url: https://github.com/astral-sh/ruff/pull/2768
synced_at: 2026-01-12T04:52:01Z
```

# perf: Use custom allocator

---

_Pull request opened by @MichaReiser on 2023-02-11 16:21_

This PR replaces the system allocator with a custom allocator to improve performance:

* Windows: mimalloc
* Unix: tikv-jemallocator

## Performance:

* Linux
  * `cpython --no-cache`: 208.8ms -> 190.5ms
  * `cpython`: 32.8ms -> 31ms
* Mac: 
  * `cpython --no-cache`: 436.3ms -> 380ms
  * `cpython`: 40.9ms -> 39.6ms
* Windows: 
  * `cpython --no-cache`: 367ms -> 268ms
  * `cpython`: 92.5ms -> 92.3ms
  
## Size

* Linux: +5MB from 13MB -> 18MB (I need to double check this)
* Mac: +0.7MB from 8.3MB-> 9MB
* Windows: -0.16MB from 8.29MB -> 8.13MB (that's unexpected)

---

_Comment by @charliermarsh on 2023-02-11 17:07_

Nice! Are there any costs or risks associated with this apart from the increased binary size? Have you used custom allocators in previous projects? I've seen them mentioned in [Nethercote's book](https://nnethercote.github.io/perf-book/heap-allocations.html#using-an-alternative-allocator) but never tried them :)

---

_Comment by @MichaReiser on 2023-02-11 17:26_

We use the same setup at Rome and haven't had any issues (but it greatly improved performance) [[PR1](https://github.com/rome/tools/pull/3237), [PR2](https://github.com/rome/tools/pull/2900)]

The book you mention summarises it well

> The exact effect will depend on the individual program and the alternative allocator chosen, but large improvements in speed and very large reductions in memory usage have been seen in practice. The effect will also vary across platforms, because each platform’s system allocator has its own strengths and weaknesses. The use of an alternative allocator can also affect binary size.

We could try to measure peak memory usage and compare it between allocators. 

The main risk I see is that it introduces an additional surface that needs maintaining. We need to update the allocator when it has a known security issue and it may be more likely that a custom allocator has bugs which I would expect less from the system allocators. 

---

_Comment by @charliermarsh on 2023-02-11 17:40_

(Thumbs-up from me.)

---

_Comment by @MichaReiser on 2023-02-11 17:45_

I'm undecided whether the performance improvement on Linux warrants such a significant binary size increase.

But, using jemalloc on all unix platforms has a harmonizing effect, reducing the differences between the two platforms. 

---

_Marked ready for review by @MichaReiser on 2023-02-11 17:45_

---

_Comment by @MichaReiser on 2023-02-11 17:53_

The build failure looks unrelated to these changes.

---

_Merged by @charliermarsh on 2023-02-11 18:26_

---

_Closed by @charliermarsh on 2023-02-11 18:26_

---

_Comment by @andersk on 2023-02-11 20:30_

Much of the size difference on Linux is due to debug symbols. (NixOS 23.05 on Ryzen 7 1800X, averages of 100 runs.)

allocator | size | stripped | cpython time | cpython RSS memory
--- | --- | --- | --- | ---
system | 13.01 MiB | 9.32 MiB | 407 ms | 131 MiB
jemalloc | 18.25 MiB | 9.92 MiB | 378 ms | 175 MiB
mimalloc | 13.04 MiB | 9.37 MiB | 454 ms | 202 MiB
mimalloc (no `secure`) | 13.03 MiB | 9.36 MiB | 401 ms | 197 MiB


---

_Comment by @andersk on 2023-02-11 20:38_

The memory use change is a bit concerning though—with Ruff already being as blindingly fast as it is, is an 8% speed increase really worth a 34% memory use increase?

---

_Comment by @charliermarsh on 2023-02-11 20:51_

Ahhh that's really interesting, thank you for running + tabulating. And indeed that's a bit concerning. I was going to say that perhaps it makes sense to ship on Windows but not elsewhere given that the speed increase in the PR summary is much more significant, but presumedly (?) that would result in a similar memory use increase as the `mimalloc` benchmarks above.

---

_Comment by @charliermarsh on 2023-02-11 22:39_

@MichaReiser - Should we consider putting this behind a flag while we continue to discuss, based on the above?

---

_Comment by @MichaReiser on 2023-02-12 07:40_

@andersk, what command did you use to get the peak memory usage? 

I used mac's instruments to get some numbers before and after.

<table>
<tr>
	<td>system allocator
	<td>jemalloc
<tr>
	<td><img width="1792" alt="Memory usage with system allocator" src="https://user-images.githubusercontent.com/1203881/218298010-0ed86279-b2d4-42a3-9876-2d8bb36a84b8.png"> 
	<td><img width="1792" alt="Memory usage with jemalloc allocator" src="https://user-images.githubusercontent.com/1203881/218298026-244b4f99-ae8f-4107-97d3-32f804bb66db.png">
<tr>
	<td>Peak: 42MB
	<td>Peak: 145MB
</table>

This looks very concerning but these measurements aren't necessarily fair. My understanding of the numbers shown by Instruments is that it shows `total(malloc) - total(free)` over time. This is useful for understanding the allocations in your program but only has limited meaning in assessing how much physical memory your application uses because it doesn't account for heap fragmentation and the fact that the OS allocates the memory in pages (~4Kib). Meaning, you can have a situation where the OS isn't able to free a single page because your application has freed all allocations except one on every page. 

Using `jemalloc` gives you an entirely different graph because `jemalloc` handles the memory pages manually. It is an abstraction on top of `malloc` and `free`. That's why the graph only shows a few but larger allocations. Now, why does `jemalloc` not return memory? One possibility is that the heap is too fragmented to release a single page or `ruff` terminates before `jemalloc` runs its memory release routine. 

> This was a new notion to me. Surely a good piece of code, which behaves itself and puts away its toys when it's done, will keep its resource usage to a minimum? As it turns out, that isn't exactly the case. It really comes down to the behavior of the allocator, and most allocators won't necessarily return memory right away to the OS. Jemalloc has a few ways this can be tuned, but by default it uses a time-based strategy for returning memory back to the OS. Memory allocations are serviced first by memory already held by the process that is free for reuse, and then failing that, new memory is acquired from the OS. Given enough time, should a chunk of memory not have anything assigned to it, it will be returned (and if returning resources quickly is important to you, you can configure jemalloc to return the memory immediately).
[source](https://engineering.linkedin.com/blog/2021/taming-memory-fragmentation-in-venice-with-jemalloc)

I recommend investing time to find a long-running workload and tooling that measures the reserved OS memory if we're concerned about peak memory consumption. I myself aren't too concerned, considering that 150MB for lining the whole cpython code seems reasonable. 

> @MichaReiser - Should we consider putting this behind a flag while we continue to discuss, based on the above?

I personally would probably just revert the PR and reopen it. It's not a lot of code. 

For JS tooling it is very common to use custom allocators:
* [SWC](https://github.com/swc-project/swc/pull/4791)
* [lightning_css](https://github.com/parcel-bundler/lightningcss/commit/18adb3be5215c74b5dfa9a495c0a11e959e0d361)
* [Rome](https://github.com/rome/tools/pull/2900)
* [Oxc](https://github.com/Boshen/oxc/pull/12)

---

_Comment by @andersk on 2023-02-12 08:04_

The `time(1)` command on Linux gives you memory information, but you have to make sure to ask for the command and not the shell builtin:

```console
$ time ruff -n ../cpython
…
real    0m0.710s
user    0m5.472s
sys     0m0.146s

$ command time ruff -n ../cpython
…
5.39user 0.15system 0:00.79elapsed 695%CPU (0avgtext+0avgdata 112264maxresident)k
0inputs+0outputs (0major+28331minor)pagefaults 0swaps

$ \time ruff -n ../cpython
…
5.47user 0.15system 0:00.71elapsed 786%CPU (0avgtext+0avgdata 111580maxresident)k
0inputs+0outputs (0major+28573minor)pagefaults 0swaps
```

---

_Comment by @MichaReiser on 2023-02-15 11:36_

I spent some more time collecting metrics around memory usage but are having troubles interpreting the numbers.

## System Allocator (commit 6d1adc85fcbce5c9f87c04e59251a78c5eeda557)

### `/usr/bin/time`

`Maximum resident set size (kbytes): 178368 -> ~174MB`

<details><summary>Details</summary>

```
Command exited with non-zero status 1
	Command being timed: "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache"
	User time (seconds): 3.36
	System time (seconds): 0.23
	Percent of CPU this job got: 1179%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.30
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 178368
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 8
	Minor (reclaiming a frame) page faults: 44799
	Voluntary context switches: 13397
	Involuntary context switches: 3917
	Swaps: 0
	File system inputs: 0
	File system outputs: 0
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 1
```
</details>

### Valgrind


Total heap memory (mapped pages): 1,681,956,864 -> 1604MB [Full output](https://gist.github.com/MichaReiser/a6e51e0f1f17791bcf7984c364d24271)

## Jemalloc (ca49b00e55973a1daa577c4ccb34dd5c8a5311fb)

### `/usr/bin/time`

`Maximum resident set size (kbytes): 346904 -> 338MB`

<details>
	<summary>Details</summary>

```
Command exited with non-zero status 1
	Command being timed: "./target/release/ruff ./crates/ruff/resources/test/cpython/ --no-cache"
	User time (seconds): 3.40
	System time (seconds): 0.12
	Percent of CPU this job got: 1161%
	Elapsed (wall clock) time (h:mm:ss or m:ss): 0:00.30
	Average shared text size (kbytes): 0
	Average unshared data size (kbytes): 0
	Average stack size (kbytes): 0
	Average total size (kbytes): 0
	Maximum resident set size (kbytes): 346904
	Average resident set size (kbytes): 0
	Major (requiring I/O) page faults: 17
	Minor (reclaiming a frame) page faults: 22861
	Voluntary context switches: 835
	Involuntary context switches: 4940
	Swaps: 0
	File system inputs: 0
	File system outputs: 0
	Socket messages sent: 0
	Socket messages received: 0
	Signals delivered: 0
	Page size (bytes): 4096
	Exit status: 1
```

</details>


### Valgrind

Total mapped pages: 1917MB  [Full output](https://gist.github.com/MichaReiser/2339206d6e44d06c420a8a349b8dba9b)


## Summary

`/usr/bin/time` suggests that memory usage almost doubled, whereas valgridn only shows a 20% increase. One thing I haven't tried yet is to force trigger a jemalloc release of unused memory.



---
