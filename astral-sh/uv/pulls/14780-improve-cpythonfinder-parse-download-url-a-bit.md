```yaml
number: 14780
title: "Improve `CPythonFinder._parse_download_url` a bit"
type: pull_request
state: merged
author: j178
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor
created_at: 2025-07-21T06:16:01Z
updated_at: 2025-07-22T04:18:29Z
url: https://github.com/astral-sh/uv/pull/14780
synced_at: 2026-01-12T16:11:24Z
```

# Improve `CPythonFinder._parse_download_url` a bit

---

_@j178_

## Summary

Rename `_parse_download_url` to `_parse_download_asset` and move the `asset['digest']` logic into it.

## Test Plan

```console
uv run ./crates/uv-python/fetch-download-metadata.py
```


---

_@konstin approved on 2025-07-21 10:22_

---

_Merged by @konstin on 2025-07-21 10:22_

---

_Closed by @konstin on 2025-07-21 10:22_

---

_Label `internal` added by @konstin on 2025-07-21 10:23_

---

_Branch deleted on 2025-07-22 04:18_

---
