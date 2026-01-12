```yaml
number: 12043
title: Avoid consider PATH updated when the export is commented in the shellrc
type: pull_request
state: merged
author: JonCholas
labels:
  - bug
assignees: []
merged: true
base: main
head: joncholas/improve-update-shell
created_at: 2025-03-07T13:24:29Z
updated_at: 2025-03-08T14:45:06Z
url: https://github.com/astral-sh/uv/pull/12043
synced_at: 2026-01-12T16:10:06Z
```

# Avoid consider PATH updated when the export is commented in the shellrc

---

_@JonCholas_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The way the `tool update-shell` checks if the command to export the PATH exists or not in the RC files is a blind search, and therefore if finds the command inside comments.

example with .zshenv

This content
```
# uv
# export PATH="/Users/cholas/.local/bin:$PATH"
```

Generates the following msg
```
error: The executable directory /Users/cholas/.local/bin is not in PATH, but the Zsh configuration files are already up-to-date
```

With this change, that content won't be considered as configured and the following will be added
```
# uv
export PATH="/Users/cholas/.local/bin:$PATH"
```

This will make the `update-shell` more reliable

## Test Plan

I tested with and without the change with commented export in zsh in mac. Tested running `cargo run -- tool update-shell`


---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/update_shell.rs`:76 on 2025-03-07 15:50_

Could we instead skip lines that start with `#`?

---

_@charliermarsh reviewed on 2025-03-07 15:50_

---

_@charliermarsh approved on 2025-03-08 14:23_

---

_Label `bug` added by @charliermarsh on 2025-03-08 14:23_

---

_Merged by @charliermarsh on 2025-03-08 14:45_

---

_Closed by @charliermarsh on 2025-03-08 14:45_

---
