```yaml
number: 19918
title: 0.12.9 does not contain a wheel for Windows
type: issue
state: closed
author: davidbrownell
labels: []
assignees: []
created_at: 2025-08-14T16:18:42Z
updated_at: 2025-08-14T16:25:12Z
url: https://github.com/astral-sh/ruff/issues/19918
synced_at: 2026-01-12T15:54:57Z
```

# 0.12.9 does not contain a wheel for Windows

---

_@davidbrownell_

### Summary

I just tried to sync dependencies for a package using `ruff`. When using 0.12.9, I receive the error below. This seems to be a problem specific to this release, as previous versions work as expected across Linux, MacOS, and Windows.

```
Using CPython 3.13.6
Creating virtual environment at: .venv
Resolved 46 packages in 14ms
error: Distribution `ruff==0.12.9 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `ruff` (v0.12.9) only has wheels for the following platform: `linux_armv6l`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

### Version

0.12.9

---

_Comment by @davidbrownell on 2025-08-14 16:20_

Never mind, this seems to be working now.

---

_Comment by @MichaReiser on 2025-08-14 16:24_

Thanks for reporting. I did a quick check and I do see windows wheels on pypi ([source](https://pypi.org/project/ruff/#files)), which matches your latest comment. You might have been unlucky to install Ruff right in the middle of us publishing the new release (wheels are uploaded one by one). 

I'll close this based on your feedback but please let me know if you keep running into problems.

---

_Closed by @MichaReiser on 2025-08-14 16:24_

---
