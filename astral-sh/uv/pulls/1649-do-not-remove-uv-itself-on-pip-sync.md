```yaml
number: 1649
title: Do not remove uv itself on pip sync
type: pull_request
state: merged
author: flyaroundme
labels:
  - bug
assignees: []
merged: true
base: main
head: do-not-uninstall-self-in-venv
created_at: 2024-02-18T14:35:57Z
updated_at: 2024-02-18T16:25:58Z
url: https://github.com/astral-sh/uv/pull/1649
synced_at: 2026-01-10T15:33:24Z
```

# Do not remove uv itself on pip sync

---

_Pull request opened by @flyaroundme on 2024-02-18 14:35_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Added `uv` to the list of the preserved packages when building the installer plan. In that case `uv` is not going to be removed when, for example, using `python -m uv pip sync requirements.txt` when requirements.txt does not contain `uv`, but `uv` is installed in that venv.

Closes #1631 

## Test Plan

Got through the example attached to https://github.com/astral-sh/uv/issues/1631 and did not see the uv deletion in the output
```
$ python -m uv pip sync requirements.txt
Installed 1 package in 20ms
 + ruff==0.2.2
```


---

_@charliermarsh approved on 2024-02-18 14:37_

---

_Label `bug` added by @charliermarsh on 2024-02-18 14:37_

---

_Comment by @charliermarsh on 2024-02-18 14:37_

Nice, thank you!

---

_Comment by @charliermarsh on 2024-02-18 14:38_

Welcome to the project.

---

_Merged by @charliermarsh on 2024-02-18 14:39_

---

_Closed by @charliermarsh on 2024-02-18 14:39_

---
