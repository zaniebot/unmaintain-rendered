---
number: 10215
title: Error reading requirements.txt
type: issue
state: open
author: ojio-san
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-12-28T18:40:50Z
updated_at: 2024-12-29T00:42:59Z
url: https://github.com/astral-sh/uv/issues/10215
synced_at: 2026-01-10T01:24:51Z
---

# Error reading requirements.txt

---

_Issue opened by @ojio-san on 2024-12-28 18:40_

Hello there,

I wanted to install the requirements for a well known [github project](https://github.com/pymedusa/Medusa/blob/master/requirements.txt), but I got this error :

```
$ uv pip install --break-system-packages --system -r requirements.txt
error: Couldn't parse requirement in `requirements.txt` at position 0
  Caused by: Expected direct URL (`https://codeload.github.com/pymedusa/adba/tar.gz/37b0c74e76b40b3dbde29e71da75a1808eb121de`) to end in a supported file extension: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`, `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
adba @ https://codeload.github.com/pymedusa/adba/tar.gz/37b0c74e76b40b3dbde29e71da75a1808eb121de
       ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

Here are my questions :
a) is there some sort of flag to disable url checking ?
b) if not, I think that's a bug : we should be able to download a GZIP file, without checking the end of the URL for a whitelist (`.whl`, `.tar.gz`, `.zip`, etc)

Also if this is some sort of security, please note that the end of the URL (ie. the extension )means nothing, the server could send us anything.

Thank you :)

Additional informations:
- OS : alpine linux v3.21.0
- python : 3.12.8
- uv : 0.5.13

---

_Label `compatibility` added by @charliermarsh on 2024-12-29 00:40_

---

_Comment by @charliermarsh on 2024-12-29 00:41_

We should fix this but I consider it somewhat low priority.

---

_Label `bug` added by @charliermarsh on 2024-12-29 00:42_

---
