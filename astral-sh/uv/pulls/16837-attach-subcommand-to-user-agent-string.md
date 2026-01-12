```yaml
number: 16837
title: Attach subcommand to User-Agent string
type: pull_request
state: merged
author: zsol
labels:
  - registry
assignees: []
merged: true
base: main
head: zsol/jj-knlrolpvxukt
created_at: 2025-11-24T17:08:38Z
updated_at: 2025-12-01T15:29:56Z
url: https://github.com/astral-sh/uv/pull/16837
synced_at: 2026-01-12T16:12:28Z
```

# Attach subcommand to User-Agent string

---

_@zsol_

## Summary

This PR makes uv log an additional field in the user agent (parsed by https://github.com/pypi/linehaul-cloud-function/pull/237) which captures the subcommand that caused the http requests.

Follow-up to #16702.

## Test Plan

Unit tests. Whether the various commands are wired up correctly has been spot-tested manually.

---

_Label `registry` added by @zsol on 2025-11-24 17:09_

---

_Marked ready for review by @zsol on 2025-11-28 14:52_

---

_Review requested from @konstin by @zsol on 2025-11-28 14:52_

---

_@konstin approved on 2025-11-28 15:55_

Let's wait with merging until https://github.com/pypi/linehaul-cloud-function/pull/237 shipped.

---

_Renamed from "linehaul: log subcommand" to "Attach subcommand to User-Agent string" by @zsol on 2025-12-01 15:29_

---

_Merged by @zsol on 2025-12-01 15:29_

---

_Closed by @zsol on 2025-12-01 15:29_

---

_Branch deleted on 2025-12-01 15:29_

---
