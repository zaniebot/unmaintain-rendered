```yaml
number: 17543
title: "Update Reqwest to `0.13.1`"
type: pull_request
state: open
author: salmonsd
labels: []
assignees: []
base: main
head: update-reqwest-tls
created_at: 2026-01-16T22:20:41Z
updated_at: 2026-01-20T15:22:38Z
url: https://github.com/astral-sh/uv/pull/17543
synced_at: 2026-01-20T15:44:17Z
```

# Update Reqwest to `0.13.1`

---

_@salmonsd_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR fixes or improves the TLS experience by upgrading reqwest to `0.13.1` via https://github.com/astral-sh/uv/issues/17427

## Test Plan

This was tested locally behind a Corporate CA running `cargo nextest run`.

Dependent PRs (and publishing) required before merging:
1. https://github.com/astral-sh/reqwest-middleware/pull/11
2. https://github.com/astral-sh/ambient-id/pull/21
3. https://github.com/astral-sh/async_http_range_reader/pull/3


---

_Assigned to @zanieb by @zanieb on 2026-01-16 22:21_

---

_Comment by @salmonsd on 2026-01-16 22:34_

Will work on failing tests (apologies)

---

_@salmonsd reviewed on 2026-01-20 15:22_

---

_Review comment by @salmonsd on `crates/uv-client/src/base_client.rs`:602 on 2026-01-20 15:22_

*note*

This adds custom certs to the certificate store _regardless_ of the backend specified, which does diverge from the previous functionality which used native-tls if `--native-tls` flag was passed _OR_ if `SSL_CERT_*` env vars are present. Open to feedback if we want to keep that similar functionality.

---
