```yaml
number: 14702
title: Github action setup-uv fails with uvicorn project package dependency
type: issue
state: closed
author: vpmedia
labels:
  - bug
assignees: []
created_at: 2025-07-18T06:12:37Z
updated_at: 2025-07-18T06:59:22Z
url: https://github.com/astral-sh/uv/issues/14702
synced_at: 2026-01-12T16:01:54Z
```

# Github action setup-uv fails with uvicorn project package dependency

---

_@vpmedia_

### Summary

My GitHub action started to fail after the v0.8.0 uv release, with the following message:

```
Trying to find version for uv in: /home/runner/work/PROJECT_NAME/PROJECT_NAME/pyproject.toml
Found version for uv in /home/runner/work/PROJECT_NAME/PROJECT_NAME/pyproject.toml: icorn>=0.34.2
Error: No version found for icorn>=0.34.2
```

I have a package dependency defined in my pyproject.uml for the `uvicorn` server
`"uvicorn>=0.34.2"`

I think some magic must happen and the `astral-sh/setup-uv@v6` thinks `**uv**icorn` is a version definition for `uv`



### Platform

Ubuntu 20.X

### Version

uv 0.8.0

### Python version

Python 3.10

---

_Label `bug` added by @vpmedia on 2025-07-18 06:12_

---

_Comment by @konstin on 2025-07-18 06:49_

Fixed by https://github.com/astral-sh/setup-uv/pull/492

---

_Closed by @konstin on 2025-07-18 06:49_

---

_Comment by @vpmedia on 2025-07-18 06:59_

@konstin Thanks!

---
