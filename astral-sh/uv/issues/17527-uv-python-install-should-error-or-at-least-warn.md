```yaml
number: 17527
title: "'uv python install' should error or at least warn when the UV_NO_MANAGED_PYTHON environment variable is set"
type: issue
state: closed
author: cmelaro
labels:
  - enhancement
assignees: []
created_at: 2026-01-16T16:18:12Z
updated_at: 2026-01-16T16:30:11Z
url: https://github.com/astral-sh/uv/issues/17527
synced_at: 2026-01-16T16:59:50Z
```

# 'uv python install' should error or at least warn when the UV_NO_MANAGED_PYTHON environment variable is set

---

_@cmelaro_

### Summary

Executing 'uv python install' should error out or at least warn when the UV_NO_MANAGED_PYTHON environment variable is set.

With most uv/uvx commands when the UV_NO_MANAGED_PYTHON environment variable is set and the required Python version isn't available you get an error and hint "A managed Python download is available for Python X.XX, but the Python preference is set to 'only system'" however 'uv python install' will install any requested version.  

Suggested behavior that then the environment variable is set that it errors and does not install unless a '--force' flag is provided; minimally should provide a warning even if install is allowed. 

Preventing undesired versions from being installed is important in enterprise environments where security requirements which dictate allowed versions and authorized versions are installed external to uv.

### Example

_No response_

---

_Label `enhancement` added by @cmelaro on 2026-01-16 16:18_

---

_Comment by @cmelaro on 2026-01-16 16:30_

Closing this after finding that "UV_PYTHON_DOWNLOADS=never" achieves desired result.

---

_Closed by @cmelaro on 2026-01-16 16:30_

---
