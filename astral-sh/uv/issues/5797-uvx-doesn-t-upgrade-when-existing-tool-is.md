```yaml
number: 5797
title: "`uvx` doesn't upgrade when existing tool is installed"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
created_at: 2024-08-05T18:44:34Z
updated_at: 2024-08-06T01:52:13Z
url: https://github.com/astral-sh/uv/issues/5797
synced_at: 2026-01-12T15:58:58Z
```

# `uvx` doesn't upgrade when existing tool is installed

---

_@charliermarsh_

This is a confusing workflow:

```
❯ uv tool install ruff==0.0.60
Resolved 1 package in 5ms
Installed 1 package in 3ms
 + ruff==0.0.60
Installed 1 executable: ruff

❯ uvx ruff --version
ruff 0.0.60

❯ uvx --upgrade ruff --version
ruff 0.0.60
```


---

_Label `bug` added by @charliermarsh on 2024-08-05 18:44_

---

_Label `preview` added by @charliermarsh on 2024-08-05 18:44_

---

_Comment by @charliermarsh on 2024-08-05 18:47_

Not 100% sure what the semantics should be here, it's actually pretty tricky. _Even if_ we respected `--upgrade` here properly, we wouldn't upgrade the installed tool past `0.0.60`, since the receipt has `ruff==0.0.60` in it. So, like, even if you did `uvx --upgrade ruff` and we gave you latest Ruff, running `uvx ruff` again would... give you `0.0.60`.

I wonder if we should avoid using installed tools _ever_ in `uvx` now that we have cached environments.


---

_Comment by @charliermarsh on 2024-08-05 18:48_

\cc @zanieb 

---

_Comment by @zanieb on 2024-08-05 18:52_

I think using the installed tool wasn't intended as a performance thing, rather it's part of the experience to use the version you've chosen to install by default. This becomes more important when we consider project-level tools where `uvx` would invoke the project-specific version by default. We could reconsider that experience.

The `--isolated` flag was intended to ignore the installed version. Is there an equivalent for that now?

A `--latest` flag was discussed in the design, should we add that?

I don't think `--upgrade` is even intended to be available in `uvx` since it's an ephemeral environment? If resolver options are passed, we should probably just ignore the receipt entirely?

---

_Comment by @charliermarsh on 2024-08-05 18:54_

Yeah `--isolated` already exists.

---

_Comment by @charliermarsh on 2024-08-05 18:55_

I guess we can ignore the installed tool if you pass `--upgrade` or `--reinstall`?

---

_Comment by @charliermarsh on 2024-08-05 18:57_

@zanieb -- I might expect `ruff@latest` to work

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-05 19:07_

---

_Closed by @charliermarsh on 2024-08-06 01:52_

---
