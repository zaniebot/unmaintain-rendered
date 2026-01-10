```yaml
number: 11888
title: Add support for Windows legacy scripts via uv run
type: pull_request
state: merged
author: samypr100
labels:
  - windows
assignees: []
merged: true
base: main
head: windows-legacy-scripts
created_at: 2025-03-02T00:33:27Z
updated_at: 2025-03-10T11:09:06Z
url: https://github.com/astral-sh/uv/pull/11888
synced_at: 2026-01-10T11:10:39Z
```

# Add support for Windows legacy scripts via uv run

---

_Pull request opened by @samypr100 on 2025-03-02 00:33_

## Summary

Closes https://github.com/astral-sh/uv/issues/9151

This adds support for running .ps1, .cmd, .bat legacy scripts typically provided by setuptools [legacy script files](https://packaging.python.org/en/latest/guides/distributing-packages-using-setuptools/#scripts).

Note, .bat and .cmd scripts were somewhat supported previously by [Command](https://doc.rust-lang.org/std/process/index.html#batch-file-special-handling) when the extension was explicit but documentation says such behavior should not be relied upon.

In addition, when no extension is provided and a legacy script exists, it will try to infer the appropriate extension on Windows and use the right runtime with preference for .ps1. Only powershell.exe and cmd.exe are supported right now.

## Test Plan

Added tests. Tested with nuitka locally via uv run.

Note uvx support will be added in a follow up.


---

_Label `windows` added by @samypr100 on 2025-03-02 00:34_

---

_Marked ready for review by @samypr100 on 2025-03-02 00:48_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-02 00:48_

---

_Comment by @charliermarsh on 2025-03-02 00:48_

Nice, thanks.

---

_Assigned to @zanieb by @zanieb on 2025-03-02 15:50_

---

_@zanieb approved on 2025-03-05 20:38_

Thank you!

---

_Comment by @zanieb on 2025-03-05 20:39_

We should probably add some docs to this in the `uv run` reference and / or concept pages? I think that can happen after though.


---

_Merged by @zanieb on 2025-03-05 20:39_

---

_Closed by @zanieb on 2025-03-05 20:39_

---

_Branch deleted on 2025-03-06 00:01_

---

_Comment by @kdeldycke on 2025-03-10 11:09_

I confirm the issue has been fixed in uv 0.6.5, see: https://github.com/Nuitka/Nuitka/issues/3173#issuecomment-2710215547

---
