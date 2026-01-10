```yaml
number: 556
title: Overwrite individual files when reflinking
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/overwrite
created_at: 2023-12-04T23:56:20Z
updated_at: 2023-12-04T23:59:37Z
url: https://github.com/astral-sh/uv/pull/556
synced_at: 2026-01-10T15:44:44Z
```

# Overwrite individual files when reflinking

---

_Pull request opened by @charliermarsh on 2023-12-04 23:56_

Similar to #516, but for individual files.

## Test Plan

Ran:

```sh
cargo run -p puffin-cli -- pip-uninstall plaid-python
mkdir -p /Users/crmarsh/workspace/puffin/.venv/lib/python3.10/site-packages/tests
echo "x=1" > /Users/crmarsh/workspace/puffin/.venv/lib/python3.10/site-packages/tests/__init__.py
cargo run -p puffin-cli -- pip-sync requirements.txt --no-cache --verbose
```


---

_Label `bug` added by @charliermarsh on 2023-12-04 23:56_

---

_Merged by @charliermarsh on 2023-12-04 23:59_

---

_Closed by @charliermarsh on 2023-12-04 23:59_

---

_Branch deleted on 2023-12-04 23:59_

---
