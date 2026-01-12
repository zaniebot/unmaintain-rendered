```yaml
number: 3006
title: "chore: Remove `default_members` from `Cargo.toml`"
type: pull_request
state: merged
author: MichaReiser
labels: []
assignees: []
merged: true
base: main
head: chore/default-members
created_at: 2023-02-18T10:24:42Z
updated_at: 2023-02-19T15:21:42Z
url: https://github.com/astral-sh/ruff/pull/3006
synced_at: 2026-01-12T04:39:44Z
```

# chore: Remove `default_members` from `Cargo.toml`

---

_Pull request opened by @MichaReiser on 2023-02-18 10:24_

This PR removes the `default_members` from the workspace configuration. 

## Why

I'm not familiar with the motivation for why the `default_members` setting was added initially, and I do not object to keeping it. I'll explain my motivation for removing it below. 

My main reason for removing the `default_members` override is that new contributors may not know that `cargo test`, `cargo build`, and other commands only run on a subset of crates. They may then be surprised that their PRs are failing in CI, but everything works locally. 

My guess why `default_members` was added is to speed up the development workflow. That's fair, but I question the value because `ruff` is the heaviest crate to build. 




---

_Merged by @charliermarsh on 2023-02-19 12:18_

---

_Closed by @charliermarsh on 2023-02-19 12:18_

---

_Comment by @charliermarsh on 2023-02-19 12:18_

(Agreed.)

---

_Comment by @charliermarsh on 2023-02-19 15:21_

Oh, I guess the reason this was present was to make `ruff_cli` the default `run` invocation. You now have to do `cargo run -p ruff_cli` to run Ruff from root.

---

_Comment by @charliermarsh on 2023-02-19 15:21_

Not sure if there's a way to fix that, or if we need to update the docs anywhere.

---
