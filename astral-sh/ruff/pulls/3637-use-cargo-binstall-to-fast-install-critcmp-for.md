```yaml
number: 3637
title: Use cargo-binstall to fast install critcmp for benchmark-compare
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - internal
assignees: []
merged: true
base: main
head: add-cache-to-benchmark-compare
created_at: 2023-03-20T23:17:55Z
updated_at: 2023-03-21T14:06:17Z
url: https://github.com/astral-sh/ruff/pull/3637
synced_at: 2026-01-12T04:39:45Z
```

# Use cargo-binstall to fast install critcmp for benchmark-compare

---

_Pull request opened by @JonathanPlasse on 2023-03-20 23:17_

_No description provided._

---

_Comment by @github-actions[bot] on 2023-03-20 23:30_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     14.0±0.18ms     2.9 MB/sec    1.00     13.9±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      3.8±0.08ms     4.4 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.00    429.5±1.48µs     6.9 MB/sec    1.00   431.2±12.27µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.3±0.05ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.8±0.16ms     5.2 MB/sec    1.00      7.7±0.09ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02   1697.7±2.00µs     9.8 MB/sec    1.00  1671.0±18.57µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    175.7±0.77µs    16.8 MB/sec    1.00    174.6±1.22µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.0 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     19.7±0.75ms     2.1 MB/sec    1.02     20.1±0.74ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.3±0.21ms     3.1 MB/sec    1.00      5.2±0.18ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   659.1±34.14µs     4.5 MB/sec    1.00   656.4±28.50µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.4±0.34ms     3.0 MB/sec    1.04      8.8±0.34ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     11.1±0.47ms     3.7 MB/sec    1.05     11.6±0.44ms     3.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.16ms     7.1 MB/sec    1.03      2.4±0.09ms     6.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   269.7±14.84µs    10.9 MB/sec    1.03   277.9±17.42µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.8±0.16ms     5.3 MB/sec    1.07      5.1±0.21ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-20 23:35_

---

_Label `internal` added by @charliermarsh on 2023-03-20 23:37_

---

_Review comment by @MichaReiser on `.github/workflows/benchmark.yaml`:29 on 2023-03-21 07:38_

whoops, thx

---

_Review comment by @MichaReiser on `.github/workflows/benchmark.yaml`:81 on 2023-03-21 08:36_

I considered adding rust-cache to the critcmp step but decided against it because GitHub limits the cache per repository to 10GB. That means GitHub starts evicting cached artifacts once we exceed the 10GB limit. You can see all our cache artifacts [here](https://github.com/charliermarsh/ruff/actions/caches). 

Now, the reason I excluded `critcmp` was to avoid adding yet another cache (where each cache is around 600MB) that results in the eviction of other cached artifacts (e.g., for the clippy process) because I thought having warm caches for clippy is more important than for the benchmark results. 

I don't know if that's the right call, nor have I verified my assumption that caching the c`ritcmp` build causes any build time regression for other jobs. So happy to give it a try. But do we need to built `critcmp` in the first place...

I researched more to find ways to avoid building `critcmp` altogether. I was excited when I read that [criterion 0.4](https://www.tweag.io/blog/2022-03-03-criterion-rs/) has built-in tabulating. No need to use `critcmp`, we can generate the comparison directly in the benchmark step. Unfortunately, they [removed tabulating](https://github.com/bheisler/criterion.rs/pull/610) before the release in favor of adding it to [`cargo-criterion`](https://github.com/bheisler/cargo-criterion). I tried `cargo criterion` locally, but the feature doesn't seem to exist, and even if it does, we would only be switching one dependency for another. 

So what if we could avoid running `cargo install` altogether? Can we download the `critcmp` binary or cache it somehow? That's when I discovered [`cargo-binstall`](https://github.com/cargo-bins/cargo-binstall). It installs binaries rather than building the crates from source... yay! Uhm, okay, but how do we install `cargo-binstall`?

`cargo-binstall` recommends using [install-development-tools](https://github.com/marketplace/actions/install-development-tools): `uses: taiki-e/install-action@cargo-binstall`. All that's left to do is to run `cargo binstall critcmp` (maybe figure out the right options)

~~Luckily `cargo-binstall` publishes prebuilt binaries on GitHub and there are even a github actions to download binaries from the release page ([release-downloader](https://github.com/marketplace/actions/release-downloader) or [`dtonlay/install`](https://github.com/dtolnay/install)). Alternatively, we could just use `curl` or `wget` to get the binary. Do you want to give this at try?~~



---

_@MichaReiser reviewed on 2023-03-21 08:36_

---

_@JonathanPlasse reviewed on 2023-03-21 08:42_

---

_Review comment by @JonathanPlasse on `.github/workflows/benchmark.yaml`:81 on 2023-03-21 08:42_

Yes!

---

_Review comment by @JonathanPlasse on `.github/workflows/benchmark.yaml`:81 on 2023-03-21 12:18_

Currently, `critcmp` does not seem to have prebuilt binaries on GitHub.

---

_@JonathanPlasse reviewed on 2023-03-21 12:18_

---

_@MichaReiser reviewed on 2023-03-21 12:21_

---

_Review comment by @MichaReiser on `.github/workflows/benchmark.yaml`:81 on 2023-03-21 12:21_

This shouldn't be necessary as far as I understand. You can use `cargo binstall` to install `critcmp`. It uses the [`quickinstall`](https://github.com/cargo-bins/cargo-quickinstall) registry under the hood that provides pre-built binaries (at least it works on my linux machine)

```sh
❯ cargo binstall critcmp -y
 INFO resolve: Resolving package: 'critcmp'
 WARN The package critcmp v0.1.7 will be downloaded from third-party source QuickInstall
 INFO This will install the following binaries:
 INFO   - critcmp (critcmp -> /home/micha/.cargo/bin/critcmp-v0.1.7)
 INFO And create (or update) the following symlinks:
 INFO   - critcmp (/home/micha/.cargo/bin/critcmp -> critcmp-v0.1.7)
 INFO Installing binaries...
 INFO Done in 2.070975169s
```

---

_Comment by @JonathanPlasse on 2023-03-21 13:42_

3s instead of 2m3s to install `critcmp`!

---

_@MichaReiser approved on 2023-03-21 13:44_

> 3s instead of 2m3s to install `critcmp`!

Whooohoo nice :partying_face: ! Thanks for working on this.

@Boshen some more benchmark CI performance improvements ;)

---

_Merged by @MichaReiser on 2023-03-21 13:45_

---

_Closed by @MichaReiser on 2023-03-21 13:45_

---

_Comment by @MichaReiser on 2023-03-21 13:45_

Whoops, I should have changed the title before merging... Too late :no_good: 

---

_Branch deleted on 2023-03-21 13:59_

---

_Comment by @JonathanPlasse on 2023-03-21 14:01_

Should it be revert and merge again with the good title?

---

_Renamed from "Add Swatinem/rust-cache to benchmark-compare job" to "Use cargo-binstall to fast install critcmp for benchmark-compare" by @JonathanPlasse on 2023-03-21 14:03_

---

_Comment by @MichaReiser on 2023-03-21 14:06_

> Should it be revert and merge again with the good title?

I think we're good. There are more fun things to do, and it isn't that bad in my view. The title may be misleading because the implementation is different but it correctly points out which part of the system was changed, and the PR contains all details. 

---
