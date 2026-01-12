```yaml
number: 3250
title: Warn when an unsupported Python version is encountered
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: feat/add-warning-message-unsupported-python-version
created_at: 2024-04-24T16:19:23Z
updated_at: 2024-04-24T17:51:58Z
url: https://github.com/astral-sh/uv/pull/3250
synced_at: 2026-01-12T16:05:31Z
```

# Warn when an unsupported Python version is encountered

---

_@zanieb_

I rebased https://github.com/astral-sh/uv/pull/2757 then realized that we want to implement this for more than `uv venv`.

Closes https://github.com/astral-sh/uv/issues/2587
Closes https://github.com/astral-sh/uv/issues/2757

```
❯ cargo run -q -- pip install -p /Users/mz/bin/python3.7 anyio
warning: uv is only compatible with Python 3.8+, found Python 3.7.17.
Audited 1 package in 84ms

❯ cargo run -q -- venv -p /Users/mz/bin/python3.7
warning: uv is only compatible with Python 3.8+, found Python 3.7.17.
Using Python 3.7.17 interpreter at: /Users/mz/bin/python3.7
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

---

_Review requested from @charliermarsh by @zanieb on 2024-04-24 16:26_

---

_Review requested from @konstin by @zanieb on 2024-04-24 16:26_

---

_@konstin approved on 2024-04-24 16:35_

---

_Merged by @zanieb on 2024-04-24 17:51_

---

_Closed by @zanieb on 2024-04-24 17:51_

---

_Branch deleted on 2024-04-24 17:51_

---
