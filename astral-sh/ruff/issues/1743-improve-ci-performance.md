```yaml
number: 1743
title: Improve CI performance
type: issue
state: closed
author: charliermarsh
labels:
  - internal
  - performance
assignees: []
created_at: 2023-01-09T08:08:01Z
updated_at: 2023-01-13T19:21:12Z
url: https://github.com/astral-sh/ruff/issues/1743
synced_at: 2026-01-10T11:09:43Z
```

# Improve CI performance

---

_Issue opened by @charliermarsh on 2023-01-09 08:08_

A few ideas (all of which should be benchmarked prior to committing):

- [x] Skip `--release` in CI builds
- [x] Install `insta` via [`rust-cargo-install`](https://github.com/marketplace/actions/rust-cargo-install) (#1750)
- [ ] Try out [`cargo nextest`](https://nexte.st/)
- [x] Try out [`rust-cache`](https://github.com/Swatinem/rust-cache) action (#1750)


---

_Label `internal` added by @charliermarsh on 2023-01-09 08:08_

---

_Label `performance` added by @charliermarsh on 2023-01-09 08:08_

---

_Comment by @charliermarsh on 2023-01-09 08:11_

Couple data points -- given this, it's really only worth focusing on `test` and `build` for now:

<img width="306" alt="Screen Shot 2023-01-09 at 3 10 07 AM" src="https://user-images.githubusercontent.com/1309177/211264177-3cebb9bc-ee86-44be-bbe2-2413a0f8d086.png">
<img width="310" alt="Screen Shot 2023-01-09 at 3 10 23 AM" src="https://user-images.githubusercontent.com/1309177/211264180-ce8adcf3-0b23-48f1-ad8d-5a2db261f3cf.png">


---

_Comment by @charliermarsh on 2023-01-09 08:11_

(I feel like our build cache is really bad, though I don't understand why.)

---

_Comment by @charliermarsh on 2023-01-09 08:13_

Oh, maybe the problem is that we need to use separate caches for the different actions.

---

_Comment by @colin99d on 2023-01-09 14:18_

This isn't an issue yet since the CI is relatively quick and we dont have too many PRs at the same time, but stopping obsolete workflows could save us some time waiting to get a machine: 
https://yonatankra.com/7-github-actions-tricks-i-wish-i-knew-before-i-started/ (see number 6)


---

_Comment by @charliermarsh on 2023-01-09 16:20_

Good suggestion! Although I think we actually do that now thanks to @messense:

```yaml
concurrency:
  group: ${{ github.workflow }}-${{ github.ref_name }}-${{ github.event.pull_request.number || github.sha }}
  cancel-in-progress: true
```

---

_Comment by @colin99d on 2023-01-09 16:28_

Awesome!! I guess I need to improve my fuzzy finder (;.

---

_Comment by @ducaale on 2023-01-09 19:47_

>- Try out [`rust-cache`](https://github.com/Swatinem/rust-cache) action

This option seems to halve the CI duration time (~8m vs ~5m). The `Clippy` step is a bottleneck at the moment, but I believe splitting it into two jobs will speed things up further.



---

_Comment by @max-sixty on 2023-01-10 07:51_

FYI for binaries like `cargo-insta`, you probably want https://github.com/PRQL/prql/blob/b88083c63a83c235d454ab5eff190e1661fea048/.github/actions/test-rust/action.yaml#L41, which caches just the binary — so the runner doesn't need to pull a huge cache. The cache size is then just few MBs, and it loads in a few seconds.

---

_Comment by @Boshen on 2023-01-10 07:58_

FYI you can replace the `uses: actions-rs/toolchain@v1` step with a simple `rustup show`. 
I used this in the rome repo and it's working great.

https://github.com/rome/tools/blob/e0325d39ed9276eda6c08f973f4edd1af99e9b40/.github/workflows/main.yml#L29-L30

This'll save a few seconds from installing the plugin, and as a bonus, you no longer need to manually sync your Rust version.

(Hi from twitter 👋 )

---

_Comment by @charliermarsh on 2023-01-10 12:57_

@Boshen - Oh nice! Thank you! I tried it briefly (https://github.com/charliermarsh/ruff/pull/1769) but think it needs some fiddling since we use nightly for `rustfmt` and need to support the WASM target. Maybe I'll come back to it later :)

---

_Comment by @charliermarsh on 2023-01-10 12:57_

@Boshen - I looked through the originating PR on those changes -- should I also be doing a cache clean at the end of the job?

---

_Closed by @charliermarsh on 2023-01-10 21:51_

---

_Comment by @charliermarsh on 2023-01-10 21:51_

Closing for now as I think we've gotten through most of the items, and CI got way faster.

---

_Comment by @not-my-profile on 2023-01-12 14:40_

My latest PR does some minor edits to `.github/workflows/ci.yaml` and the CI is now taking >9min just to run `cargo install cargo-insta`. Would separating the `cargo insta test --all` into its own `.yaml` file help with that?

---

_Comment by @charliermarsh on 2023-01-12 15:27_

@not-my-profile - I wouldn't expect those changes to impact the install time. But they're definitely invalidating the cache given the `Cargo.toml` and `Cargo.lock` modifications. Is it possible that it's just a cold-cache artifact, and that discrepancy will go away once it's merged etc.?

---

_Comment by @not-my-profile on 2023-01-12 15:33_

Yes I am quite certain that it's a cold-cache effect. My point was that having the `cargo insta` in its own `.yml` file probably would result in fewer cache invalidations.

I was only slightly annoyed by the long build time since I was repeatedly editing the `ci.yaml` file but since we generally only seldomly do that and the cache works as expected feel free to ignore my comment.^^

---

_Comment by @messense on 2023-01-12 15:50_

I think it's because [crates.io-index](https://github.com/rust-lang/crates.io-index) [squashed into one commit](https://internals.rust-lang.org/t/cargos-crate-index-upcoming-squash-into-one-commit/8440) today, which makes the git clone delta calculation extremely slow when you have old caches.

---

_Comment by @max-sixty on 2023-01-13 19:21_

FYI if it's a cold cache effect, this approach does get its own cache line https://github.com/charliermarsh/ruff/issues/1743#issuecomment-1376857341...

---
