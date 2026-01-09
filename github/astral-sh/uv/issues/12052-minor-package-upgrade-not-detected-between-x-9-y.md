---
number: 12052
title: Minor package upgrade not detected between x.9.y and x.10.z+
type: issue
state: closed
author: lgatellier
labels:
  - bug
assignees: []
created_at: 2025-03-07T16:50:16Z
updated_at: 2025-03-10T11:42:34Z
url: https://github.com/astral-sh/uv/issues/12052
synced_at: 2026-01-07T13:12:18-06:00
---

# Minor package upgrade not detected between x.9.y and x.10.z+

---

_Issue opened by @lgatellier on 2025-03-07 16:50_

### Summary

Hi,

First of all, thanks a lot for developing this killer package manager, it's really amazing !

## Context
I want to upgrade a tool installed with uv.
This tool has been published with uv publish on my self-hosted GitLab pypi package registry.

My current installed version is `1.9.1`, and I want uv to install latest `1.10.0` version.

## Actual behaviour
uv finds nothing to upgrade.
```bash
$ mytool --version
1.9.1
$ uv tool update mytool
Nothing to upgrade
$ mytool --version
1.9.1
```

## Expected behaviour
uv finds and installs the latest `1.10.0` version.

## Additional info
I also ran the following tests :
- Install pypi.org-hosted package `python-gitlab<4.10`, uv installs 4.9..0. I try to upgrade to '<4.11', uv does not update the package to 4.10.0.
- Install pypi.org-hosted package `python-gitlab<4.11`, uv installs 4.10.0. I try to upgrade to '<4.13', uv upgrades python-gitlab to 4.12.2.

IMHO : The version comparison between installed and existing ones seems to confuse when comparing a one-digit version number with a two-digits one.

### Platform

Windows 11 WSL2 Linux 5.10.16.3-microsoft-standard-WSL2 Ubuntu 24.04.1 LTS x86_64

### Version

0.6.5

### Python version

cpython-3.12.3-linux-x86_64-gnu

---

_Label `bug` added by @lgatellier on 2025-03-07 16:50_

---

_Closed by @lgatellier on 2025-03-07 16:54_

---

_Comment by @charliermarsh on 2025-03-07 16:54_

Hmm... So, when you run `uv tool upgrade`, we respect the constraints you provided when you first installed the tool. Like, if you ran `uv tool install mytool<1.10` when you first installed `mytool`, then `uv tool upgrade mytool` would _never_ upgrade beyond `<1.10`.

---

_Comment by @charliermarsh on 2025-03-07 16:54_

If you want to change the installed constraints, you'd typically need to run `uv tool install mytool<=1.10`, for example.

---

_Comment by @zanieb on 2025-03-07 17:19_

We should be able to warn on this case, right?

---

_Comment by @zanieb on 2025-03-07 17:20_

This is discussed at https://docs.astral.sh/uv/concepts/tools/#mutating-tool-environments — we should add a better heading

---

_Comment by @lgatellier on 2025-03-10 11:41_

Hi @charliermarsh  and @zanieb,

Thanks for your answers. I was trying to upgrade `mytool` (without version constraint) from installed `1.9.1` to freshly released `1.10.0` and ran several unsuccessful tests, so I opened this issue, but it started working like 5 minutes after I opened the issue.

I've just ran some new tests (on the same computer and on a different one), and it works perfectly :
1. Install `mytool==1.9.1`
2. Then install `mytool<1.12` (no-op)
3. Then, upgrade tool `mytool`, and uv installs latest `1.11.0` version.

Seems like a cache-related issue (or a Friday-evening-related issue 😅 ?), then (i ran some uv cache clean as a last resort on friday), but cannot reproduce, so I think we can forget about it. Sorry for the inconvenience 😓 .

---
