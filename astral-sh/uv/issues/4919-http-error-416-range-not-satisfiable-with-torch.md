```yaml
number: 4919
title: HTTP error 416 Range Not Satisfiable with torch index
type: issue
state: closed
author: konstin
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-07-09T09:19:22Z
updated_at: 2024-07-09T18:38:39Z
url: https://github.com/astral-sh/uv/issues/4919
synced_at: 2026-01-12T15:58:52Z
```

# HTTP error 416 Range Not Satisfiable with torch index

---

_@konstin_

Trying to use the torch index at https://download.pytorch.org/whl/nightly/cu121 fails with HTTP error 416 Range Not Satisfiable:

```
$ uv pip install -U --prerelease allow torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu121 --no-cache-dir
error: Failed to download `torch==2.5.0.dev20240708+cu121`
  Caused by: Failed to unzip wheel: torch-2.5.0.dev20240708+cu121-cp312-cp312-linux_x86_64.whl
  Caused by: an upstream reader returned an error: io error occurred: HTTP status client error (416 Range Not Satisfiable) for url (https://download.pytorch.org/whl/nightly/cu121/torch-2.5.0.dev20240708%2Bcu121-cp312-cp312-linux_x86_64.whl)
  Caused by: io error occurred: HTTP status client error (416 Range Not Satisfiable) for url (https://download.pytorch.org/whl/nightly/cu121/torch-2.5.0.dev20240708%2Bcu121-cp312-cp312-linux_x86_64.whl)
  Caused by: HTTP status client error (416 Range Not Satisfiable) for url (https://download.pytorch.org/whl/nightly/cu121/torch-2.5.0.dev20240708%2Bcu121-cp312-cp312-linux_x86_64.whl)
```

This works with pip

---

_Label `bug` added by @konstin on 2024-07-09 09:19_

---

_Renamed from "HTTP error 416 Range Not Satisfiable with torch error" to "HTTP error 416 Range Not Satisfiable with torch index" by @konstin on 2024-07-09 09:19_

---

_Comment by @charliermarsh on 2024-07-09 16:23_

We may need to add fallback for this somewhere.

---

_Label `compatibility` added by @charliermarsh on 2024-07-09 16:23_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-09 16:28_

---

_Comment by @charliermarsh on 2024-07-09 17:41_

You hit this consistently or once?

---

_Comment by @konstin on 2024-07-09 18:38_

This morning it was consistent, now it's gone. Looks like a server misconfiguration.

---

_Closed by @konstin on 2024-07-09 18:38_

---
