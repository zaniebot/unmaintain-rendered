```yaml
number: 3600
title: "feat: move to cargo-xwin for clippy"
type: pull_request
state: merged
author: samypr100
labels:
  - internal
assignees: []
merged: true
base: main
head: cargo-xwin-clippy
created_at: 2024-05-15T03:19:19Z
updated_at: 2024-05-17T02:32:03Z
url: https://github.com/astral-sh/uv/pull/3600
synced_at: 2026-01-12T16:05:44Z
```

# feat: move to cargo-xwin for clippy

---

_@samypr100_

## Summary

Move to cargo-xwin for clippy

Closes #3507

---

_Comment by @messense on 2024-05-15 03:45_

FYI, consider install `cargo-xwin` from pypi or github releases to avoid compiling it. And you should try to cache Windows SDK download by explicitly setting `XWIN_CACHE_DIR` and cache it via the cache action.

---

_Comment by @charliermarsh on 2024-05-15 03:47_

Would cargo-binstall work or no?

---

_Comment by @messense on 2024-05-15 03:48_

probably works, I never tried.

---

_Comment by @messense on 2024-05-15 03:49_

BTW, I think you forgot to use a Linux runner? otherwise using `cargo-xwin` on Windows isn't beneficial?

---

_Comment by @samypr100 on 2024-05-15 04:07_

Indeed, was trying to gather some baseline data on windows first 😅; numbers don't look too promising thus far.

---

_Comment by @codspeed-hq[bot] on 2024-05-15 04:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100:cargo-xwin-clippy)

### Merging #3600 will **not alter performance**

<sub>Comparing <code>samypr100:cargo-xwin-clippy</code> (aa61db7) with <code>main</code> (ed91b1d)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Comment by @konstin on 2024-05-15 12:59_

The clippy step is 12:30min while clippy itself takes 30s, do you know where the other 12min are? The xwin download and unpack are <2min on my machine.

---

_Marked ready for review by @samypr100 on 2024-05-15 13:24_

---

_Comment by @messense on 2024-05-15 13:30_

> do you know where the other 12min are?

Probably because the network connection from GH action to Windows SDK download isn't very good,  cached version is much better. 

---

_Label `internal` added by @konstin on 2024-05-15 13:32_

---

_@konstin approved on 2024-05-15 13:33_

Clippy itself takes 30s, the whole job 1:30min, great work!

---

_Comment by @samypr100 on 2024-05-15 13:40_

😅 need to add up the totals numbers and make a table, will do later today to get better comparison.

At a glance on an unchached run it's marginally worse by a quite the factor (2m-3m before vs 15 min now) ~ .
On cached runs job totals are actually about the same on both windows and linux.

TL;DR Seems when cache is busted the impact is much worse than it is now and we're not gaining on job total speeds.

(will also explore some other fine tuning later like separating the windows sdk cache out)

---

_Comment by @konstin on 2024-05-15 13:42_

Would moving the xwin cache into a separate cache step only keyed on `rust-toolchain.toml` help? This way the cache would only ever be invalidated when we upgrade rust

---

_Comment by @samypr100 on 2024-05-15 13:43_

> Would moving the xwin cache into a separate cache step only keyed on rust-toolchain.toml help? This way the cache would only ever be invalidated when we upgrade rust

You read my mind 😆 

---

_Comment by @zanieb on 2024-05-15 15:22_

We do frequently fill our cache, we'll want to be careful that it's not evicted often due to space limitations.

---

_Comment by @samypr100 on 2024-05-16 02:16_

Here's some of the initial numbers to compare I've grabbing from all the past runs.

|  Current Main     | Before Cache (Step, Job)  | After Cache (Step, Job) |
|-------------------|---------------------------|-------------------------|
| Clippy on Windows | 2m~, 3m~                  | 30s~, 1.25m~            |

|  New Clippy Runs                         | After (Step, Job)
|-----------------------------------|------------------|
| xwin on windows                   | 2m 45s~, 5m~     |
| xwin on linux (no cache)          | 14m~, 15m~       |
| xwin on linux (rust cache)        | 12m~, 13m~       |
| xwin on linux (xwin cache)        | 2m~, 3m~         |
| xwin on linux (rust + xwin cache) | 30s~, 1.25m~     |

It seems clippy xwin runs about the same speed as windows currently (post the ReFS move). The job totals are aligned on best scenarios.

The xwin cache made the biggest difference for cargo xwin, making it run consistently with current windows runner, but it introduces a bigger risk when that cache gets busted.

The 15 minute hit will be most noticeable on downstream forks (where there is no cache often) or when cache is lost in main after rust toolchain upgrades or cache eviction.

---

_Comment by @konstin on 2024-05-16 07:43_

Personally, i think this still is a noticeable improvement, it means we skip the virtual disk setup and have a windows runner less; the 15min cache priming about every 6 weeks are acceptable.

---

_@messense reviewed on 2024-05-16 08:10_

---

_Review comment by @messense on `.github/workflows/ci.yml`:86 on 2024-05-16 08:10_

I don't think xwin cache needs to be keyed by Rust version, it's just an unpacked Windows SDK, no Rust involved.

---

_Renamed from "feat: explore cargo-xwin for clippy" to "feat: move to cargo-xwin for clippy" by @samypr100 on 2024-05-17 01:44_

---

_Comment by @samypr100 on 2024-05-17 01:45_

> Personally, i think this still is a noticeable improvement, it means we skip the virtual disk setup and have a windows runner less; the 15min cache priming about every 6 weeks are acceptable.

Sounds good, PR is good to go now. Apologies, I thought I had this PR was in draft all this time 😅 

---

_@samypr100 reviewed on 2024-05-17 01:46_

---

_Review comment by @samypr100 on `.github/workflows/ci.yml`:86 on 2024-05-17 01:46_

Thanks, moved cache key to be just `cargo-xwin`

---

_Merged by @zanieb on 2024-05-17 02:15_

---

_Closed by @zanieb on 2024-05-17 02:15_

---

_Branch deleted on 2024-05-17 02:32_

---
