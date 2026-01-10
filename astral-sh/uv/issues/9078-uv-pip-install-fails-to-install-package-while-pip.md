---
number: 9078
title: "`uv pip install` fails to install package while `pip install` succeeds"
type: issue
state: closed
author: arpitg-sh
labels:
  - bug
assignees: []
created_at: 2024-11-13T06:26:22Z
updated_at: 2024-11-14T11:05:34Z
url: https://github.com/astral-sh/uv/issues/9078
synced_at: 2026-01-10T01:24:36Z
---

# `uv pip install` fails to install package while `pip install` succeeds

---

_Issue opened by @arpitg-sh on 2024-11-13 06:26_

**Description:** I'm encountering an issue when using uv pip to install Python packages. Specifically, the build process for jsmin==2.2.2 fails, while doing the same with native pip passes.

**Steps to Reproduce**

Using uv:

```
uv pip install setuptools
uv pip install azure-cli
```
This results in a failure:
```
9.923   × Failed to download and build `jsmin==2.2.2`
9.923   ╰─▶ Build backend failed to determine requirements with `build_wheel()`
9.923       (exit status: 1)
9.923
9.923       [stderr]
9.923       /root/.cache/uv/builds-v0/.tmpdkIMOq/lib/python3.9/site-packages/setuptools/_distutils/dist.py:261:        
9.923       UserWarning: Unknown distribution option: 'test_suite'
9.923         warnings.warn(msg)
9.923       error in jsmin setup command: `use_2to3` is invalid.
```
Using regular pip(v24.0):

```
pip install setuptools
pip install azure-cli
```
This works successfully without errors.
Additional Information
```
uv Platform: "Debian GNU/Linux 10 (buster)"
uv Version: uv 0.5.1
```

---

_Comment by @dimbleby on 2024-11-13 08:19_

You will want to allow prereleases when adding azure-cli

---

_Comment by @charliermarsh on 2024-11-13 16:24_

Yeah adding `--pre` seems to get this to work. I think this is correct as-is -- without `--pre`, we end up trying to build a dependency but failing.

---

_Comment by @charliermarsh on 2024-11-13 16:28_

Actually, I think this is failing during pre-fetching, maybe? I have a separate solution to this, so I'll leave it open.

---

_Label `bug` added by @charliermarsh on 2024-11-13 16:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-13 16:28_

---

_Comment by @dimbleby on 2024-11-13 17:54_

It's failing because when pre-releases are disallowed, uv searches way back to old versions of azure-cli that no-one should want. 

I think it's understood (by those who know!) that uv takes a different view of pre-releases than other installers, and different also than the specs. Of course there are reasons, but this is an example of the downsides.

---

_Comment by @charliermarsh on 2024-11-13 17:56_

Yeah -- I generally agree. The thing I noticed in this specific case though is that we don't actually seem to ever consider `jsmin`, we just "pre-fetch" it when looking at certain `azure-cli` versions. So we should be able to delay the error here such that the resolution still succeeds (but would fail, as today, if the resolution proceeded differently and we _did_ end up looking at `jsmin` as a candidate).

---

_Referenced in [astral-sh/uv#9098](../../astral-sh/uv/pulls/9098.md) on 2024-11-13 20:30_

---

_Closed by @charliermarsh on 2024-11-13 20:49_

---

_Closed by @charliermarsh on 2024-11-13 20:49_

---

_Comment by @arpitg-sh on 2024-11-14 11:05_

@charliermarsh Do you know when will this be released? 

---
