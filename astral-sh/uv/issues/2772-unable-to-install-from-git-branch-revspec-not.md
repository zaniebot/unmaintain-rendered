---
number: 2772
title: Unable to install from git branch. revspec not found
type: issue
state: closed
author: krishnasism
labels:
  - bug
assignees: []
created_at: 2024-04-02T12:06:34Z
updated_at: 2024-04-04T02:05:56Z
url: https://github.com/astral-sh/uv/issues/2772
synced_at: 2026-01-10T01:23:22Z
---

# Unable to install from git branch. revspec not found

---

_Issue opened by @krishnasism on 2024-04-02 12:06_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hi,
I am trying to install a custom branch from git directly, but it fails with an error `revspec not found`

```zsh
% uv pip install --no-cache-dir  git+https://github.com/weareprestatech/pdfminer.six.git@20240222#egg=pdfminer-six
Updating https://github.com/weareprestatech/pdfminer.six.git (20240222)                                                                                                                                              error: Failed to build source distribution
  Caused by: Git operation failed
  Caused by: revspec '20240222' not found; class=Reference (4); code=NotFound (-3)
```

This works fine without uv (i.e., only pip) or with uv if I don't mention the branch

Both of the following work:
```zsh
% pip install --no-cache-dir  git+https://github.com/weareprestatech/pdfminer.six.git@20240222#egg=pdfminer-six 
% uv pip install --no-cache-dir  git+https://github.com/weareprestatech/pdfminer.six.git#egg=pdfminer-six 
```

**System**: MacOS M2


---

_Label `bug` added by @charliermarsh on 2024-04-02 12:26_

---

_Comment by @charliermarsh on 2024-04-02 21:19_

The problem is that we treat `20240222` like a "short commit", whereas here it's actually a branch.

---

_Comment by @charliermarsh on 2024-04-02 21:19_

You can fix this by using the full Git SHA instead of `20240222`, but we should support it anyway.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-02 21:24_

---

_Comment by @krishnasism on 2024-04-02 21:27_

Thanks :) I'll try this in the meanwhile

---

_Referenced in [astral-sh/uv#2795](../../astral-sh/uv/pulls/2795.md) on 2024-04-03 03:13_

---

_Closed by @charliermarsh on 2024-04-04 02:05_

---
