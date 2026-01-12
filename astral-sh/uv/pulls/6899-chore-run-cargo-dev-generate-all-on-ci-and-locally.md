```yaml
number: 6899
title: "chore: run `cargo dev generate-all` on CI and locally"
type: pull_request
state: merged
author: mkniewallner
labels:
  - testing
assignees: []
merged: true
base: main
head: chore/run-dev-generate-all-on-ci
created_at: 2024-08-31T20:11:56Z
updated_at: 2024-08-31T23:07:16Z
url: https://github.com/astral-sh/uv/pull/6899
synced_at: 2026-01-12T16:07:35Z
```

# chore: run `cargo dev generate-all` on CI and locally

---

_@mkniewallner_

## Summary

Noticed that running `cargo dev generate-all` on `main` produced changes and saw that that the command is not run on the CI nor as a pre-commit hook.

Not sure if having the command running as a pre-commit hook is something we want, so I can remove it if you prefer. I find that nice to have as it's probably easy to forget to run it, especially for new contributors (and it will only run if there are changes in `uv_cli` or `uv_settings` crates).

## Test Plan

- Added `cargo dev generate-all --mode check` on the CI, which produced [this failing job](https://github.com/astral-sh/uv/actions/runs/10648055597/job/29516699393)
- Ran `cargo dev generate-all` locally and committed the changes, which produced [this succeeding job](https://github.com/astral-sh/uv/actions/runs/10648076910/job/29516744942)

---

_Marked ready for review by @mkniewallner on 2024-08-31 20:36_

---

_@charliermarsh approved on 2024-08-31 23:03_

Thanks. I also noticed this recently. I think we have some actual Rust tests that run `check` for a few of the outputs but not all.

---

_Merged by @charliermarsh on 2024-08-31 23:03_

---

_Closed by @charliermarsh on 2024-08-31 23:03_

---

_Label `testing` added by @charliermarsh on 2024-08-31 23:03_

---

_Branch deleted on 2024-08-31 23:07_

---
