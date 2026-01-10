```yaml
number: 21735
title: Enable PEP 740 attestations when publishing to PyPI
type: pull_request
state: merged
author: woodruffw
labels:
  - release
  - ci
assignees: []
merged: true
base: main
head: ww/pep740
created_at: 2025-12-01T17:21:44Z
updated_at: 2025-12-01T18:15:22Z
url: https://github.com/astral-sh/ruff/pull/21735
synced_at: 2026-01-10T16:48:02Z
```

# Enable PEP 740 attestations when publishing to PyPI

---

_Pull request opened by @woodruffw on 2025-12-01 17:21_

## Summary

This uses our own astral-sh/attest-action to add PEP 740 attestations to our PyPI releases.

See https://github.com/astral-sh/uv/pull/16910 for the equivalent uv CI change 🙂 

## Test Plan

Not easy to test unfortunately, since this is squarely in the release flow. I've separately tested this action and have successfully integrated it on some other projects.

---

_Assigned to @woodruffw by @woodruffw on 2025-12-01 17:21_

---

_Label `release` added by @woodruffw on 2025-12-01 17:21_

---

_Label `ci` added by @woodruffw on 2025-12-01 17:21_

---

_@amyreese approved on 2025-12-01 18:14_

---

_Merged by @woodruffw on 2025-12-01 18:15_

---

_Closed by @woodruffw on 2025-12-01 18:15_

---

_Branch deleted on 2025-12-01 18:15_

---
