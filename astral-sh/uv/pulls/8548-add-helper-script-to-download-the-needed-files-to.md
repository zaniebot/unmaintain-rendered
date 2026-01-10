```yaml
number: 8548
title: add helper script to download the needed files to mirror for UV_PYTHON_INSTALL_MIRROR
type: pull_request
state: merged
author: MeitarR
labels:
  - internal
assignees: []
merged: true
base: main
head: add-mirror-script-for-python
created_at: 2024-10-24T22:40:04Z
updated_at: 2025-01-04T10:09:03Z
url: https://github.com/astral-sh/uv/pull/8548
synced_at: 2026-01-10T11:44:29Z
```

# add helper script to download the needed files to mirror for UV_PYTHON_INSTALL_MIRROR

---

_Pull request opened by @MeitarR on 2024-10-24 22:40_

## Summary

I added `crates/uv-python/create-mirror.py` to make it easy to download all the needed files to create a mirror for Python distributions in an offline environment.

the script also has an option to iterate over the git history of the `download-metadata.json` to make sure we have all the files needed for all the uv versions


## Test Plan

```
uv run create-mirror.py --from-all-history --os linux --arch x86_64 --name cpython
2024-10-25 01:31:12,973 - INFO - Starting download of 466 files.
Downloading: 100%|███████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 466/466 [06:11<00:00,  1.26file/s]
Successfully downloaded: 466
now you can run UV_PYTHON_INSTALL_MIRROR='file:///home/meitar/dev/uv/crates/uv-python/mirror' uv python install
```
then checked (the `unshare` command make sure that the process don't have any netwok)
```
UV_PYTHON_INSTALL_MIRROR=file:///home/meitar/dev/uv/crates/uv-python/mirror sudo -E unshare -n /home/meitar/.local/bin/uv python install 3.13
Searching for Python versions matching: Python 3.13
Installed Python 3.13.0 in 2.91s
 + cpython-3.13.0-linux-x86_64-gnu
```


---

_Comment by @zanieb on 2024-10-25 04:04_

Cool! This may be better hosted outside this repository since it's just a Python script. I'll take a closer look though.

Related #8062 

---

_Assigned to @zanieb by @zanieb on 2024-10-25 04:04_

---

_Comment by @MeitarR on 2024-10-25 08:43_

> Cool! This may be better hosted outside this repository since it's just a Python script. I'll take a closer look though.
> 
> Related #8062

Yes, I thought about it, but decided that it would be better to keep it close to the JSON for a couple of reasons:

1. Simplicity – It removes the need to specify the path to the JSON file and the repo containing the full history.
2. Coupling with JSON format – If the format of the JSON changes, having the script nearby ensures we can quickly update it to stay compatible.
3. Discoverability – When someone looks into how to create a mirror, they’ll naturally find this script since it contains relevant strings like `UV_PYTHON_INSTALL_MIRROR` that they'll likely search for.



---

_Comment by @MeitarR on 2024-11-13 21:16_

So, What do you think @zanieb ?

---

_Comment by @MeitarR on 2024-11-26 18:35_

@charliermarsh 

---

_Comment by @zanieb on 2024-11-26 18:42_

After looking more closely, I feel the same way — I don't want to maintain this and think it makes more sense as an external script. I think @charliermarsh is partial to merging it. I won't stand in the way of that, but since we're not testing this I wouldn't be surprised if it becomes outdated.

---

_Comment by @MeitarR on 2024-11-26 19:25_

So we are waiting for @charliermarsh decision? 

---

_Label `internal` added by @charliermarsh on 2024-12-04 01:32_

---

_@charliermarsh approved on 2024-12-04 01:32_

---

_Merged by @charliermarsh on 2024-12-04 01:41_

---

_Closed by @charliermarsh on 2024-12-04 01:41_

---

_Comment by @MeitarR on 2024-12-04 05:56_

Thank you for merging ♥️

---

_Comment by @jeffcarrico on 2025-01-03 04:45_

Thank you for contributing this script, I was about to set out to try to automate exactly this so it saved me a lot of time and worked perfectly.

---

_Comment by @MeitarR on 2025-01-04 10:09_

@jeffcarrico 
this issue might also interest you #10203 

---
