```yaml
number: 2221
title: Show appropriate activation command based on shell detection
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/shell
created_at: 2024-03-05T21:42:20Z
updated_at: 2024-03-05T22:28:18Z
url: https://github.com/astral-sh/uv/pull/2221
synced_at: 2026-01-12T16:04:55Z
```

# Show appropriate activation command based on shell detection

---

_@charliermarsh_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Closes https://github.com/astral-sh/uv/issues/2174.

## Test Plan

On Nushell:

```
(uv) ~/workspace/uv> cargo run venv                                                                                                                                                                                      
Using Python 3.12.0 interpreter at: /Users/crmarsh/workspace/uv/.venv/bin/python3
Creating virtualenv at: .venv
Activate with: overlay use .venv/bin/activate.nu
```

On Bash:

```
❯ cargo run venv "foo bar"
Using Python 3.12.0 interpreter at: /Users/crmarsh/.local/share/rtx/installs/python/3.12.0/bin/python3
Creating virtualenv at: foo bar
Activate with: source 'foo bar/bin/activate'
```


---

_Label `bug` added by @charliermarsh on 2024-03-05 21:42_

---

_Marked ready for review by @charliermarsh on 2024-03-05 21:42_

---

_Merged by @charliermarsh on 2024-03-05 22:28_

---

_Closed by @charliermarsh on 2024-03-05 22:28_

---

_Branch deleted on 2024-03-05 22:28_

---
