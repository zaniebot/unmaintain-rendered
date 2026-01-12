```yaml
number: 13564
title: "RFE: Update available managed python versions without requiring updating uv"
type: issue
state: open
author: danra
labels:
  - enhancement
assignees: []
created_at: 2025-05-21T05:16:08Z
updated_at: 2025-05-23T21:43:41Z
url: https://github.com/astral-sh/uv/issues/13564
synced_at: 2026-01-12T16:01:31Z
```

# RFE: Update available managed python versions without requiring updating uv

---

_@danra_

### Summary

After updating my `.python-version` from 3.13.1 to 3.13.3, my CI failed running `uv run` with the error message:

```
error: No interpreter found for Python 3.13.3 in managed installations
```

Which I was able to resolve by logging into the build machine and manually running `uv self update`. (and that's a command I wouldn't want to add to the CI, as it would introduce its own potential for instability).

It would be a nicer UX if modifying the Python version would not be breaking. How about allowing `uv` to update the available list of managed Python versions and be able to install them, separate from updating `uv` itself?

### Example

_No response_

---

_Label `enhancement` added by @danra on 2025-05-21 05:16_

---

_Comment by @konstin on 2025-05-21 08:43_

We hardcode the Python versions into uv so we can check their hashes on download and know they are compatible with the patching we need to perform to make the sys paths when installing python-build-standalone on Unix. Instead, we recommend setting a [`required-version`](https://docs.astral.sh/uv/reference/settings/#required-version) for uv. uv releases are highly compatible, so we recommend updating to the latest uv release.

---

_Renamed from "RFE: Update available managed python versions without requireing updating uv" to "RFE: Update available managed python versions without requiring updating uv" by @danra on 2025-05-23 21:43_

---
