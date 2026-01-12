```yaml
number: 16952
title: "[Draft] Clarify duration environment variable format at parse time"
type: pull_request
state: open
author: anoop-rehman
labels: []
assignees: []
draft: true
base: main
head: clarify-UV_HTTP_TIMEOUT-format
created_at: 2025-12-03T03:53:49Z
updated_at: 2025-12-03T09:17:30Z
url: https://github.com/astral-sh/uv/pull/16952
synced_at: 2026-01-12T16:12:32Z
```

# [Draft] Clarify duration environment variable format at parse time

---

_@anoop-rehman_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
<!-- What's the purpose of the change? What does it do, and why? -->

### Problem
If any of the duration environment variables (``UV_HTTP_TIMEOUT``, ``UV_UPLOAD_HTTP_TIMEOUT``, etc) is set to a value that ends in ``s``, the current error message is misleading about the expected format.

Current error message, before this PR:
<img width="730" height="26" alt="Screen Shot 2025-12-02 at 10 52 10 PM" src="https://github.com/user-attachments/assets/e569932c-211a-4751-9e46-d346b6166726" />

### Proposed Solution
At parse time, emit the following special error message if any of the duration environment variables are set to a value that ends in ``s``: ``expected an integer value in seconds (e.g., 30), not a string with units (e.g., 30s)``

Improved error message, after this PR:
<img width="726" height="39" alt="Screen Shot 2025-12-02 at 10 50 36 PM" src="https://github.com/user-attachments/assets/8337ab87-223a-452e-84e5-e710f2c67537" />

## Test Plan

<!-- How was it tested? -->
1. I built the uv binary with `cargo build --package uv --bin uv`
2. I tested the error message with `UV_HTTP_TIMEOUT=120s ./target/debug/uv pip list`

---

_Renamed from "Clarify UV_HTTP_TIMEOUT (and other duration environment variables) format in error message" to "Emit clearer error message at parse time if duration environment variable includes units" by @anoop-rehman on 2025-12-03 04:20_

---

_Renamed from "Emit clearer error message at parse time if duration environment variable includes units" to "[Draft] Clarify duration environment variable format at parse time" by @anoop-rehman on 2025-12-03 04:24_

---
