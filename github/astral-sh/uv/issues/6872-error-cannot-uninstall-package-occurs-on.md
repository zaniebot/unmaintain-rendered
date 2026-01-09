---
number: 6872
title: "`Error: cannot uninstall package` occurs on installing a package a second time"
type: issue
state: closed
author: mweislley
labels:
  - compatibility
assignees: []
created_at: 2024-08-30T14:23:24Z
updated_at: 2024-08-30T19:02:07Z
url: https://github.com/astral-sh/uv/issues/6872
synced_at: 2026-01-07T13:12:17-06:00
---

# `Error: cannot uninstall package` occurs on installing a package a second time

---

_Issue opened by @mweislley on 2024-08-30 14:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm working on upgrading my project to use `uv` and ran into this error when I installed the `requirements.txt` file a second time. I've narrowed it down to exactly a particular dependency that gives the error.

**Reproduction steps**
```sh
vscode ➜ /workspaces $ uv venv -p 3.12.3 test-venv
Using Python 3.12.3
Creating virtualenv at: test-venv
Activate with: source test-venv/bin/activate
vscode ➜ /workspaces $ source test-venv/bin/activate
(test-venv) vscode ➜ /workspaces $ uv pip install suds-community==0.8.5
Resolved 1 package in 99ms
Prepared 1 package in 65ms
Installed 1 package in 2ms
 + suds-community==0.8.5
(test-venv) vscode ➜ /workspaces $ uv pip install suds-community==0.8.5
Resolved 1 package in 1ms
error: Cannot uninstall package; `top_level.txt` file not found at: test-venv/lib/python3.12/site-packages/suds_community.egg-info/top_level.txt
```

**Expected**
Don't throw an error when installing a package that was already installed properly

I've included a more detailed output with `RUST_LOG=TRACE` and `-v`
[reinstall.log](https://github.com/user-attachments/files/16818419/reinstall.log)


---

_Comment by @charliermarsh on 2024-08-30 14:25_

I'll take a look, thanks.

---

_Comment by @charliermarsh on 2024-08-30 14:30_

The structure of the wheel is slightly problematic... It seems to include a directory at the top-level that shouldn't be there. I'd recommend upgrading the version honestly, but we can also make uv robust to this.

---

_Comment by @charliermarsh on 2024-08-30 14:35_

Another way to explain it is that because there are two separate `suds_community` directories here, it appears to uv as if the package were installed twice:

```
❯ ls -l .venv/lib/python3.12/site-packages/
total 24
drwxr-xr-x@  3 crmarsh  staff    96 Aug 30 10:30 __pycache__
-rw-r--r--   1 crmarsh  staff    18 Aug 30 10:30 _virtualenv.pth
-rw-r--r--   1 crmarsh  staff  4378 Aug 30 10:30 _virtualenv.py
drwxr-xr-x   9 crmarsh  staff   288 Aug 30 10:31 pip
drwxr-xr-x  11 crmarsh  staff   352 Aug 30 10:30 pip-24.2.dist-info
drwxr-xr-x  28 crmarsh  staff   896 Aug 30 10:31 suds
drwxr-xr-x   9 crmarsh  staff   288 Aug 30 10:31 suds_community-0.8.5.dist-info
drwxr-xr-x   6 crmarsh  staff   192 Aug 30 10:31 suds_community.egg-info
```

---

_Label `compatibility` added by @charliermarsh on 2024-08-30 14:35_

---

_Comment by @mweislley on 2024-08-30 14:55_

Thank you!
Yes, I did notice the latest version of that package doesn't have this issues (`1.2.0`) and I'm attempting to upgrade it but the dependency that uses it ALSO has a patch it applies to `suds-community` so it's becoming complicated. If `uv` would handle this, that would make my life easier.

---

_Comment by @charliermarsh on 2024-08-30 14:56_

Yeah totally understand -- I generally don't like "upgrade it" as a solution for users (sometimes there's no choice). I think we can be more graceful here but you might be shown a warning or something when we reinstall (rather than failing).

---

_Referenced in [astral-sh/uv#6881](../../astral-sh/uv/pulls/6881.md) on 2024-08-30 17:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-30 17:39_

---

_Closed by @charliermarsh on 2024-08-30 19:02_

---

_Closed by @charliermarsh on 2024-08-30 19:02_

---

_Referenced in [iree-org/iree#18761](../../iree-org/iree/issues/18761.md) on 2024-10-11 18:50_

---

_Referenced in [astral-sh/uv#16351](../../astral-sh/uv/pulls/16351.md) on 2025-10-17 21:50_

---
