---
number: 16290
title: "classifiers = [\"Private :: Do Not Upload\"] short-circuits uv publish entirely"
type: issue
state: open
author: heyitsaamir
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-10-14T00:42:18Z
updated_at: 2025-10-20T12:24:36Z
url: https://github.com/astral-sh/uv/issues/16290
synced_at: 2026-01-10T01:26:05Z
---

# classifiers = ["Private :: Do Not Upload"] short-circuits uv publish entirely

---

_Issue opened by @heyitsaamir on 2025-10-14 00:42_

### Summary

The guidance on not publishing private packages is to add `classifiers = ["Private :: Do Not Upload"]` in your pyproject.toml file. However, if you do that in a workspace, and use `uv publish`, we see:

```sh
Caused by: Upload failed with status code 400 Bad Request. Server says: 400 'Private :: Do Not Upload' is not a valid classifier. See https://packaging.python.org/specifications/core-metadata for more information.
```

and the packages that _do_ need publishing are also not published. The expectation is that the packages without this disclaimer should continue on to be published.

### Platform

 Ubuntu   24.04.3   LTS 

### Version

0.9.2

### Python version

3.12.11

---

_Label `bug` added by @heyitsaamir on 2025-10-14 00:42_

---

_Comment by @konstin on 2025-10-20 12:24_

The idea of `classifiers = ["Private :: Do Not Upload"]` is that you can publish it to a private registry, but it prevents accidentally publishing internal code by uploading it to PyPI.

For a mechanism for `uv publish` that keeps some packages in the workspace private and publishing others, we should use different configuration than a classifier.

---

_Label `bug` removed by @konstin on 2025-10-20 12:24_

---

_Label `enhancement` added by @konstin on 2025-10-20 12:24_

---

_Label `needs-design` added by @konstin on 2025-10-20 12:24_

---
