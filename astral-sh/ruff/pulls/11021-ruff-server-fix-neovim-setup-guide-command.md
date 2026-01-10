```yaml
number: 11021
title: "`ruff server`: fix Neovim setup guide command"
type: pull_request
state: merged
author: MithicSpirit
labels:
  - documentation
assignees: []
merged: true
base: main
head: nvim-lsp-setup
created_at: 2024-04-19T01:55:24Z
updated_at: 2024-04-19T02:56:44Z
url: https://github.com/astral-sh/ruff/pull/11021
synced_at: 2026-01-10T22:37:01Z
```

# `ruff server`: fix Neovim setup guide command

---

_Pull request opened by @MithicSpirit on 2024-04-19 01:55_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes the Neovim setup guide command for `ruff server`. Note that
`lspconfig.*.setup` expects a table as its first argument, even if it is empty,
as a `nil` value causes an error.


---

_Review requested from @dhruvmanila by @charliermarsh on 2024-04-19 02:04_

---

_Comment by @charliermarsh on 2024-04-19 02:05_

Thanks! Tagging @dhruvmanila (looks reasonable to me, but I don't know anything about NeoVim).

---

_@dhruvmanila approved on 2024-04-19 02:53_

---

_Label `documentation` added by @dhruvmanila on 2024-04-19 02:53_

---

_Merged by @dhruvmanila on 2024-04-19 02:54_

---

_Closed by @dhruvmanila on 2024-04-19 02:54_

---

_Branch deleted on 2024-04-19 02:56_

---
