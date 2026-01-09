---
number: 16669
title: Discover new Python version in airgapped environment
type: issue
state: closed
author: etiennebonnafoux
labels:
  - question
assignees: []
created_at: 2025-11-10T16:32:09Z
updated_at: 2025-12-04T09:49:18Z
url: https://github.com/astral-sh/uv/issues/16669
synced_at: 2026-01-07T13:12:19-06:00
---

# Discover new Python version in airgapped environment

---

_Issue opened by @etiennebonnafoux on 2025-11-10 16:32_

### Question

I work in a air-gapped environment.

When I do `uv python list` I got 

```
cpython-3.14.0rc3-linux-x86_64-gnu                 <download available>
cpython-3.14.0rc3+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.13.7-linux-x86_64-gnu                    <download available>
cpython-3.13.7+freethreaded-linux-x86_64-gnu       <download available>
cpython-3.13.2-linux-x86_64-gnu                    /home/ebonnafoux/.local/bin/python3.13 -> /home/ebonnafoux/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/bin/python3.13
cpython-3.13.2-linux-x86_64-gnu                    /home/ebonnafoux/.local/share/uv/python/cpython-3.13.2-linux-x86_64-gnu/bin/python3.13
cpython-3.12.11-linux-x86_64-gnu                   /usr/bin/python3.12
cpython-3.12.11-linux-x86_64-gnu                   <download available>
cpython-3.11.13-linux-x86_64-gnu                   <download available>
cpython-3.10.18-linux-x86_64-gnu                   <download available>
cpython-3.10.12-linux-x86_64-gnu                   /usr/bin/python3.10
cpython-3.10.12-linux-x86_64-gnu                   /usr/bin/python3 -> python3.10
cpython-3.9.23-linux-x86_64-gnu                    <download available>
cpython-3.8.20-linux-x86_64-gnu                    <download available>
pypy-3.11.13-linux-x86_64-gnu                      <download available>
pypy-3.10.16-linux-x86_64-gnu                      <download available>
pypy-3.9.19-linux-x86_64-gnu                       <download available>
pypy-3.8.16-linux-x86_64-gnu                       <download available>
graalpy-3.12.0-linux-x86_64-gnu                    <download available>
graalpy-3.11.0-linux-x86_64-gnu                    <download available>
graalpy-3.10.0-linux-x86_64-gnu                    <download available>
graalpy-3.8.5-linux-x86_64-gnu                     <download available>
```

When I create a folder named `20250918` with in it `cpython-3.14.0rc3+20250918-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz`  and I do `uv python install --mirror=file:///home/ebonnafoux/ 3.14` , it installs python 3.14 succesfully. 
Now there is a more recent version of python 3.14 so I create a folder named `20251031` with in it `cpython-3.14.0+20251031-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz`.

But how do I make uv discover this new version (without internet acces of course) ?

Thanks in advance for your precision,

### Platform

linux x86_64

### Version

uv 0.8.23

---

_Label `question` added by @etiennebonnafoux on 2025-11-10 16:32_

---

_Comment by @konstin on 2025-11-11 15:43_

uv has an internal list of Python versions in the binary (so we have the hashes shipped with uv). The currently recommended workflow for sharing managed Python with an offline machine is `UV_PYTHON_CACHE_DIR` (https://docs.astral.sh/uv/reference/environment/#uv_python_cache_dir):

* Install the same uv version on an internet-connected and on the airgapped machine.
* Run `UV_PYTHON_CACHE_DIR=python_cache_dir uv python install 3.14 --reinstall` on the online machine. The `--reinstall` is to avoid that the machine already has 3.14 an does nothing.
* Copy `python_cache_dir` over
* Run `UV_PYTHON_CACHE_DIR=python_cache_dir uv python install 3.14 --reinstall` on the offline machine

An alternative is [`UV_PYTHON_INSTALL_MIRROR`](https://docs.astral.sh/uv/reference/environment/#uv_python_install_mirror).

---

_Comment by @etiennebonnafoux on 2025-11-13 21:26_

Thanks for your indication. I will try this, and then close the issue.

---

_Comment by @MeitarR on 2025-11-14 20:05_

@konstin @etiennebonnafoux, actually, for air-gapped networks, I would recommend this approach - https://github.com/astral-sh/uv/issues/10203#issuecomment-2796960833 (using [python-downloads-json-url](https://docs.astral.sh/uv/reference/settings/#python-downloads-json-url)). This way you control the list of available pythons and also are not dependent on the hardcoded URLs

(also linking this issue for more info #12838)

---

_Closed by @etiennebonnafoux on 2025-12-04 09:49_

---
