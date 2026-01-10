---
number: 12883
title: "uv-build* release links are broken"
type: issue
state: closed
author: the-connoisseur
labels:
  - bug
assignees: []
created_at: 2025-04-14T17:44:03Z
updated_at: 2025-04-14T22:05:34Z
url: https://github.com/astral-sh/uv/issues/12883
synced_at: 2026-01-10T01:25:26Z
---

# uv-build* release links are broken

---

_Issue opened by @the-connoisseur on 2025-04-14 17:44_

### Summary

The download links for all the uv-build* releases and their checksums lead to "404 Not Found".

eg: https://github.com/astral-sh/uv/releases/download/0.6.14/uv-build-x86_64-unknown-linux-gnu.tar.gz

This seems to be the case for older releases too. Is this intentional?

The dist-manifest.json seems to contain these, and that's causing the downloader to fail when it tries to fetch them at these links.

### Platform

Linux x86_64

### Version

uv 0.6.14

### Python version

Python 3.13.1

---

_Label `bug` added by @the-connoisseur on 2025-04-14 17:44_

---

_Comment by @the-connoisseur on 2025-04-14 17:50_

Additional context:

I'm trying to use uv from rules_python in my Bazel project. This is where it tries to download the URLs in the manifest:
https://github.com/bazel-contrib/rules_python/blob/main/python/uv/private/uv.bzl#L435

---

_Assigned to @Gankra by @Gankra on 2025-04-14 18:24_

---

_Referenced in [astral-sh/uv#12885](../../astral-sh/uv/pulls/12885.md) on 2025-04-14 18:35_

---

_Comment by @Gankra on 2025-04-14 18:37_

Apologies, all the info in the release was auto-generated due to a misconfiguration. No such files were ever built or uploaded, nor should they have been.

The only official distribution of uv-build is [the uv-build pypi package](https://pypi.org/project/uv-build/).

---

_Comment by @Gankra on 2025-04-14 18:48_

(Well, that and uv contains uv-build-as-a-library)

---

_Closed by @Gankra on 2025-04-14 18:49_

---

_Comment by @the-connoisseur on 2025-04-14 22:05_

Thanks for the quick fix!

---

_Referenced in [astral-sh/uv#12927](../../astral-sh/uv/issues/12927.md) on 2025-04-17 12:46_

---
