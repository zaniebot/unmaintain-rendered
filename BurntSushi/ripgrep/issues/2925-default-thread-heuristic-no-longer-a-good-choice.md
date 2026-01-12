```yaml
number: 2925
title: Default thread heuristic no longer a good choice in Mac OSX Sequoia
type: issue
state: closed
author: jchien14
labels:
  - question
  - wontfix
assignees: []
created_at: 2024-11-07T18:11:22Z
updated_at: 2025-10-11T01:34:04Z
url: https://github.com/BurntSushi/ripgrep/issues/2925
synced_at: 2026-01-12T16:13:25Z
```

# Default thread heuristic no longer a good choice in Mac OSX Sequoia

---

_@jchien14_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

x86_64 ripgrep 14.1.1

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 15.1

### Describe your bug.

I upgraded from macOS 14.7 to 15.1 last night. Since then, I noticed ripgrep on my repo regressed from ~2s a search to ~10s

```
rg foo 1.28s user 108.10s system 1104% cpu 9.901 total
rg foo 1.40s user 85.94s system 882% cpu 9.898 total
```

From empirically testing, it appears that `--threads 3` provides the ideal behavior (my CPU has 14 cores)
```
rg --threads 4 foo  0.78s user 7.55s system 360% cpu 2.307 total
rg --threads 4 foo  0.76s user 7.64s system 392% cpu 2.143 total
rg --threads 4 foo  0.77s user 7.38s system 389% cpu 2.092 total
rg --threads 3 foo  0.76s user 5.48s system 286% cpu 2.179 total
rg --threads 3 foo  0.76s user 5.98s system 290% cpu 2.321 total
rg --threads 3 foo  0.75s user 5.78s system 289% cpu 2.255 total
rg --threads 3 foo  0.78s user 5.89s system 293% cpu 2.270 total
```

It looks like the current default is 12. Not sure what changed in 15.1 but two other people reproed this as well upon upgrading, one with a laptop with most system extensions uninstalled, so I don't think it's a bad interaction with a piece of third-party software on 15.1.

### What are the steps to reproduce the behavior?

Run any ripgrep search without specifying the number of threads on 15.1 

### What is the actual behavior?

Slow query, 10s in a relatively large repository I tested

### What is the expected behavior?

~1-2 seconds on OSX. For reference, ripgrepping this repo on Ubuntu is ~600ms

---

_Comment by @BurntSushi on 2024-11-07 18:17_

Can you provide a benchmark I can run please?

---

_Comment by @BurntSushi on 2024-11-07 18:18_

2s -> 10s feels like a mammoth change. Feels like something else is going on here personally.

I doubt the default heuristic is going to change any time soon, so you'll definitely want to make a ripgrep config file or something with your preferred default.

---

_Comment by @jchien14 on 2024-11-07 18:25_

Not sure what you mean by a benchmark -- unfortunately, I don't have a framework or anything, just a few reports amongst coworkers. 

