---
number: 8895
title: "Big change on `uv sync` timings when \"Prepared X packages in Ys\""
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2024-11-07T19:08:40Z
updated_at: 2024-11-07T20:15:13Z
url: https://github.com/astral-sh/uv/issues/8895
synced_at: 2026-01-07T13:12:18-06:00
---

# Big change on `uv sync` timings when "Prepared X packages in Ys"

---

_Issue opened by @jamesbraza on 2024-11-07 19:08_

With `uv==0.4.30`, there is a big time variance in "Prepared X packages in Ys" in GitHub Actions.

In a Python 3.11 CI run: https://github.com/Future-House/paper-qa/actions/runs/11727536412/job/32668838466?pr=669

```none
> Run uv sync --python-preference=only-managed
...
Resolved 169 packages in 1.26s
Prepared 168 packages in 28.45s
Installed 168 packages in 903ms
...
```

In a Python 3.12 CI run: https://github.com/Future-House/paper-qa/actions/runs/11727536412/job/32668838972?pr=669

```none
> Run uv sync --python-preference=only-managed
...
Resolved 169 packages in 1.52s
Prepared 168 packages in 13m 43s
Installed 168 packages in 334ms
...
```

Note 3.11 takes ~0.5 min and 3.12 takes ~13.5 min.

I think what's happening is Python 3.12 is being installed during the "Prepared", and this takes like 13 mins.

I guess the request is to split out installs of new Python versions into a different timing log line, for more understandability

---

_Comment by @zanieb on 2024-11-07 19:15_

I don't think that the Python install should take that long, that's surprising.

Do you have verbose logs?

I'm not sure if there's a great way for us to improve the timing report if that is the case though, it's challenging implementation-wise.

---

_Comment by @jamesbraza on 2024-11-07 19:51_

Hi Zanie thanks for the quick response. I just did a CI run with `uv sync --verbose`:

- Python 3.11: https://github.com/Future-House/paper-qa/actions/runs/11730068134/job/32677083704
- Python 3.12: https://github.com/Future-House/paper-qa/actions/runs/11730068134/job/32677084030

However, this time it did not happen, both `uv sync` took 32 seconds. So I am not sure what to do here, maybe it was some quirk of GitHub Actions Runner.

Anything we can do here? Or should we just close this issue

---

_Comment by @zanieb on 2024-11-07 20:14_

I don't think we can do anything. Seems like something weird happened.

Fwiw I gave this a quick try and we download the Python version before printing the "Resolved" message so that can't be the cause

```
❯ uv sync -p 3.12.2 -v
DEBUG uv 0.4.30 (c47e2c0f0 2024-11-07)
DEBUG Found project root: `/Users/zb/workspace/example/test/prj`
DEBUG No workspace root found, using project root
DEBUG Using Python request `3.12.2` from explicit request
DEBUG Searching for Python 3.12.2 in managed installations or system path
DEBUG Searching for managed installations at `/Users/zb/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.12.7-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.12.6-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.11.10-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.11.5-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.10.15-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.9.20-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.18-macos-aarch64-none`
DEBUG Skipping incompatible managed installation `cpython-3.8.12-macos-aarch64-none`
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/zb/.local/bin/python3.12` (search path)
DEBUG Skipping interpreter at `/Users/zb/.local/bin/python3.12` from search path: does not satisfy request `3.12.2`
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/opt/homebrew/bin/python3` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.12/bin/python3.12` from search path: does not satisfy request `3.12.2`
DEBUG Found `cpython-3.12.6-macos-aarch64-none` at `/opt/homebrew/bin/python3.12` (search path)
DEBUG Skipping interpreter at `/opt/homebrew/opt/python@3.12/bin/python3.12` from search path: does not satisfy request `3.12.2`
DEBUG Found `cpython-3.9.6-macos-aarch64-none` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/Library/Developer/CommandLineTools/usr/bin/python3` from search path: does not satisfy request `3.12.2`
DEBUG Found `cpython-3.12.7-macos-aarch64-none` at `/Users/zb/.local/bin/python3.12` (search path)
DEBUG Skipping interpreter at `/Users/zb/.local/bin/python3.12` from search path: does not satisfy request `3.12.2`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/Users/zb/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/indygreg/python-build-standalone/releases/download/20240224/cpython-3.12.2%2B20240224-aarch64-apple-darwin-install_only.tar.gz to temporary location: /Users/zb/.local/share/uv/python/.cache/.tmpqaU348
DEBUG Extracting cpython-3.12.2%2B20240224-aarch64-apple-darwin-install_only.tar.gz
DEBUG Moving /Users/zb/.local/share/uv/python/.cache/.tmpqaU348/python to /Users/zb/.local/share/uv/python/cpython-3.12.2-macos-aarch64-none
DEBUG Released lock at `/Users/zb/.local/share/uv/python/.lock`
Using CPython 3.12.2
Creating virtual environment at: .venv
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: prj @ file:///Users/zb/workspace/example/test/prj
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 4 packages in 1ms
```

---

_Closed by @zanieb on 2024-11-07 20:15_

---
