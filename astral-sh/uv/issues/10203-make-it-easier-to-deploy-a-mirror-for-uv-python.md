---
number: 10203
title: Make it easier to deploy a mirror for uv-python command
type: issue
state: closed
author: MeitarR
labels:
  - wish
assignees: []
created_at: 2024-12-27T17:18:48Z
updated_at: 2025-04-11T13:48:56Z
url: https://github.com/astral-sh/uv/issues/10203
synced_at: 2026-01-10T01:24:51Z
---

# Make it easier to deploy a mirror for uv-python command

---

_Issue opened by @MeitarR on 2024-12-27 17:18_

# The problem

Currently, in order to use the feature of `uv` to fetch Python versions (`uv python install`), in internal and air-gapped networks,
one needs to 
1. download all the needed binaries (using https://github.com/astral-sh/uv/blob/main/scripts/create-python-mirror.py )
2. then get the resulting folder (14~ GB for a single `uv` version) to the target network - depending on the organization it may be a complicated stage, that is very hard (or even impossible) to automate
3. and serve it to everyone else in the target network.

a process that may take hours.

The big issue is that the paths of the CPython binaries are hardcoded in the binary and change almost every release of `uv`. Thus, the one who maintains the mirror must go through that process frequently, or else the feature of `uv python install` will _break_ when updating the uv version locally.

(see https://github.com/astral-sh/uv/commits/main/crates/uv-python/download-metadata.json for the frequency)

# Suggested solution

instead of hardcoded URLs, fetch the json externally.

I think that one way of solving this issue is, instead of hardcoding the list of URLs in the binary, you can fetch the list (json) `https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json` from external URL (probably from the repo), letting you update the available python binaries versions without the need to update `uv`, and let the users change the URL the binary uses to fetch the JSON (https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json), so it will work for every version of `uv`, and if you update your list, we can download and update it in the target network later without consequences.

---

_Referenced in [astral-sh/uv#8015](../../astral-sh/uv/issues/8015.md) on 2024-12-27 18:11_

---

_Label `wish` added by @charliermarsh on 2024-12-28 14:48_

---

_Comment by @MeitarR on 2024-12-31 07:19_

@charliermarsh is this PR welcome? (And do you agree with the suggested solution?)

---

_Referenced in [astral-sh/uv#8548](../../astral-sh/uv/pulls/8548.md) on 2025-01-04 10:09_

---

_Referenced in [astral-sh/uv#10939](../../astral-sh/uv/pulls/10939.md) on 2025-01-24 15:49_

---

_Comment by @fcwys on 2025-03-22 03:12_

I also hope to use this method. Currently, when configuring an image using python install mirror, due to the `python-build-standalone` version being updated to 20250317, the image site automatically synchronizes to the latest version. However, when installing Python using UV, the download still fails due to the 20250212 version.

UV Version：uv 0.6.5 (bcbcd0a1e 2025-03-06)

![Image](https://github.com/user-attachments/assets/826de1e2-33f0-4345-8c62-9ad728d0ed51)

---

_Comment by @Crypto-Spartan on 2025-03-25 01:28_

I have the same issue as @fcwys. Internal mirror is slow to get the updated `python-build-standalone` binaries and uv fails to download the latest tag. I work in an ephemeral environment in a corporate network, so i have an install script that reinstalls everything everyday. When uv breaks like this, I straight up can't use it at all.

I would love it if I could tell uv to use an older tag when downloading the python-build-standalone binaries, as sometimes we have the binaries from the _previous week_, but since uv is so picky about which tag it's downloading, it won't work.

---

_Referenced in [astral-sh/uv#12808](../../astral-sh/uv/issues/12808.md) on 2025-04-10 14:09_

---

_Comment by @MeitarR on 2025-04-11 13:44_

After #10939 merged, this is the flow to setup and use the mirror in your network

### Setup
#### on the internet-accessible-computer 
1. **Clone** this repo
2. **Download the Python binaries** by running the [create-python-mirror](https://github.com/astral-sh/uv/blob/main/scripts/create-python-mirror.py) script with the wanted parameters 
3. Transfer the mirror folder and the matching [crates/uv-python/download-metadata.json](https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json) file to your network

#### on your network
1. Put the content of the mirror folder on some server
2. Filter the `download-metadata.json` file only to have the versions you downloaded (I recommend doing it with `jq` command)
3. In the json, replace references to `https://github.com/astral-sh/python-build-standalone/releases/download` with the URL of the server you put the mirror in step 1
4. (suggestion) Also put the final `download-metadata.json` in the same server


### Usage for users
1. fetch the modified `download-metadata.json` and save it in a known location (I recommend `/etc/uv/download-metadata.json`)
2. Set the environment variable  [UV_PYTHON_DOWNLOADS_JSON_URL](https://docs.astral.sh/uv/configuration/environment/#uv_python_downloads_json_url) to the location you saved the json in step 1 (recommand to add it to your .bashrc/.zshrc )
3. Use `uv python` as you would before

---

_Comment by @Gankra on 2025-04-11 13:48_

I'm going to tenatively mark this issue as resolved since you can do the above process, but we'll keep an eye out for whether this can/should be more ergonomic/automatic.

---

_Closed by @Gankra on 2025-04-11 13:48_

---

_Referenced in [astral-sh/uv#12838](../../astral-sh/uv/issues/12838.md) on 2025-04-11 15:21_

---

_Referenced in [astral-sh/uv#16669](../../astral-sh/uv/issues/16669.md) on 2025-11-14 20:05_

---
