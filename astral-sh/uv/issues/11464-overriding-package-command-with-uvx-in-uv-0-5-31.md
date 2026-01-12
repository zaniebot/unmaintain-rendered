```yaml
number: 11464
title: Overriding package command with uvx in uv 0.5.31 is broken
type: issue
state: closed
author: sillock1
labels:
  - bug
assignees: []
created_at: 2025-02-12T23:07:07Z
updated_at: 2025-02-12T23:29:52Z
url: https://github.com/astral-sh/uv/issues/11464
synced_at: 2026-01-12T16:00:37Z
```

# Overriding package command with uvx in uv 0.5.31 is broken

---

_@sillock1_

### Summary

I'm trying to use the uvx command to override a package and since the latest uv release (0.5.31) this no longer works.

I.E I want to use the sarif command in [sarif-tools](https://pypi.org/project/sarif-tools/)

Using uv <= 0.5.30 I get the following

> uvx --from sarif-tools@latest sarif version
> Installed 20 packages in 7ms
> 3.0.4

When using uv 0.5.31 I get this

> uvx --from sarif-tools@latest sarif version
> Installed 20 packages in 7ms
> The executable `sarif-tools` was not found.
> warning: An executable named `sarif-tools` is not provided by package `sarif-tools`.
> The following executables are provided by ``sarif-tools``:
> - sarif
> Consider using `uvx --from sarif-tools <EXECUTABLE_NAME>` instead.
> 



### Platform

Linux 6.12.9 x86_64 GNU/Linux (NixOS)

### Version

uv 0.5.31

### Python version

Python 3.13.1

---

_Label `bug` added by @sillock1 on 2025-02-12 23:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-02-12 23:07_

---

_Comment by @charliermarsh on 2025-02-12 23:07_

That seems like my mistake.

---

_Comment by @charliermarsh on 2025-02-12 23:19_

Apologies, we somehow lacked test coverage for this. It's fixed in https://github.com/astral-sh/uv/pull/11465.

---

_Closed by @charliermarsh on 2025-02-12 23:29_

---

_Closed by @charliermarsh on 2025-02-12 23:29_

---
