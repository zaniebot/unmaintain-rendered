```yaml
number: 6759
title: Unable to use uv tool install for specific versions 
type: issue
state: closed
author: damienrj
labels:
  - bug
  - duplicate
assignees: []
created_at: 2024-08-28T15:28:35Z
updated_at: 2024-08-28T16:40:50Z
url: https://github.com/astral-sh/uv/issues/6759
synced_at: 2026-01-12T15:59:07Z
```

# Unable to use uv tool install for specific versions 

---

_@damienrj_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```
❯ uv --version
uv 0.3.5 (6c62d9fbf 2024-08-27)
```

OSX

```
❯ uv tool install codecov-cli@0.7.4
error: Failed to parse: `codecov-cli@0.7.4`
  Caused by: Expected path (`/Users/damien/Development/mlkite/0.7.4`) to end in a supported file extension: `.whl`, `.zip`, `.tar.gz`, `.tar.bz2`, `.tar.xz`, or `.tar.zst`
codecov-cli@0.7.4
```

I am trying to install a specific version but get the above error.


---

_Comment by @charliermarsh on 2024-08-28 15:30_

Ahh, this needs to be `uv tool install codecov-cli==0.7.4`.

Tracked here: https://github.com/astral-sh/uv/issues/6535.

---

_Comment by @charliermarsh on 2024-08-28 15:31_

We'll do this today, you're not the first to run into it.

---

_Label `bug` added by @charliermarsh on 2024-08-28 15:31_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 15:32_

---

_Comment by @damienrj on 2024-08-28 15:32_

Thanks, I didn't find that when I was searching. Great project!

---

_Label `duplicate` added by @zanieb on 2024-08-28 15:34_

---

_Closed by @charliermarsh on 2024-08-28 16:40_

---
