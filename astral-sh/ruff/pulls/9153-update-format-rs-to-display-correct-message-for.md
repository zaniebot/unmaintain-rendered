```yaml
number: 9153
title: Update format.rs to display correct message for already formatted files
type: pull_request
state: merged
author: VictorGob
labels:
  - cli
assignees: []
merged: true
base: main
head: format_check_better_message
created_at: 2023-12-16T00:13:03Z
updated_at: 2023-12-18T09:00:34Z
url: https://github.com/astral-sh/ruff/pull/9153
synced_at: 2026-01-12T15:55:27Z
```

# Update format.rs to display correct message for already formatted files

---

_@VictorGob_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

New messages for "format" mode. 
Fixes #9132 

## Test Plan

<!-- How was it tested? -->

I ran the tests specified in `CONTRIBUTING.md`
```bash
cargo run -p ruff_cli -- check /path/to/some_files.py --no-cache
cargo run -p ruff_cli -- format --check /path/to/some_files.py --no-cache

cargo clippy --workspace --all-targets --all-features -- -D warnings
RUFF_UPDATE_SCHEMA=1 cargo test
pre-commit run --all-files --show-diff-on-failure
```

**Note:** In case no files are detected, either correctly formatted, changed, or unchanged, it does not display a message. Wouldn't it be better to show some message in this case?

---

_Comment by @github-actions[bot] on 2023-12-16 00:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2023-12-16 13:22_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:518 on 2023-12-16 13:22_

I think we want to apply this change only in the event that we have `FormatMode::Check | FormatMode::Diff` -- do you mind tweaking, similar to "would be reformatted" below?

---

_Review comment by @VictorGob on `crates/ruff_cli/src/commands/format.rs`:518 on 2023-12-17 21:59_

Sorry, I don't understand: _Check and Diff_ here are like the lower branch of the 'if'

---

_@VictorGob reviewed on 2023-12-17 21:59_

---

_@charliermarsh reviewed on 2023-12-18 00:40_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/format.rs`:518 on 2023-12-18 00:40_

@VictorGob - I pushed an update to illustrate what I meant. What do you think of this approach?

---

_Merged by @charliermarsh on 2023-12-18 05:07_

---

_Closed by @charliermarsh on 2023-12-18 05:07_

---

_Label `cli` added by @charliermarsh on 2023-12-18 05:07_

---

_@VictorGob reviewed on 2023-12-18 09:00_

---

_Review comment by @VictorGob on `crates/ruff_cli/src/commands/format.rs`:518 on 2023-12-18 09:00_

**Chef kiss** :ok_hand: 

---