Here's a process sample (16 threads)
[rg_2024-11-07_101925_9S3I.sample.txt](https://github.com/user-attachments/files/17667706/rg_2024-11-07_101925_9S3I.sample.txt)
I wonder if 15.1 has worse max parallelism at opening and closing file handles? 

Currently working around it with a config file to get the previous performance with the config file, but wanted to document this in case anyone else hits it

---

_Comment by @BurntSushi on 2024-11-07 18:28_

This is your benchmark:

> I noticed ripgrep on my repo regressed from ~2s a search to ~10s

But I can't run this. Can you share a benchmark that I can run please? For example, consider trying on an open source repository that we can both access. Like maybe the Linux kernel or Chromium.

The first step in diagnosing performance problems is having a shared reality. _Sometimes_ that's not possible. But if the problem here is truly as general as "more threads is dramatically slower," then it should ideally reproduce on another repo.

---

_Comment by @BurntSushi on 2024-11-07 18:28_

And yes, thank you for reporting this! I appreciate it.

---

_Comment by @jchien14 on 2024-11-08 04:00_

Following the instructions here for a fresh checkout of Chromium on Mac: https://chromium.googlesource.com/chromium/src/+/main/docs/mac_build_instructions.md#Install

Results are quite different:
```
rg --threads 12 testtesttest  4.67s user 263.67s system 893% cpu 30.029 total
rg --threads 11 testtesttest  5.53s user 194.23s system 709% cpu 28.156 total
rg --threads 10 testtesttest  5.32s user 105.07s system 524% cpu 21.043 total
rg --threads 9 testtesttest  5.02s user 70.38s system 404% cpu 18.660 total
rg --threads 8 testtesttest  4.83s user 57.36s system 327% cpu 18.968 total
rg --threads 7 testtesttest  4.71s user 51.42s system 272% cpu 20.628 total
rg --threads 6 testtesttest  4.60s user 47.50s system 230% cpu 22.561 total
rg --threads 5 testtesttest  4.41s user 43.28s system 184% cpu 25.837 total
rg --threads 4 testtesttest  4.58s user 42.68s system 146% cpu 32.208 total
rg --threads 3 testtesttest  5.20s user 46.57s system 117% cpu 43.994 total
```

Don't have much time until the weekend to do more samples than n=1, and also investigate the differences in file size/# files that might account for it (the chromium repo is much bigger than the one I was testing on). 

It is quite interesting that there's appears there's an optimum in terms of both wall-time that's different than the optimum for CPU usage (for the repo I was testing on they were the same). Performance starts going down significantly after the optimum. Unfortunately, I no longer can test on 14.7 to compare. 

---

_Comment by @BurntSushi on 2025-09-23 01:48_

I did this on my M2 mac mini on a checkout of the Linux kernel:

```
$ hyperfine -i 'rg ZQZQZQZQZQ' 'rg ZQZQZQZQZQ -j1' 'rg ZQZQZQZQZQ -j2' 'rg ZQZQZQZQZQ -j3' 'rg ZQZQZQZQZQ -j4' 'rg ZQZQZQZQZQ -j5' 'rg ZQZQZQZQZQ -j6' 'rg ZQZQZQZQZQ -j7' 'rg ZQZQZQZQZQ -j8' 'rg ZQZQZQZQZQ -j9' 'rg ZQZQZQZQZQ -j10' 'rg ZQZQZQZQZQ -j11' 'rg ZQZQZQZQZQ -j12' 'rg ZQZQZQZQZQ -j13'
```

And got this summary:

```
Summary
  rg ZQZQZQZQZQ -j4 ran
    1.03 ± 0.01 times faster than rg ZQZQZQZQZQ -j5
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ -j9
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ -j12
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ -j10
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ -j11
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ -j8
    1.04 ± 0.01 times faster than rg ZQZQZQZQZQ -j13
    1.05 ± 0.01 times faster than rg ZQZQZQZQZQ -j6
    1.05 ± 0.01 times faster than rg ZQZQZQZQZQ -j7
    1.30 ± 0.01 times faster than rg ZQZQZQZQZQ -j3
    1.81 ± 0.01 times faster than rg ZQZQZQZQZQ -j2
    3.30 ± 0.02 times faster than rg ZQZQZQZQZQ -j1
```

So `-j4` is technically the best here, but its difference with the others (including ripgrep's default heuristic) is effectively immaterial. This doesn't look like compelling enough evidence for me to change the heuristic.

With that said...

```
$ sw_vers
ProductName:            macOS
ProductVersion:         13.4
BuildVersion:           22F66
```

So... my results above are probably bunk. Sigh.

This one will have to stay open and wait for me to get a chance to update my mac mini. I usually only ssh into it on my LAN, and I'm not sure how to make it upgrade that way.

---

_Label `question` added by @BurntSushi on 2025-09-23 01:48_

---

_Comment by @BurntSushi on 2025-10-11 01:33_

OK, so I've finally upgraded my mac mini:

```
$ sw_vers
ProductName:            macOS
ProductVersion:         26.0.1
BuildVersion:           25A362
```

And I re-ran my `hyperfine` command from above:

```
Summary
  rg ZQZQZQZQZQ -j4 ran
    1.21 ± 0.01 times faster than rg ZQZQZQZQZQ -j3
    1.35 ± 0.01 times faster than rg ZQZQZQZQZQ -j5
    1.61 ± 0.05 times faster than rg ZQZQZQZQZQ -j2
    1.73 ± 0.04 times faster than rg ZQZQZQZQZQ -j6
    2.19 ± 0.02 times faster than rg ZQZQZQZQZQ -j7
    2.57 ± 0.09 times faster than rg ZQZQZQZQZQ -j8
    2.80 ± 0.64 times faster than rg ZQZQZQZQZQ
    2.86 ± 0.05 times faster than rg ZQZQZQZQZQ -j1
    2.93 ± 0.10 times faster than rg ZQZQZQZQZQ -j9
    3.18 ± 0.09 times faster than rg ZQZQZQZQZQ -j10
    3.22 ± 0.12 times faster than rg ZQZQZQZQZQ -j11
    3.27 ± 0.12 times faster than rg ZQZQZQZQZQ -j12
    3.33 ± 0.14 times faster than rg ZQZQZQZQZQ -j13
```

Ummm. Wow. Lol. What a difference.

The problem is that if I run it on something else (like my clone of `chromium`):

```
Summary
  rg ZQZQZQZQZQ -j8 ran
    1.69 ± 0.09 times faster than rg ZQZQZQZQZQ -j4
```

Then `-j8` is noticeably faster.

So I'm afraid there isn't really a consistent and obviously better heuristic that I can choose here. So I think I have to defer to the status quo? Not sure.

It's also possible Apple fucked something up and they'll fix it in a future release. I don't know.

---

_Closed by @BurntSushi on 2025-10-11 01:33_

---

_Label `wontfix` added by @BurntSushi on 2025-10-11 01:34_

---
