```yaml
number: 12487
title: Specifying macOS platform breaks pip install
type: issue
state: closed
author: itymchyshyn-sc
labels:
  - question
assignees: []
created_at: 2025-03-26T14:16:15Z
updated_at: 2025-03-28T11:49:46Z
url: https://github.com/astral-sh/uv/issues/12487
synced_at: 2026-01-12T16:01:04Z
```

# Specifying macOS platform breaks pip install

---

_@itymchyshyn-sc_

### Summary

When `--python-platform macos` is specified for `uv pip install` or `uv pip compile`, `uv` tries to install package build for macOS 12 even if it is not available and there is a build for macOS 15. This applies to both installation from PyPI and from a local wheel file.

Minimal example:
```
pip download numpy  # for me it downloaded numpy-2.2.4-cp310-cp310-macosx_14_0_arm64.whl
uv pip install --python-platform macos numpy-2.2.4-cp310-cp310-macosx_14_0_arm64.whl  # error
uv pip install numpy-2.2.4-cp310-cp310-macosx_14_0_arm64.whl  # no error
```

### Platform

macOS 15 arm64

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.10.14

---

_Label `bug` added by @itymchyshyn-sc on 2025-03-26 14:16_

---

_Comment by @charliermarsh on 2025-03-26 15:33_

You should set a target macOS version: https://docs.astral.sh/uv/configuration/environment/#macosx_deployment_target. By default, we assume the most recent non-EOL version (12).

---

_Label `bug` removed by @charliermarsh on 2025-03-26 15:33_

---

_Label `question` added by @charliermarsh on 2025-03-26 15:33_

---

_Comment by @itymchyshyn-sc on 2025-03-26 17:08_

@charliermarsh - I see, thanks!

In that case, isn't the macOS 15 the most recent non-EOL version?
Here https://endoflife.date/macos I see that macOS 12 support has ended, so isn't it EOL?
Also, from the security perspective, it seems safer to default to the latest macOS version.
And from the user perspective, it's unexpected that there is a difference between `uv pip install` and `uv pip install --python-platform macos` on Mac.

---

_Comment by @charliermarsh on 2025-03-26 17:25_

Sorry, it’s actually “least-recent non-EOL version” (correct in the docs, but not in my comment). So it should be bumped to macOS 13.

Hmm, I don’t know that changing the default would be more secure, since if anything this means you’re building from source strictly more often?

---

_Comment by @itymchyshyn-sc on 2025-03-26 19:11_

> Hmm, I don’t know that changing the default would be more secure, since if anything this means you’re building from source strictly more often?

Indeed, from a stability standpoint, it's safer not to use build for the newest version.

Another thing is that engineers often need to upgrade to the latest macOS for security reasons. And if they have internal PyPI index in their company, they will be uploading packages built for their latest macOS version to it. This means that `uv pip install --python-platform macos` or `uv pip compile --python-platform macos` will not be able to find that package.

IMO a good solution here would be to switch from "least-recent non-EOL version" to "least-recent non-EOL version s.t. there is a corresponding build for the package". For example, `uv pip install --python-platform macos <package>` could do the following:
```
for mac_version in sorted(non_eol_mac_versions):
    if there is the <package> build for mac_version:
        choose that build
        break
```

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-27 21:49_

---

_Closed by @charliermarsh on 2025-03-28 11:49_

---

_Closed by @charliermarsh on 2025-03-28 11:49_

---
