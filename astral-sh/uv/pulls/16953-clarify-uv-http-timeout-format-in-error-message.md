---
number: 16953
title: Clarify UV_HTTP_TIMEOUT format in error message for streaming timeout
type: pull_request
state: open
author: anoop-rehman
labels: []
assignees: []
base: main
head: implement-item-1
created_at: 2025-12-03T04:03:39Z
updated_at: 2025-12-04T18:31:51Z
url: https://github.com/astral-sh/uv/pull/16953
synced_at: 2026-01-10T01:26:18Z
---

# Clarify UV_HTTP_TIMEOUT format in error message for streaming timeout

---

_Pull request opened by @anoop-rehman on 2025-12-03 04:03_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
<!-- What's the purpose of the change? What does it do, and why? -->

Relevant issues: #16940 

### Problem
The error message for network streaming timeouts is misleading about the expected format:
```
Failed to download distribution due to network timeout.
Try increasing UV_HTTP_TIMEOUT (current value: 30s).
```
This suggests users should set values like ``120s`` or ``180s``, but the actual required format is a plain integer representing seconds.

### Solution
This PR updates the hint to be clearer about the input format, changing it from ``Try increasing UV_HTTP_TIMEOUT (current value: {}s).`` to ``Try increasing UV_HTTP_TIMEOUT to a larger integer value (in seconds), e.g., UV_HTTP_TIMEOUT=60``

---

_Renamed from "Clarify UV_HTTP_TIMEOUT format in error message" to "Clarify UV_HTTP_TIMEOUT format in error message for streaming timeout" by @anoop-rehman on 2025-12-03 04:05_

---

_Marked ready for review by @anoop-rehman on 2025-12-03 04:15_

---

_Referenced in [astral-sh/uv#16940](../../astral-sh/uv/issues/16940.md) on 2025-12-03 04:31_

---

_@zanieb approved on 2025-12-04 15:36_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:1210 on 2025-12-04 15:38_

Sorry I realized this wrong after I approved :D

Two things

1. I don't think we need to be so verbose about "integer value (in seconds)" if we show a concrete example
2. We need to actually make sure the new value is larger than the current one! We should hint `self.timeout().as_secs() * 2` instead of hard-coding 60

```suggestion
                    "Failed to download distribution due to network timeout ({}s).\nTry increasing UV_HTTP_TIMEOUT to a larger value, e.g., UV_HTTP_TIMEOUT={}",
```

---

_@zanieb reviewed on 2025-12-04 15:38_

---

_@zanieb reviewed on 2025-12-04 15:38_

---

_Review comment by @zanieb on `crates/uv-client/src/registry_client.rs`:1210 on 2025-12-04 15:38_

As a third thing, we should use backticks, e.g., `UV_HTTP_TIMEOUT` and `UV_HTTP_TIMEOUT=60`

---

_@anoop-rehman reviewed on 2025-12-04 18:31_

---

_Review comment by @anoop-rehman on `crates/uv-client/src/registry_client.rs`:1210 on 2025-12-04 18:31_

Thank you sm for taking the time to review this! Agreed with all 3 points and applied these changes.

---
