---
number: 2801
title: Speed up CI with a two-tier system built on bors
type: issue
state: closed
author: epage
labels: []
assignees: []
created_at: 2021-10-01T19:55:18Z
updated_at: 2021-10-07T15:02:08Z
url: https://github.com/clap-rs/clap/issues/2801
synced_at: 2026-01-10T01:27:26Z
---

# Speed up CI with a two-tier system built on bors

---

_Issue opened by @epage on 2021-10-01 19:55_

### Discussed in https://github.com/clap-rs/clap/discussions/2723

<div type='discussions-op-text'>

<sup>Originally posted by **epage** August 18, 2021</sup>
clap tests a lot of system combinations

Pros
- Ensure we don't break clap for any of our users

Cons
- This makes it easy to max out our number of parallel runners, slowing down CI results for PRs
- While Github offers hosting for free, we should be mindful of the cost and be respectful with the resources we use

However, with bors, we can get the best of both worlds.  As a [merge queue](https://epage.github.io/dev/submit-queue/), bors does one extra CI check before commits make it to master.  We can rely on that to implement a two-tier CI system
- Checks that run on pull requests
- Checks that run on branches, like `master`, `staging`, and `trying`

If a contributor suspects their change would impact a case not covered for pull requests, they can comment `bors try` on their PR (I'm assuming `try` is available to contributors, its unclear who has access rights to each command).

So this means for each push to a PR, we can run a fraction of the jobs, completing much more quickly while still verifying nothing will make master red by testing during the merge proces.  If it would, it gets kicked out and the PR author gets that feedback.

Proposed implementation:
- Split `ci.yml` into `pr.yml` and `branch.yml`
- `pr.yml` would not include
  - `wasm`
  - `i686` procs (clap generally doesn't have proc-specific logic)
  - `macos-latest` (close enough to `ubuntu-latest` for what clap does)
  - rust `beta` and `nightly` (changes are unlikely to break with upcoming Rust versions but instead with MSRV)

Additional speed ups
- (Maybe) Feature combinations be run within the same job (reuse dependency builds / reduce startup times)
- Try https://github.com/Swatinem/rust-cache</div>

---

_Referenced in [clap-rs/clap#2802](../../clap-rs/clap/pulls/2802.md) on 2021-10-01 20:02_

---

_Comment by @epage on 2021-10-01 22:06_

@pksunkara you mentioned in https://github.com/clap-rs/clap/discussions/2723 that you were going to look into this.  I was already planning on looking into it (https://github.com/clap-rs/clap/pull/2802) but the PoC cost is low enough that I think its worth us both playing with our ideas and communicating through our respective PRs.  We can see how they turn out and pick the best from both.

---

_Comment by @pksunkara on 2021-10-01 22:24_

Your PR is pretty good, just a few things I wanted to be addressed.

---

_Closed by @epage on 2021-10-07 15:02_

---
