```yaml
number: 13130
title: "[docs] Changed fish completions append `>>` to overwrite `>`"
type: pull_request
state: merged
author: ndrew222
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2025-04-27T13:26:48Z
updated_at: 2025-04-28T00:52:15Z
url: https://github.com/astral-sh/uv/pull/13130
synced_at: 2026-01-10T11:10:40Z
```

# [docs] Changed fish completions append `>>` to overwrite `>`

---

_Pull request opened by @ndrew222 on 2025-04-27 13:26_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Changed the appending `>>` to an overwriting `>`
The reason being that if the `echo 'uv generate-shell-completion fish | source' >> ~/.config/fish/completions/uv.fish` command were to be executed twice, it would result in a redundant mass in the fish file.
Instead, a direct write would be more appropriate, allowing for the completions to be updated if necessary, should `uv` get more commands in an update.

Granted, it's not assumed that the completions command is going to be run more than once on the other shells, but this is a trivial change that I think would provide marginal improvements to whomever may need to use it


---

_@charliermarsh approved on 2025-04-28 00:52_

This seems reasonable given that the uv completions have their own files.

---

_Label `documentation` added by @charliermarsh on 2025-04-28 00:52_

---

_Merged by @charliermarsh on 2025-04-28 00:52_

---

_Closed by @charliermarsh on 2025-04-28 00:52_

---
