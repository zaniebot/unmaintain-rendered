---
number: 12234
title: "--offline is not offline for \"git+https\" sources."
type: issue
state: closed
author: konstin
labels:
  - bug
  - good first issue
  - help wanted
assignees: []
created_at: 2025-03-17T09:53:35Z
updated_at: 2025-04-04T16:02:55Z
url: https://github.com/astral-sh/uv/issues/12234
synced_at: 2026-01-07T13:12:18-06:00
---

# --offline is not offline for "git+https" sources.

---

_Issue opened by @konstin on 2025-03-17 09:53_

 _Originally posted by @andrei-korshikov in [#942](https://github.com/astral-sh/uv/issues/942#issuecomment-2728480835)_

I'm on uv 0.6.6 (c1a0bb85e 2025-03-12). Some of my dependencies are `git+https`, e.g. I have
 
```python
# /// script
# dependencies = [
#   "pycurl @ git+https://github.com/pycurl/pycurl.git",
#   ]
# ///
```
 
On every run of `uv run --offline my_script.py` I see `Resolving dependencies...` message and get these messages on `stderr` (and see corresponding traffic in Wireshark):

```
Updating https://github.com/pycurl/pycurl.git (HEAD)
Updated https://github.com/pycurl/pycurl.git (d92395d9fe1e18365c4e4812f4351bd6985872e1)
```

If this behaviour is intended, I think it should be documented. Documentation for `--offline` reads: "Disable network access. When disabled, uv will only use locally cached data and locally available files.", and as we see for "git+https" it is definitely not so.

---

Off-topic: and huge thank you for `uv`!:) 



---

_Label `bug` added by @konstin on 2025-03-17 09:53_

---

_Comment by @charliermarsh on 2025-03-17 13:15_

Yeah this seems like an oversight.

(As an aside, `Updated https://github.com/pycurl/pycurl.git (d92395d9fe1e18365c4e4812f4351bd6985872e1)` on its own does not always mean that we've made a network request. We show that even when checking out an already-available commit locally. But if the user is seeing Wireshark traffic then it does sound like a bug.)

---

_Comment by @andrei-korshikov on 2025-03-17 14:22_

> But if the user is seeing Wireshark traffic then it does sound like a bug.

Just for clarification. `--offline` without Internet access:
```
% uv run --offline --script sync.py
   Updating https://github.com/pycurl/pycurl.git (HEAD)
  × Failed to download and build `pycurl @ git+https://github.com/pycurl/pycurl.git`
  ├─▶ Git operation failed
  ├─▶ failed to fetch into: /home/korshikov/.cache/uv/git-v0/db/9a596e5213c3162d
  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://github.com/pycurl/pycurl.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
      --- stderr
      fatal: unable to access 'https://github.com/pycurl/pycurl.git/': Could not resolve host: github.com
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-17 18:19_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-03-17 18:19_

---

_Comment by @charliermarsh on 2025-03-17 18:19_

I think we can set `GIT_ALLOW_PROTOCOL=file` (https://git-scm.com/docs/git)

---

_Label `good first issue` added by @charliermarsh on 2025-03-17 18:20_

---

_Label `help wanted` added by @charliermarsh on 2025-03-17 18:20_

---

_Comment by @thejchap on 2025-03-18 02:39_

I'll take a look at this one

---

_Referenced in [astral-sh/uv#12619](../../astral-sh/uv/pulls/12619.md) on 2025-04-02 04:49_

---

_Closed by @zanieb on 2025-04-04 16:02_

---

_Closed by @zanieb on 2025-04-04 16:02_

---
