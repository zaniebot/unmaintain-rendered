```yaml
number: 13238
title: Python downloads do not retry on stream errors
type: issue
state: closed
author: zanieb
labels:
  - bug
assignees: []
created_at: 2025-04-30T17:16:31Z
updated_at: 2025-07-11T02:16:26Z
url: https://github.com/astral-sh/uv/issues/13238
synced_at: 2026-01-12T16:01:22Z
```

# Python downloads do not retry on stream errors

---

_@zanieb_

e.g.

```
----- stderr -----
error: Failed to install cpython-3.13.3-linux-x86_64-gnu
  Caused by: Failed to download https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.13.3%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: Request failed after 1 retries
  Caused by: error sending request for url (https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.13.3%2B20250409-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz)
  Caused by: client error (SendRequest)
  Caused by: http2 error
  Caused by: stream error received: refused stream before processing any application logic
```

```
error: Failed to install cpython-3.11.11-linux-x86_64-gnu
  Caused by: Failed to extract archive: cpython-3.11.11-20250317-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz
  Caused by: Invalid gzip header
```

Did this regress in #12175? I know the download was tweaked.

---

_Label `bug` added by @zanieb on 2025-04-30 17:18_

---

_Comment by @zanieb on 2025-04-30 17:18_

cc @konstin and @Gankra can ya'll look into this?

---

_Comment by @konstin on 2025-05-02 09:09_

I can't reproduce this locally but #13260 handles the only thing that changed for the existing downloads. 

---

_Label `ci-flake` added by @zanieb on 2025-05-30 21:16_

---

_Comment by @jtfmumm on 2025-06-13 15:30_

Another case: https://github.com/astral-sh/uv/actions/runs/15637900583/job/44057783424?pr=14029

---

_Comment by @konstin on 2025-06-13 15:33_

For context, the problem is that we can't retry once a stream is started, we need to wrap the entire streaming logic into a loop to make this work. I.e., not just the request as we do now as the request gets turned into a stream, as the stream is a lower level object that doesn't have middleware and such anymore.

---

_Label `ci-flake` removed by @zanieb on 2025-06-13 16:51_

---

_Renamed from "Python downloads are flaking" to "Python downloads do not retry on stream errors" by @zanieb on 2025-06-13 16:51_

---

_Comment by @charliermarsh on 2025-07-11 02:16_

I am hoping this is fixed with the changes to remove transparent IO errors.

---

_Closed by @charliermarsh on 2025-07-11 02:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-11 02:16_

---
