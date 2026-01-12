```yaml
number: 2319
title: "Address #2220 (slow download perf against PyPi mirror)"
type: pull_request
state: merged
author: thundergolfer
labels:
  - performance
assignees: []
merged: true
base: main
head: main
created_at: 2024-03-10T00:40:57Z
updated_at: 2024-03-10T01:06:36Z
url: https://github.com/astral-sh/uv/pull/2319
synced_at: 2026-01-12T16:04:58Z
```

# Address #2220 (slow download perf against PyPi mirror)

---

_@thundergolfer_

## Summary

Addressing the extremely slow performance detailed in https://github.com/astral-sh/uv/issues/2220. There are two changes to increase download performance:

1. setting `accept-encoding: identity`, in the spirit of https://github.com/pypa/pip/pull/1688
2. increasing buffer from 8KiB to 128KiB. 

### 1. accept-encoding: identity

I think this related `pip` PR has a good explanation of what's going on: https://github.com/pypa/pip/pull/1688

```
  # We use Accept-Encoding: identity here because requests
  # defaults to accepting compressed responses. This breaks in
  # a variety of ways depending on how the server is configured.
  # - Some servers will notice that the file isn't a compressible
  #   file and will leave the file alone and with an empty
  #   Content-Encoding
  # - Some servers will notice that the file is already
  #   compressed and will leave the file alone and will add a
  #   Content-Encoding: gzip header
  # - Some servers won't notice anything at all and will take
  #   a file that's already been compressed and compress it again
  #   and set the Content-Encoding: gzip header
```

The `files.pythonhosted.org` server is the 1st kind. Example debug log I added in `uv` when installing against PyPI:

<img width="1459" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/ef10d758-46aa-4c8e-9dba-47f33437401b">

(there is no `content-encoding` header in this response, the `whl` hasn't been compressed, and there is a content-length header)

Our internal mirror is the third case. It does seem sensible that our mirror should be modified to act like the 1st kind. But `uv` should handle all three cases like `pip` does.

### 2. buffer increase

In https://github.com/astral-sh/uv/issues/2220 I observed that `pip`'s downloading was causing up-to 128KiB flushes in our mirror. 

After fix 1, `uv` was still only causing up-to 8KiB flushes, and was slower to download than `pip`. Increasing this buffer from the default 8KiB led to a download performance improvement against our mirror and the expected observed 128KiB flushes.

## Test Plan

Ran benchmarking as instructed by @charliermarsh 

<img width="1447" alt="image" src="https://github.com/astral-sh/uv/assets/12058921/840d9c8d-4b98-4bfa-89f3-073a2dec1f23">

No performance improvement or regression. 


---

_Label `performance` added by @charliermarsh on 2024-03-10 00:47_

---

_Merged by @charliermarsh on 2024-03-10 00:49_

---

_Closed by @charliermarsh on 2024-03-10 00:49_

---

_Comment by @charliermarsh on 2024-03-10 00:50_

Thanks! I read through the linked `pip` PR. This makes sense to me.

---

_Comment by @zanieb on 2024-03-10 01:06_

Thank you!

---
