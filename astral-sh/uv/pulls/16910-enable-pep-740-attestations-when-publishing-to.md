```yaml
number: 16910
title: Enable PEP 740 attestations when publishing to PyPI
type: pull_request
state: merged
author: woodruffw
labels: []
assignees: []
merged: true
base: main
head: ww/uv-publish-attestations
created_at: 2025-12-01T16:48:16Z
updated_at: 2025-12-01T18:01:34Z
url: https://github.com/astral-sh/uv/pull/16910
synced_at: 2026-01-12T16:12:31Z
```

# Enable PEP 740 attestations when publishing to PyPI

---

_@woodruffw_

## Summary

This uses our own `astral-sh/attest-action` to add PEP 740 attestations to our PyPI releases.

## Test Plan

There's no great way to test this, since it lives squarely in the release flow 😞. However, `attest-action` itself has integration tests and we're successfully using it on `astral-sh/sigstore-models`; I've also integrated it into some third-party projects' release workflows.

---

_Assigned to @woodruffw by @woodruffw on 2025-12-01 16:49_

---

_Review requested from @zanieb by @woodruffw on 2025-12-01 16:49_

---

_@zanieb approved on 2025-12-01 16:56_

---

_Merged by @woodruffw on 2025-12-01 18:01_

---

_Closed by @woodruffw on 2025-12-01 18:01_

---

_Branch deleted on 2025-12-01 18:01_

---
