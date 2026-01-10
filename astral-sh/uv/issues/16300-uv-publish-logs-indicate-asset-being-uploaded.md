---
number: 16300
title: uv publish logs indicate asset being uploaded even though it is being correctly skipped
type: issue
state: open
author: wevonosky
labels:
  - bug
assignees: []
created_at: 2025-10-14T18:03:35Z
updated_at: 2025-10-17T09:57:02Z
url: https://github.com/astral-sh/uv/issues/16300
synced_at: 2026-01-10T01:26:05Z
---

# uv publish logs indicate asset being uploaded even though it is being correctly skipped

---

_Issue opened by @wevonosky on 2025-10-14 18:03_

### Summary

In a workspace if I build multiple packages like:

```
uv build --package lib_a
uv build --package lib_b
```

and then publish those packages (in my case to AWS Codeartifact):

```
uv publish --index myindex --username __token__
```

the logged output on initial publish looks something like this:

```
Publishing 4 files https://domain-123456789012.d.codeartifact.us-west-2.amazonaws.com/pypi/myindex/
Uploading lib_a-1.0.0-py3-none-any.whl (2.4KiB)
Uploading lib_a-1.0.0.tar.gz (3.9KiB)
Uploading lib_b-1.0.0-py3-none-any.whl (33.9KiB)
Uploading lib_b-1.0.0.tar.gz (41.8KiB)
```

If I then run the same publish command without rebuilding, the logged output looks like this:

```
Publishing 4 files https://domain-123456789012.d.codeartifact.us-west-2.amazonaws.com/pypi/myindex/
Uploading lib_a-1.0.0-py3-none-any.whl (2.4KiB)
File lib_a-1.0.0.tar.gz already exists, skipping
File lib_b-1.0.0-py3-none-any.whl already exists, skipping
File lib_b-1.0.0.tar.gz already exists, skipping
```

Every time I run the publish command, the first log says it is uploading the `lib_a-1.0.0-py3-none-any.whl` asset, however, in CodeArtifact the index doesn't show the package being updated which is the expected behavior. To verify this seems like a bug in uv,  I removed the `lib_a` assets from dist and reran the publish command with this output:

```
Publishing 2 files https://domain-123456789012.d.codeartifact.us-west-2.amazonaws.com/pypi/myindex/
Uploading lib_b-1.0.0-py3-none-any.whl (33.9KiB)
File lib_b-1.0.0.tar.gz already exists, skipping
```

The logs say the `lib_b-1.0.0-py3-none-any.whl` asset is being uploaded even though in previous logs it was correctly skipped. 

### Platform

Ubuntu 22.04.5 LTS

### Version

uv 0.8.13

### Python version

Python 3.13.7

---

_Label `bug` added by @wevonosky on 2025-10-14 18:03_

---

_Renamed from "uv publsh logs indicate asset being uploaded even though its being skipped" to "uv publsh logs indicate asset being uploaded even though its being correctly skipped" by @wevonosky on 2025-10-14 18:24_

---

_Renamed from "uv publsh logs indicate asset being uploaded even though its being correctly skipped" to "uv publish logs indicate asset being uploaded even though it is being correctly skipped" by @wevonosky on 2025-10-14 18:28_

---

_Comment by @konstin on 2025-10-17 09:57_

Can you share verbose logs (`-v`)?

---
