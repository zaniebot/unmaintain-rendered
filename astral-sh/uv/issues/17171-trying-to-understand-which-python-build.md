```yaml
number: 17171
title: Trying to understand which python-build-standalone to mirror
type: issue
state: open
author: keestux
labels:
  - question
assignees: []
created_at: 2025-12-18T11:02:23Z
updated_at: 2025-12-26T19:46:19Z
url: https://github.com/astral-sh/uv/issues/17171
synced_at: 2026-01-12T16:02:45Z
```

# Trying to understand which python-build-standalone to mirror

---

_@keestux_

### Question

With `create-python-mirror.py` I can mirror the necessary Python builds for my air-gapped use of `uv`.

What I don't understand is how I can figure out which build dates a certain version of `uv` needs. I did it the clumsy way by installing uv version a.b.c and then do 
```sh
uv -v python install 3.11 |& grep -E '^DEBUG (uv|Downloading'
```
It will produce output like this
```txt
DEBUG uv 0.8.14
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250828/cpython-3.11.13%2B20250828-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
```

So, this uv version needs `20250828`. But where in the `uv` source code can I find that it sets this build date?

Also, there is a problem with the mirror script (or with `crates/uv-python/download-metadata.json` which it is using to decide what to mirror). There is no `20250828` in there anymore. So, uv 0.8.14 cannot be used. I'm trying to understand what rules to put in place for uv versions and mirrored Python builds.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @keestux on 2025-12-18 11:02_

---

_Comment by @zanieb on 2025-12-18 23:48_

Each version of uv can support arbitrary build dates. The build dates are from the `python-build-standalone` repository. You should be able to find them all in the download metadata? We replace older builds with newer ones where we can to reduce the size of this embedded JSON file.

---

_Comment by @MeitarR on 2025-12-26 19:46_

The solution for air-gapped use is to overwrite the versions json using [python-downloads-json-url](https://docs.astral.sh/uv/reference/settings/#python-downloads-json-url)

There is an open PR that adds the generation of the json to this script https://github.com/astral-sh/uv/pull/16475

---
