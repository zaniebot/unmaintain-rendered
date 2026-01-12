```yaml
number: 16353
title: Improve 403 Forbidden error message to indicate package may not exist
type: pull_request
state: merged
author: aug6th
labels:
  - error messages
assignees: []
merged: true
base: main
head: fix/issue-16340-incorrect-403-error
created_at: 2025-10-18T06:43:15Z
updated_at: 2025-10-20T12:49:25Z
url: https://github.com/astral-sh/uv/pull/16353
synced_at: 2026-01-12T16:12:13Z
```

# Improve 403 Forbidden error message to indicate package may not exist

---

_@aug6th_

Fixes #16340

## Summary

Some package registries (PyTorch, corporate PyPI mirrors) return `403 Forbidden` when a package is not found, instead of `404 Not Found`. The previous error message incorrectly suggested this was always an authentication issue, causing confusion for users.

This PR updates the error hint to clarify that a 403 error could indicate either missing authentication credentials OR that the package doesn't exist on the index.

## Test Plan

- Updated existing snapshot test in `crates/uv/tests/it/edit.rs` to reflect the new error message
- The change is purely a message improvement with no behavioral changes

## Changes

### Before

hint: An index URL (https://example.com/simple) could not be queried due to a lack of valid authentication credentials (403 Forbidden).

### After

hint: An index URL (https://example.com/simple) returned a 403 Forbidden error. This could indicate missing authentication credentials, or the package may not exist on this index.


## Files Changed

- `crates/uv-resolver/src/pubgrub/report.rs` - Updated error message
- `crates/uv/tests/it/edit.rs` - Updated snapshot test expectation

---

_Label `error messages` added by @konstin on 2025-10-20 09:39_

---

_Comment by @konstin on 2025-10-20 09:41_

I've changed the message to be consistent with the 401 message and to indicate that credentials may exist, but they may not be valid. (We may improve this further in a follow-up by checking whether credentials were used for this index)

---

_@konstin approved on 2025-10-20 09:41_

---

_Merged by @konstin on 2025-10-20 11:43_

---

_Closed by @konstin on 2025-10-20 11:43_

---

_Comment by @samypr100 on 2025-10-20 12:48_

On this avenue of change, artifactory has an [option](https://jfrog.com/help/r/jfrog-platform-administration-documentation/hide-existence-of-unauthorized-resources) to return 404 instead of 403 to hide existence.

---
