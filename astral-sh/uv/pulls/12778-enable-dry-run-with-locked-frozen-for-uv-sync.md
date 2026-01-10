```yaml
number: 12778
title: "Enable `--dry-run` with `--locked` / `--frozen` for `uv sync`"
type: pull_request
state: merged
author: christeefy
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: uv/sync/dry-run-w-frozen-and-locked
created_at: 2025-04-09T13:02:02Z
updated_at: 2025-04-14T17:49:37Z
url: https://github.com/astral-sh/uv/pull/12778
synced_at: 2026-01-10T11:10:40Z
```

# Enable `--dry-run` with `--locked` / `--frozen` for `uv sync`

---

_Pull request opened by @christeefy on 2025-04-09 13:02_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Closes #12687. 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Added the corresponding integration tests for:
- `uv sync --dry-run --locked`
  - [x] Preview lock changes
  - [x] Errors if lockfile is out-of-date
- `uv sync --dry-run --frozen`
  - [x] Preview lock changes

---

_@christeefy reviewed on 2025-04-10 19:06_

---

_Review comment by @christeefy on `crates/uv/tests/it/sync.rs`:7942 on 2025-04-10 19:06_

With `--locked --dry-run`, we perform an environment check first before erroring on the outdated lockfile.

---

_Comment by @christeefy on 2025-04-10 20:29_

Just added logic for performing the environment check, as part of `--dry-run --locked`.

It involved some more changes[^1]:
- I chose to refactor functionality some repetition into `identify_installation_target`. It involves quite a bit of borrowing and lifetimes, but it compiles. If this is an anti-pattern, let me know how can I do it better.
- 🚨 I've also modified `ProjectError::LockMismatch` to hold an associated `Lock`. This allows me to run an environment check via `do_sync` (which needs `&lock`) before returning the error. I believe this isn't a breaking change, but please correct me if I'm mistaken.

[^1]: For comparison, see the code diff _without_ the env check [here](https://github.com/astral-sh/uv/pull/12778/files/1f275a841b21e2e3947d13aabd796f074e19c4db).

---

_Marked ready for review by @christeefy on 2025-04-10 20:30_

---

_@charliermarsh reviewed on 2025-04-14 12:39_

Nice, thank you.

---

_@charliermarsh approved on 2025-04-14 12:50_

---

_Label `enhancement` added by @charliermarsh on 2025-04-14 13:03_

---

_Label `cli` added by @charliermarsh on 2025-04-14 13:03_

---

_Merged by @charliermarsh on 2025-04-14 13:08_

---

_Closed by @charliermarsh on 2025-04-14 13:08_

---

_Branch deleted on 2025-04-14 17:49_

---
