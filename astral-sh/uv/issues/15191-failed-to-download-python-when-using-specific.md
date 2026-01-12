```yaml
number: 15191
title: failed to download python when using specific mirror
type: issue
state: open
author: COVID-9102
labels:
  - question
assignees: []
created_at: 2025-08-09T16:06:51Z
updated_at: 2025-08-11T12:12:51Z
url: https://github.com/astral-sh/uv/issues/15191
synced_at: 2026-01-12T16:02:05Z
```

# failed to download python when using specific mirror

---

_@COVID-9102_

### Question

When I tried to download python from a specific mirror, the url was lengthend with a certain string like "/20250808/cpython-3.13.6%2B20250808-x86_64-pc-windows-msvc-install_only_stripped.tar.gz" ，witch then brought a "404" error.

<img width="966" height="181" alt="Image" src="https://github.com/user-attachments/assets/408f496d-4ac9-4cec-927e-c7eff80d58fc" />

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @COVID-9102 on 2025-08-09 16:06_

---

_Comment by @geofft on 2025-08-11 12:12_

That option is for a mirror for downloads of Python itself, not for a mirror for PyPI (Python packages). The Python downloads are not hosted on PyPI. It looks like that's a PyPI mirror.

You will need to mirror the GitHub releases at https://github.com/astral-sh/python-build-standalone/releases/. Is there a GitHub mirror that you can use? Maybe #14986 has helpful hints for you.


---
