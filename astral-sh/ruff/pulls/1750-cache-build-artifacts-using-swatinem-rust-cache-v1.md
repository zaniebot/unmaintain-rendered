```yaml
number: 1750
title: "Cache build artifacts using Swatinem/rust-cache@v1"
type: pull_request
state: merged
author: ducaale
labels: []
assignees: []
merged: true
base: main
head: faster-ci
created_at: 2023-01-09T19:50:20Z
updated_at: 2023-01-10T19:33:04Z
url: https://github.com/astral-sh/ruff/pull/1750
synced_at: 2026-01-12T15:55:07Z
```

# Cache build artifacts using Swatinem/rust-cache@v1

---

_@ducaale_

This GitHub Action caches build artifacts in addition to dependencies which halves the CI duration time.


| job | [Before](https://github.com/charliermarsh/ruff/actions/runs/3875785133) | [After](https://github.com/charliermarsh/ruff/actions/runs/3877340917) |
|-----|---------|------|
cargo build | 7m | 4m
cargo fmt |  41s | 30s
cargo clippy | 4m 38s | 1m 30s
cargo test | 10m 54s | 3m 27s
maturin build | 4m 36s | 1m 15s
Spell Check with typos | 29s | 25s

Resolves #1743

---

_@charliermarsh reviewed on 2023-01-09 19:51_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:85 on 2023-01-09 19:51_

Does this need `- uses: Swatinem/rust-cache@v1` too?

---

_Marked ready for review by @ducaale on 2023-01-09 19:56_

---

_Comment by @charliermarsh on 2023-01-09 20:18_

@ducaale - Do you mind quickly summarizing the before/after performance? Thank you for this!

---

_Merged by @charliermarsh on 2023-01-09 20:35_

---

_Closed by @charliermarsh on 2023-01-09 20:35_

---

_Comment by @charliermarsh on 2023-01-09 20:36_

Awesome, thank you! (I created and closed out a separate issue, and marked the item as complete in #1743, since I didn't want to close that quite yet given that it had a few more sub-items.)

---

_Branch deleted on 2023-01-09 20:44_

---

_Comment by @ducaale on 2023-01-09 20:51_

I have added a comparison table to the PR description (It might not be a fair comparison since I used a random action run in the past)

>(I created and closed out a separate issue, and marked the item as complete in https://github.com/charliermarsh/ruff/issues/1743, since I didn't want to close that quite yet given that it had a few more sub-items.)

Sounds good. Note that `insta` is now being cached by `Swatinem/rust-cache@v1`, so the only things left to do are using `cargo-nextest` and removing the `--release` flag.

Lastly, it might be worth taking a look at https://www.reillywood.com/blog/rust-faster-ci/

---

_Comment by @charliermarsh on 2023-01-09 22:27_

Thank you, that's a huge difference! So cool.

---

_Comment by @charliermarsh on 2023-01-09 22:28_

I'm just curious -- did you see my tweet about https://github.com/charliermarsh/ruff/issues/1743? Or stumble upon this organically?

---

_Comment by @charliermarsh on 2023-01-09 22:29_

(If you have a Twitter and want to be tagged, I'll tweet about this and tag you :))

---

_Comment by @ducaale on 2023-01-10 19:33_

>I'm just curious -- did you see my tweet about https://github.com/charliermarsh/ruff/issues/1743? Or stumble upon this organically?

Yes, this was prompted by the tweet about ruff's CI performance. We did a similar thing for xh's CI in https://github.com/ducaale/xh/pull/251

>(If you have a Twitter and want to be tagged, I'll tweet about this and tag you :))

Thanks, my twitter handle is @ducaale555 but I don't think this PR is worth tweeting about. All the credit goes to https://www.reillywood.com/blog/rust-faster-ci/



---
