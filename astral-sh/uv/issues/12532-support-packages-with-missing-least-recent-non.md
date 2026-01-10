---
number: 12532
title: Support packages with missing least-recent non-EOL macOS build on PyPI
type: issue
state: open
author: itymchyshyn-sc
labels:
  - enhancement
assignees: []
created_at: 2025-03-28T14:40:18Z
updated_at: 2025-03-28T15:01:11Z
url: https://github.com/astral-sh/uv/issues/12532
synced_at: 2026-01-10T01:25:21Z
---

# Support packages with missing least-recent non-EOL macOS build on PyPI

---

_Issue opened by @itymchyshyn-sc on 2025-03-28 14:40_

### Summary

**Background**

Right now `uv pip install --python-platform macos` or `uv pip compile --python-platform macos` are searching for packages built for least-recent non-EOL macOS version. It means that if there is no such build on PyPI, `uv` will raise an error even if there are builds for higher macOS versions available.

**Proposal**

Switch from "least-recent non-EOL version" to "least-recent non-EOL version s.t. there is a corresponding build for the package". For example, `uv pip install --python-platform macos <package>` could do the following:

```
for mac_version in sorted(non_eol_mac_versions):
    if there is the <package> build for mac_version:
        choose that build
        break
```

### Example

This will likely impact experience in companies, where internal PyPI index is configured. Engineers can upload internal packages to the internal PyPI and then `uv pip install` them. Considering that engineers are likely to work on the latest macOS version due to security reasons, they will be uploading packages for the latest macOS version to PyPI.

Without this feature, `uv pip install --python-platform macos` will not be able to find such packages on PyPI.
With this feature, `uv pip install --python-platform macos` will be able to do that.

---

_Label `enhancement` added by @itymchyshyn-sc on 2025-03-28 14:40_

---

_Comment by @charliermarsh on 2025-03-28 14:42_

The proposal here is to effectively "solve" for the supported macOS version, but I don't know if that's a good idea? I think you'd be better off advocating that we change the default to "most recent macOS version".

---

_Comment by @itymchyshyn-sc on 2025-03-28 14:55_

Yes, it would be even better if "most recent macOS version" were used. I would be happy with any solution 🙂 

---

_Comment by @itymchyshyn-sc on 2025-03-28 15:01_

> change the default to "most recent macOS version"

@charliermarsh - Should I open a feature request for that and close this one?

---
