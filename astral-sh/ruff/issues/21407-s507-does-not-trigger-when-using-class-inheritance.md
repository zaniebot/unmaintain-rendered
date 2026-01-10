```yaml
number: 21407
title: S507 does not trigger when using class inheritance
type: issue
state: open
author: schnow265
labels:
  - rule
assignees: []
created_at: 2025-11-12T15:14:20Z
updated_at: 2025-11-12T20:54:58Z
url: https://github.com/astral-sh/ruff/issues/21407
synced_at: 2026-01-10T11:10:00Z
```

# S507 does not trigger when using class inheritance

---

_Issue opened by @schnow265 on 2025-11-12 15:14_

### Summary

When creating a class which inherits from `paramiko.SSHClient` and then using `self.set_missing_host_key_policy(paramiko.AutoAddPolicy)`, the linter rule is not triggered. The same applies when creating an instance of the custom class and calling the function there.

I have included an example:

```python
import paramiko

ssh_client = paramiko.SSHClient()
ssh_client.set_missing_host_key_policy(paramiko.AutoAddPolicy)  # triggers rule


class Test(paramiko.SSHClient):
    def __init__(self):
        super().__init__()
        self.set_missing_host_key_policy(paramiko.AutoAddPolicy)  # does not trigger rule

client = Test()
client.set_missing_host_key_policy(paramiko.AutoAddPolicy)  # also doesn't trigger rule
```

You can check it by pasting my python snippet into a file and then using the following command:

```shell
ruff check --select S507 ./path/to/file.py
```

### Version

ruff 0.14.4

---

_Comment by @ntBre on 2025-11-12 20:54_

Thanks for the report! I think we could try to resolve base classes within the same file at least, even if multi-file analysis is still unavailable.

---

_Label `rule` added by @ntBre on 2025-11-12 20:54_

---
