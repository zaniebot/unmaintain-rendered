```yaml
number: 13214
title: Installation from explicit azure devops package fails as of 0.7
type: issue
state: closed
author: aersam
labels:
  - bug
assignees: []
created_at: 2025-04-30T07:11:08Z
updated_at: 2025-04-30T08:55:57Z
url: https://github.com/astral-sh/uv/issues/13214
synced_at: 2026-01-12T16:01:22Z
```

# Installation from explicit azure devops package fails as of 0.7

---

_@aersam_

### Summary

As of uv 0.7, this does not work anymore:

```

[[tool.uv.index]]
name = "AzureDevOps"
url = "https://VssSessionToken@pkgs.dev.azure.com/bmeurope/_packaging/BMS/pypi/simple/"
explicit = true

[tool.uv.sources.mysuperpackage]
index = "AzureDevOps"
```

It somehow fails to authenticate:

```
Failed to download `mysuperpackage==0.31.3`
  ├─▶ Failed to extract archive: mysuperpackage-0.31.3-py3-none-any.whl
  ├─▶ an upstream reader returned an error: unexpected end of file
  ╰─▶ unexpected end of file
```

### Platform

Windows 11

### Version

uv 0.7

### Python version

3.11

---

_Label `bug` added by @aersam on 2025-04-30 07:11_

---

_Comment by @l3d00m on 2025-04-30 07:36_

Duplicate of #13208

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-30 08:27_

---

_Comment by @jtfmumm on 2025-04-30 08:55_

Should be addressed for now by #13215. I'm going to close this in favor of #13208 which is tracking the underlying problem.

---

_Closed by @jtfmumm on 2025-04-30 08:55_

---
