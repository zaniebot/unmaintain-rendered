```yaml
number: 16944
title: Disable PEP 740 attestations for PyPI publishing
type: pull_request
state: merged
author: woodruffw
labels:
  - internal
assignees: []
merged: true
base: main
head: ww/disable-740
created_at: 2025-12-03T00:35:43Z
updated_at: 2025-12-03T01:11:09Z
url: https://github.com/astral-sh/uv/pull/16944
synced_at: 2026-01-12T16:12:31Z
```

# Disable PEP 740 attestations for PyPI publishing

---

_@woodruffw_

## Summary

This broke the release and I haven't figured out why yet.

## Test Plan

Blame my past self.

---

_Review requested from @zanieb by @woodruffw on 2025-12-03 00:35_

---

_Assigned to @woodruffw by @woodruffw on 2025-12-03 00:35_

---

_Label `internal` added by @woodruffw on 2025-12-03 00:42_

---

_Comment by @woodruffw on 2025-12-03 00:44_

Triage: this is a reusable workflows thing, namely https://github.com/pypi/warehouse/issues/11096 (hello past me).

Specifically, what's happening here is that the attestation is from `release.yml @ astral-sh/uv`, while the publisher identity on PyPI (which it needs to match) is `publish-pypi.yml @ astral-sh/uv`. This happens because `release.yml` is the calling workflow, while `publish-pypi.yml` is the callee.

Relevant tlog: https://search.sigstore.dev/?logIndex=737467510

Actual error:

```
DEBUG Response code for https://upload.pypi.org/legacy/: 400 Bad Request
DEBUG Upload error response: {"message": "The server could not comply with the request since it is either malformed or otherwise incorrect.\n\n\nInvalid attestations supplied during upload: Could not verify the uploaded artifact using the included attestation: Verification failed: Certificate's Build Config URI (<Extension(oid=<ObjectIdentifier(oid=1.3.6.1.4.1.57264.1.18, name=Unknown OID)>, critical=False, value=<UnrecognizedExtension(oid=<ObjectIdentifier(oid=1.3.6.1.4.1.57264.1.18, name=Unknown OID)>, value=b'\\x0cMhttps://github.com/astral-sh/uv/.github/workflows/release.yml@refs/heads/main')>)>) does not match expected Trusted Publisher (publish-pypi.yml @ astral-sh/uv)\n\n", "code": "400 Invalid attestations supplied during upload: Could not verify the uploaded artifact using the included attestation: Verification failed: Certificate's Build Config URI (<Extension(oid=<ObjectIdentifier(oid=1.3.6.1.4.1.57264.1.18, name=Unknown OID)>, critical=False, value=<UnrecognizedExtension(oid=<ObjectIdentifier(oid=1.3.6.1.4.1.57264.1.18, name=Unknown OID)>, value=b'\\x0cMhttps://github.com/astral-sh/uv/.github/workflows/release.yml@refs/heads/main')>)>) does not match expected Trusted Publisher (publish-pypi.yml @ astral-sh/uv)", "title": "Bad Request"}
error: Failed to publish `wheels_uv_build/uv_build-0.9.15-py3-none-manylinux_2_17_i686.manylinux2014_i686.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 Invalid attestations supplied during upload: Could not verify the uploaded artifact using the included attestation: Verification failed: Certificate's Build Config URI (<Extension(oid=<ObjectIdentifier(oid=1.3.6.1.4.1.57264.1.18, name=Unknown OID)>, critical=False, value=<UnrecognizedExtension(oid=<ObjectIdentifier(oid=1.3.6.1.4.1.57264.1.18, name=Unknown OID)>, value=b'\x0cMhttps://github.com/astral-sh/uv/.github/workflows/release.yml@refs/heads/main')>)>) does not match expected Trusted Publisher (publish-pypi.yml @ astral-sh/uv)
Error: Process completed with exit code 2.
```

---

_Merged by @zanieb on 2025-12-03 00:50_

---

_Closed by @zanieb on 2025-12-03 00:50_

---

_Branch deleted on 2025-12-03 00:50_

---
