```yaml
number: 3479
title: "ci: Use rust toolchain actions"
type: pull_request
state: closed
author: MichaReiser
labels: []
assignees: []
base: main
head: ci/use-rust-toolchain-action
created_at: 2023-03-13T10:55:40Z
updated_at: 2023-07-24T15:02:04Z
url: https://github.com/astral-sh/ruff/pull/3479
synced_at: 2026-01-12T15:55:12Z
```

# ci: Use rust toolchain actions

---

_@MichaReiser_

Use the `actions-rs/toolchain` action to install Rust, `actions-rs/cargo` to run cargo commands, and `action-rs/clippy-check` to run clippy.

* Using `actinos-rs/toolchain` ensures that our workflows work with [act](https://github.com/nektos/act), allowing to test locally
* Using `actions-rs/cargo`: It gives nice warnings and errors in the GitHub UI
* Using `actions/clippy-check`: Posts lint annotations

## Test Plan

Sample output after I added clippy errors

https://github.com/charliermarsh/ruff/actions/runs/4404224020/jobs/7713601234

---

_Comment by @github-actions[bot] on 2023-03-13 11:06_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Comment by @charliermarsh on 2023-03-13 12:57_

Seems reasonable to me. We intentionally moved away from `actions-rs/toolchain` at Boshen's recommendation based on profiling work from Rome. I'm curious if your benchmarking shows that it's much slower (or not).

---

_Comment by @MichaReiser on 2023-03-13 14:10_

> Seems reasonable to me. We intentionally moved away from `actions-rs/toolchain` at Boshen's recommendation based on profiling work from Rome. I'm curious if your benchmarking shows that it's much slower (or not).

Oh, interesting. This does make sense. Using `rustup` directly is easier (write stupid code) because it is installed on all GitHub runners. The question is if we're okay with the added complexity to support local-testing with act. 

I did a quick check on performance and it seems to be about the same:

* [Job using `rustup`](https://github.com/charliermarsh/ruff/actions/runs/4405150357/jobs/7715673284): ~14s
* [Job using `actions-rs/toolchain`](https://github.com/charliermarsh/ruff/actions/runs/4404934798/jobs/7715184666): ~14s

@Boshen's [commit](https://github.com/rome/tools/commit/da5b5c07f961e4961838273b76a2156dd5d95c26) that removes the action from Rome's CI

---

_Comment by @Boshen on 2023-03-13 14:29_

The benefit is that you don't have to sync your CI Rust versions with rust-toolchain.toml.

---

_Comment by @MichaReiser on 2023-03-13 15:36_

:wave: @Boshen Thanks for chiming in. 

> The benefit is that you don't have to sync your CI Rust versions with rust-toolchain.toml.

That's very helpful to know. Because this seems to be no longer an issue. The action now supports the [toolchain file](https://github.com/actions-rs/toolchain#the-toolchain-file). 

> This Action supports [toolchain files](https://rust-lang.github.io/rustup/overrides.html#the-toolchain-file), so it is not necessary to use toolchain input anymore. ....


---

_@charliermarsh approved on 2023-03-13 16:29_

---

_Comment by @charliermarsh on 2023-03-13 16:30_

@Boshen heard me even without the tag

---

_Comment by @stinodego on 2023-03-13 21:41_

For what it's worth, we moved away from `actions-rs/toolchain` as it is [unmaintained](https://github.com/actions-rs/toolchain/issues/216). We moved to [`dtolnay/rust-toolchain`](https://github.com/dtolnay/rust-toolchain), which seems to work fine.

---

_Comment by @MichaReiser on 2023-03-14 07:21_

> For what it's worth, we moved away from `actions-rs/toolchain` as it is [unmaintained](https://github.com/actions-rs/toolchain/issues/216). We moved to [`dtolnay/rust-toolchain`](https://github.com/dtolnay/rust-toolchain), which seems to work fine.

Hmm, that's a good point. I believe [`dtolnay/rust-toolchain`](https://github.com/dtolnay/rust-toolchain) doesn't respect the version specified in the `toolchain.toml`. But we could give [setup-rust-toolchain](https://github.com/actions-rust-lang/setup-rust-toolchain) a try.

Edit: `setup-rust-toolchain` doesn't work either because it ONLY installs the tools specified in the `toolchain.toml`. 



---

_Closed by @MichaReiser on 2023-03-14 07:50_

---

_Branch deleted on 2023-07-24 15:02_

---
