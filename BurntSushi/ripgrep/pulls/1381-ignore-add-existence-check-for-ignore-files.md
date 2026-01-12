```yaml
number: 1381
title: "ignore: add existence check for ignore files"
type: pull_request
state: closed
author: sharkdp
labels:
  - rollup
assignees: []
base: master
head: add-existence-check
created_at: 2019-09-17T18:52:05Z
updated_at: 2020-04-02T15:00:13Z
url: https://github.com/BurntSushi/ripgrep/pull/1381
synced_at: 2026-01-12T18:23:13Z
```

# ignore: add existence check for ignore files

---

_@sharkdp_

This PR suggests to add a simple `.exists()` check for `.gitignore`, `.ignore`, and other similar files before actually calling `File::open(…)` in `GitIgnoreBuilder::add`.

The reason is that a simple existence check via `stat` can be faster than actually trying to `open` the file (see https://stackoverflow.com/a/12774387/704831). As we typically expect the number of directories *without* ignore files to be much larger than the number of directories *with* ignore files, this leads to an overall speedup.

The performance gain is not huge for `rg`, but can be quite significant if more `.gitignore`-like files are added via `add_custom_ignore_filename`. The speedup is *larger* for folders with *low* files-per-directory ratios.

Benchmark results
-----------------

`rg --files` in my home folder (200k results, 6.5 files per directory):

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./rg-master --files` | 396.4 ± 3.2 | 390.9 | 400.0 | 1.05 |
| `./rg-feature --files` | 376.0 ± 3.6 | 369.3 | 383.5 | 1.00 |

`rg --files --hidden` in my home folder (800k results, 5.4 files per directory)

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| `./rg-master --files --hidden` | 1.575 ± 0.012 | 1.560 | 1.597 | 1.06 |
| `./rg-feature --files --hidden` | 1.479 ± 0.011 | 1.464 | 1.496 | 1.00 |

`rg --files` in the chromium-79.0.3915.2 source tree (300k results, 12.7 files per directory)

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `~/rg-master --files` | 445.2 ± 5.3 | 435.6 | 453.0 | 1.04 |
| `~/rg-feature --files` | 428.9 ± 7.0 | 418.2 | 440.0 | 1.00 |

`rg --files` in the linux-5.3 source tree (65k results, 15.1 files per directory)

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./rg-master --files` | 94.5 ± 1.9 | 89.8 | 98.5 | 1.02 |
| `./rg-feature --files` | 92.6 ± 2.7 | 88.4 | 98.7 | 1.00 |

Running a `fd` search in my home directory (`fd` uses more `.gitignore`-like files):

| Command | Mean [s] | Min [s] | Max [s] | Relative |
|:---|---:|---:|---:|---:|
| `./fd-master -H foobar` | 1.136 ± 0.012 | 1.122 | 1.158 | 1.10 |
| `./fd-feature -H foobar` | 1.030 ± 0.006 | 1.022 | 1.041 | 1.00 |

---

_Comment by @sharkdp on 2019-09-23 17:58_

(I am assuming that the CI failure is not related to my PR. If this is not the case, please let me know)

---

_Closed by @BurntSushi on 2020-02-15 13:24_

---

_Reopened by @BurntSushi on 2020-02-15 13:24_

---

_@BurntSushi approved on 2020-02-15 13:26_

I buy it, thanks!

---

_Label `rollup` added by @BurntSushi on 2020-02-15 15:25_

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Branch deleted on 2020-04-02 15:00_

---
