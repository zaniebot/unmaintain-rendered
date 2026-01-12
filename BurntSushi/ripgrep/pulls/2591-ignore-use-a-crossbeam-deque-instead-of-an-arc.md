```yaml
number: 2591
title: "ignore: Use a crossbeam deque instead of an Arc<Mutex<Vec<_>>>"
type: pull_request
state: closed
author: tavianator
labels:
  - rollup
assignees: []
base: master
head: crossbeam-deque
created_at: 2023-08-21T18:12:11Z
updated_at: 2023-09-20T21:35:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2591
synced_at: 2026-01-12T18:23:14Z
```

# ignore: Use a crossbeam deque instead of an Arc<Mutex<Vec<_>>>

---

_@tavianator_

This gives a significant scalability improvement for `fd`:

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./fd-before -uj1 '^$' ~/code/bfs/bench/corpus/chromium` | 577.6 ± 9.1 | 558.1 | 586.9 | 5.46 ± 1.29 |
| `./fd-before -uj6 '^$' ~/code/bfs/bench/corpus/chromium` | 553.7 ± 17.4 | 522.1 | 574.7 | 5.24 ± 1.25 |
| `./fd-before -uj12 '^$' ~/code/bfs/bench/corpus/chromium` | 581.7 ± 14.2 | 556.0 | 600.0 | 5.50 ± 1.30 |
| `./fd-before -uj24 '^$' ~/code/bfs/bench/corpus/chromium` | 587.7 ± 25.6 | 547.9 | 614.3 | 5.56 ± 1.33 |
| `./fd-after -uj1 '^$' ~/code/bfs/bench/corpus/chromium` | 586.8 ± 11.3 | 569.5 | 597.5 | 5.55 ± 1.31 |
| `./fd-after -uj6 '^$' ~/code/bfs/bench/corpus/chromium` | 171.9 ± 7.2 | 157.4 | 187.8 | 1.63 ± 0.39 |
| `./fd-after -uj12 '^$' ~/code/bfs/bench/corpus/chromium` | 128.7 ± 10.7 | 111.0 | 143.4 | 1.22 ± 0.30 |
| `./fd-after -uj24 '^$' ~/code/bfs/bench/corpus/chromium` | 105.7 ± 24.9 | 84.5 | 159.7 | 1.00 |

There is a minor (~1.5%) single-threaded performance regression in this benchmark.  If you want, I can try to mitigate that, but it seems okay to me.

---

_Comment by @tavianator on 2023-08-22 19:58_

Ripgrep-specific benchmarks:

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./rg-before -uuuj1 --files ~/code/bfs/bench/corpus/chromium` | 579.4 ± 12.8 | 561.5 | 599.9 | 3.54 ± 0.14 |
| `./rg-before -uuuj6 --files ~/code/bfs/bench/corpus/chromium` | 507.0 ± 13.0 | 475.2 | 516.5 | 3.10 ± 0.13 |
| `./rg-before -uuuj12 --files ~/code/bfs/bench/corpus/chromium` | 565.6 ± 4.4 | 557.1 | 571.1 | 3.46 ± 0.12 |
| `./rg-before -uuuj24 --files ~/code/bfs/bench/corpus/chromium` | 548.5 ± 3.2 | 543.1 | 553.3 | 3.36 ± 0.11 |
| `./rg-after -uuuj1 --files ~/code/bfs/bench/corpus/chromium` | 575.2 ± 16.3 | 557.9 | 605.6 | 3.52 ± 0.15 |
| `./rg-after -uuuj6 --files ~/code/bfs/bench/corpus/chromium` | 229.7 ± 9.2 | 216.1 | 249.1 | 1.41 ± 0.07 |
| `./rg-after -uuuj12 --files ~/code/bfs/bench/corpus/chromium` | 192.7 ± 7.0 | 179.8 | 205.9 | 1.18 ± 0.06 |
| `./rg-after -uuuj24 --files ~/code/bfs/bench/corpus/chromium` | 163.5 ± 5.4 | 155.8 | 178.2 | 1.00 |


---

_Comment by @BurntSushi on 2023-08-29 02:50_

Thank you for this! I just wanted to acknowledge it and thank you for the work. I'll plan to dig into this before the ripgrep 14 release. (But I'm taking a detour to do some improvements to the `aho-corasick` crate first.)

---

_Comment by @BurntSushi on 2023-09-18 20:24_

OK, so I've read through this patch and it looks good to me! It's very well written, thank you. I've also run a number of ad hoc benchmarks (shown below) and I'd describe the overall result as follows:

1. Never meaningfully slower. Sometimes meaningfully faster.
2. Sometimes uses more memory, but not an obscene amount.
3. It passes my "insane" test of searching an extremely wide directory of all crates from crates.io (from some time ago). Basically, it completes in roughly the same time as `master` and uses roughly the same amount of memory.

Some ad hoc benchmarks are below. Unless otherwise noted, file caches are warm.

On a checkout of the Linux kernel:

