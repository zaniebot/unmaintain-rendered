```yaml
number: 9769
title: Refactor unavailable metadata to shrink the resolver
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/resolver-refactorings
created_at: 2024-12-10T11:51:56Z
updated_at: 2024-12-10T16:46:57Z
url: https://github.com/astral-sh/uv/pull/9769
synced_at: 2026-01-10T12:00:01Z
```

# Refactor unavailable metadata to shrink the resolver

---

_Pull request opened by @konstin on 2024-12-10 11:51_

The resolver methods are already too large and complex, especially `choose_version*`, so i wanted to shrink and simplify them a bit before adding new methods to them.

I've split `MetadataResponse` into three variants: success, non-fatal error (reported through pubgrub), fatal error (reported as error trace). The resulting non-fatal `MetadataUnavailable` type is equivalent to the `IncompletePackage` type, so they are now merged. (`UnavailableVersion` is a bit different since, besides the extra `IncompatibleDist` variant, it have no error source attached). This shows that the missing metadata variant was unused, which I removed.

Tagging as error messages for the logging format changes.

---

_Label `error messages` added by @konstin on 2024-12-10 11:51_

---

_@charliermarsh approved on 2024-12-10 13:46_

Fine with me as long as you're confident that behavior is unchanged.

---

_Merged by @konstin on 2024-12-10 16:46_

---

_Closed by @konstin on 2024-12-10 16:46_

---

_Branch deleted on 2024-12-10 16:46_

---
