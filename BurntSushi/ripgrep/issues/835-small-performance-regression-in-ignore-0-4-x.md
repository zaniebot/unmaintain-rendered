```yaml
number: 835
title: Small performance regression in ignore-0.4.x
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - question
assignees: []
created_at: 2018-02-25T13:42:35Z
updated_at: 2018-04-24T15:19:05Z
url: https://github.com/BurntSushi/ripgrep/issues/835
synced_at: 2026-01-12T16:13:22Z
```

# Small performance regression in ignore-0.4.x

---

_@sharkdp_

#### What operating system are you using ripgrep on?

Arch Linux, x86_64

#### Describe your question, feature request, or bug.

I was running some benchmarks for `fd` and noticed that there was a small (~5%) performance regression when I updated from `ignore-0.2.2` to `ignore-0.4.x`. I investigated some more and ran the same benchmark (`fd -uu '[0-9]\.jpg$'` in my home folder) multiple times while changing the version of the ignore crate:

![image](https://user-images.githubusercontent.com/4209276/36641938-2a14fe2c-1a38-11e8-9870-f11e8f999707.png)

There are two things to notice:
1. The timing for `ignore-0.3.0` is not visisble because it's so fast (137 ms). This is due to a bug where not all results are shown (I didn't investigate further, but the git log seems to indicate that something was wrong with `ignore-0.3.0`).
2. There is a small (but statistically significant) performance regression that seems to have been introduced with commit 3cb4d13 (Custom ignore files). Interestingly, I'm running a search that doesn't even use ignore files (`-uu` is the same as in ripgrep).

#### What are the steps to reproduce the behavior?

I can reproduce this behavior with ripgrep itself:

```
▶ git checkout 51864c1; cargo install -f

...

▶ hyperfine --warmup 10 'rg -uu --files --iglob "*[0-9].jpg" ~'
Benchmark #1: rg -uu --files --iglob "*[0-9].jpg" ~

  Time (mean ± σ):     700.4 ms ±   5.3 ms    [User: 3.157 s, System: 2.310 s]
 
  Range (min … max):   691.4 ms … 709.0 ms

▶ git checkout 3cb4d13; cargo install -f

...

▶ hyperfine --warmup 10 'rg -uu --files --iglob "*[0-9].jpg" ~'
Benchmark #1: rg -uu --files --iglob "*[0-9].jpg" ~

  Time (mean ± σ):     725.4 ms ±   7.9 ms    [User: 3.318 s, System: 2.338 s]
 
  Range (min … max):   715.5 ms … 743.9 ms
```
(benchmarks performed with [hyperfine](https://github.com/sharkdp/hyperfine) on a warm disk cache)

Again, the difference is small (~4%) but significant.

---

_Comment by @sharkdp on 2018-02-25 15:32_

This seems to be caused by the additional `Gitignore` instance `custom_ignore_matcher` that has to be constructed in `Ignore::add_child_path`.

(apparently, the gitignore matchers are fully constructed even though no ignore-matching is performed)

---

_Label `bug` added by @BurntSushi on 2018-03-13 02:20_

---

_Label `question` added by @BurntSushi on 2018-03-13 02:20_

---

_Comment by @BurntSushi on 2018-04-23 23:17_

@sharkdp What corpus were your tests above on? I finally had a chance to look at this, and I can't really reproduce your results.

I'm testing on the chromium repo:

```
$ git remote -v
origin  git://github.com/nwjs/chromium.src (fetch)
origin  git://github.com/nwjs/chromium.src (push)
$ git rev-parse --short=10 HEAD
3682723171
```

Testing ripgrep on current master:

```
$ rg-baseline --version
ripgrep 0.8.1 (rev 507801c1f2)
+SIMD +AVX
$ hyperfine -s basic --warmup 10 'rg-baseline -uu --files'
Benchmark #1: rg-baseline -uu --files
  Time (mean ± σ):     230.6 ms ±  10.2 ms    [User: 1.004 s, System: 0.349 s]
  Range (min … max):   211.7 ms … 243.1 ms
```

Testing ripgrep with this PR, rebased on current master:

```
$ rg --version
ripgrep 0.8.1 (rev 2c2162f7bc)
+SIMD +AVX
$ hyperfine -s basic --warmup 10 'rg -uu --files'
Benchmark #1: rg -uu --files
  Time (mean ± σ):     230.2 ms ±   9.6 ms    [User: 927.0 ms, System: 383.5 ms]
  Range (min … max):   210.4 ms … 246.0 ms
```

---

_Comment by @sharkdp on 2018-04-24 06:38_

> @sharkdp What corpus were your tests above on? I finally had a chance to look at this, and I can't really reproduce your results.

I was running this on my home folder - not very helpful, I'm sorry. Two things seem to be different in your case:

- I believe the associated PR will show a more significant speedup in cases where there is a deep directory tree with lots of `.gitignore` files.

- Could you please try to add the `--iglob` option (with some file pattern that occurs only rarely) to make sure that the execution time is not dominated by writing to standard out?

I know it's not a very scientific method, but could you try to run the following commands in your home folder (or some other large directory)? I was just re-running the benchmarks with the rebased branch (which is now included in #836), and I still see a 10% speedup:

```
> rg-baseline --version
ripgrep 0.8.1 (rev bf51058eb2)
-SIMD -AVX

> rg-pr --version                                               
ripgrep 0.8.1 (rev 92dcb44d10)
-SIMD -AVX

> hyperfine --warmup 10 'rg-baseline -uu --files --iglob "README*"' \
                        'rg-pr       -uu --files --iglob "README*"'
Benchmark #1: rg-baseline -uu --files --iglob "README*"

  Time (mean ± σ):     760.8 ms ±   3.5 ms    [User: 3.395 s, System: 2.511 s]
 
  Range (min … max):   756.1 ms … 766.5 ms
 
Benchmark #2: rg-pr -uu --files --iglob "README*"

  Time (mean ± σ):     692.0 ms ±  15.5 ms    [User: 2.844 s, System: 2.493 s]
 
  Range (min … max):   679.6 ms … 731.3 ms
 
Summary

'rg-pr -uu --files --iglob "README*"' ran
    1.10x faster than 'rg-baseline -uu --files --iglob "README*"'
```

---

_Closed by @BurntSushi on 2018-04-24 15:19_

---