```
$ git remote -v
origin  git@github.com:torvalds/linux (fetch)
origin  git@github.com:torvalds/linux (push)

$ git rev-parse HEAD
f1fcbaa18b28dec10281551dfe6ed3a3ed80e3d6

$ time rg-before-deque -c BURNTSUSHI

real    0.084
user    0.309
sys     0.608
maxmem  14 MB
faults  0

$ time rg-after-deque -c BURNTSUSHI

real    0.079
user    0.258
sys     0.583
maxmem  13 MB
faults  0

$ hyperfine -i -w3 "rg-before-deque -c BURNTSUSHI" "rg-after-deque -c BURNTSUSHI"
Benchmark 1: rg-before-deque -c BURNTSUSHI
  Time (mean ± σ):      85.3 ms ±   1.0 ms    [User: 304.5 ms, System: 603.8 ms]
  Range (min … max):    83.3 ms …  87.3 ms    34 runs

Benchmark 2: rg-after-deque -c BURNTSUSHI
  Time (mean ± σ):      79.4 ms ±   1.2 ms    [User: 251.1 ms, System: 566.4 ms]
  Range (min … max):    76.7 ms …  81.7 ms    36 runs

Summary
  'rg-after-deque -c BURNTSUSHI' ran
    1.07 ± 0.02 times faster than 'rg-before-deque -c BURNTSUSHI'
```

On a checkout of chromium (nice result):

```
$ git remote -v
origin  git@github.com:nwjs/chromium.src (fetch)
origin  git@github.com:nwjs/chromium.src (push)

$ git rev-parse HEAD
e25f26f9aef0fd5958b980c3da4fdc2703ea4ccc

$ time rg-before-deque -c BURNTSUSHI

real    0.327
user    1.384
sys     2.279
maxmem  101 MB
faults  0

$ time rg-after-deque -c BURNTSUSHI

real    0.285
user    1.091
sys     1.977
maxmem  75 MB
faults  0

$ hyperfine -i -w3 "rg-before-deque -c BURNTSUSHI" "rg-after-deque -c BURNTSUSHI"
Benchmark 1: rg-before-deque -c BURNTSUSHI
  Time (mean ± σ):     331.8 ms ±   2.7 ms    [User: 1528.5 ms, System: 2190.0 ms]
  Range (min … max):   327.5 ms … 335.2 ms    10 runs

Benchmark 2: rg-after-deque -c BURNTSUSHI
  Time (mean ± σ):     286.5 ms ±   4.3 ms    [User: 1175.8 ms, System: 1973.8 ms]
  Range (min … max):   281.1 ms … 293.5 ms    10 runs

Summary
  'rg-after-deque -c BURNTSUSHI' ran
    1.16 ± 0.02 times faster than 'rg-before-deque -c BURNTSUSHI'
```

On a checkout of some Harry Potter fan-fiction (an amazing result):

```
$ git remote -v
origin  git@github.com:NightMachinary/r_HPfanfiction (fetch)
origin  git@github.com:NightMachinary/r_HPfanfiction (push)

$ git rev-parse HEAD
b373f0c3c03cdeed261646038170a2286fdcb4b8

$ time rg-before-deque -c BURNTSUSHI

real    0.803
user    4.006
sys     4.561
maxmem  157 MB
faults  0

$ time rg-after-deque -c BURNTSUSHI

real    0.524
user    2.242
sys     3.686
maxmem  34 MB
faults  0

$ hyperfine -i -w3 "rg-before-deque -c BURNTSUSHI" "rg-after-deque -c BURNTSUSHI"
Benchmark 1: rg-before-deque -c BURNTSUSHI
  Time (mean ± σ):     797.3 ms ±  18.2 ms    [User: 3810.4 ms, System: 4735.8 ms]
  Range (min … max):   747.8 ms … 809.7 ms    10 runs

Benchmark 2: rg-after-deque -c BURNTSUSHI
  Time (mean ± σ):     525.2 ms ±   2.7 ms    [User: 2267.6 ms, System: 3692.9 ms]
  Range (min … max):   521.6 ms … 529.5 ms    10 runs

Summary
  'rg-after-deque -c BURNTSUSHI' ran
    1.52 ± 0.04 times faster than 'rg-before-deque -c BURNTSUSHI'
```

And now my "torture" test. In this case, I clear the file cache before each search because the entire corpus doesn't fit into memory on the machine I have it on. So this seemed like the most fair test.

```
$ sudo clear-file-cache

$ time rg-before-deque BURNTSUSHI

real    1:40.15
user    31.787
sys     1:05.12
maxmem  476 MB
faults  67

$ sudo clear-file-cache

$ time rg-after-deque BURNTSUSHI

real    1:51.20
user    34.253
sys     1:11.48
maxmem  364 MB
faults  69
```

While there is a big difference here, I ran this test multiple times and there is a ton of noise. However, each test run is all within roughly the same ballpark. If I run with ripgrep 11, it didn't complete after 15 minutes and >1.5GB of memory used, so I killed it. Definitely a different ballpark. Bottom line is that as far as I can tell, this PR does not appear to regress on my torture test.

I'll merge this PR in my rollup branch with a few minor style changes. Thank you! :-)

I think the only downside of this change is that it brings in more dependencies. But they look to be good quality libraries and I feel like they are carrying some significant weight with the work-stealing queue.

---

_Label `rollup` added by @BurntSushi on 2023-09-18 20:29_

---

_Closed by @BurntSushi on 2023-09-20 15:52_

---

_Branch deleted on 2023-09-20 21:35_

---
