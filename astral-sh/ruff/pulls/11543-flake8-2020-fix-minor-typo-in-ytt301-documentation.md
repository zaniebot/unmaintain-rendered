```yaml
number: 11543
title: "[`flake8-2020`] fix minor typo in `YTT301` documentation"
type: pull_request
state: merged
author: Amar1729
labels:
  - documentation
assignees: []
merged: true
base: main
head: typo/YTT301-doc
created_at: 2024-05-26T00:52:00Z
updated_at: 2024-05-28T11:19:47Z
url: https://github.com/astral-sh/ruff/pull/11543
synced_at: 2026-01-12T15:55:38Z
```

# [`flake8-2020`] fix minor typo in `YTT301` documentation

---

_@Amar1729_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Current doc says `sys.version[0]` will select the first digit of a major version number (correct) then as an example says 

> e.g., `"3.10"` would evaluate to `"1"`

(would actually evaluate to `"3"`). Changed the example version to a two-digit number to make the problem more clear.

## Test Plan

<!-- How was it tested? -->
ran the following:
- `cargo run -p ruff -- check crates/ruff_linter/resources/test/fixtures/flake8_2020/YTT301.py --no-cache`
- `cargo insta review`
- `cargo test`
which all passed.

---

_Comment by @github-actions[bot] on 2024-05-26 01:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-05-26 17:23_

---

_Label `documentation` added by @charliermarsh on 2024-05-26 17:23_

---

_Comment by @charliermarsh on 2024-05-26 17:23_

Thanks! A strange motivating example but at least now it's consistent and correct in the rule :)

---

_Merged by @charliermarsh on 2024-05-26 17:23_

---

_Closed by @charliermarsh on 2024-05-26 17:23_

---

_Comment by @charliermarsh on 2024-05-26 17:23_

Much appreciated.

---

_Branch deleted on 2024-05-28 11:18_

---

_Comment by @Amar1729 on 2024-05-28 11:19_

ha, I also had a little laugh when I wrote that example. looking forward to python 10 ;)

---
