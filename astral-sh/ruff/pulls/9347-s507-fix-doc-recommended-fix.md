```yaml
number: 9347
title: "[S507] fix doc recommended fix"
type: pull_request
state: merged
author: mikaelarguedas
labels:
  - bug
  - documentation
assignees: []
merged: true
base: main
head: s507_fix_doc
created_at: 2024-01-01T15:05:39Z
updated_at: 2024-01-02T06:08:48Z
url: https://github.com/astral-sh/ruff/pull/9347
synced_at: 2026-01-12T15:55:28Z
```

# [S507] fix doc recommended fix

---

_@mikaelarguedas_

paramiko `set_missing_host_key_policy` has mandatory positional arg.
The [current documentation](https://docs.astral.sh/ruff/rules/ssh-no-host-key-verification/#example) leads to non-running code

```
>>> from paramiko import client
>>> ssh_client = client.SSHClient()
>>> ssh_client.set_missing_host_key_policy()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: SSHClient.set_missing_host_key_policy() missing 1 required positional argument: 'policy'
```

This PR modifies the documentation to set the policy to the [default `RejectPolicy`](https://docs.paramiko.org/en/latest/api/client.html#paramiko.client.SSHClient.set_missing_host_key_policy)

---

_Comment by @github-actions[bot] on 2024-01-01 15:18_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @charliermarsh on 2024-01-02 01:58_

---

_Label `documentation` added by @charliermarsh on 2024-01-02 01:58_

---

_Merged by @charliermarsh on 2024-01-02 01:59_

---

_Closed by @charliermarsh on 2024-01-02 01:59_

---

_Comment by @charliermarsh on 2024-01-02 01:59_

Thank you! Good catch.

---

_Branch deleted on 2024-01-02 06:08_

---
