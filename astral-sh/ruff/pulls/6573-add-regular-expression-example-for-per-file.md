```yaml
number: 6573
title: "Add regular expression example for `per-file-ignores`"
type: pull_request
state: merged
author: noklam
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix/tool.ruff.per-file-ignores
created_at: 2023-08-14T21:26:21Z
updated_at: 2023-08-14T22:02:41Z
url: https://github.com/astral-sh/ruff/pull/6573
synced_at: 2026-01-12T15:55:21Z
```

# Add regular expression example for `per-file-ignores`

---

_@noklam_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Hi! This is my first PR to `ruff` and thanks for this amazing project. While I am working on my project, I need to set different rules for my `test/` folder and the main `src` package.

It's not immediately obvious that the [`tool.ruff.per-file-ignores`](https://beta.ruff.rs/docs/settings/#per-file-ignores) support regular expression. It is useful to set rules on directory level. The PR add a simple example to make it clear this support regex.


<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `documentation` added by @charliermarsh on 2023-08-14 21:54_

---

_Comment by @charliermarsh on 2023-08-14 21:54_

Thanks! I tweaked the example a little bit. Appreciate the PR.

---

_Comment by @noklam on 2023-08-14 21:58_

Thanks @charliermarsh!

---

_Merged by @charliermarsh on 2023-08-14 22:02_

---

_Closed by @charliermarsh on 2023-08-14 22:02_

---
