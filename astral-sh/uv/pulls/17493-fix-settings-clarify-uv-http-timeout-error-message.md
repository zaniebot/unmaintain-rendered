```yaml
number: 17493
title: "fix(settings): clarify UV_HTTP_TIMEOUT error message"
type: pull_request
state: open
author: tavaresgmg
labels: []
assignees: []
base: main
head: fix/16940-uv-http-timeout-message
created_at: 2026-01-15T18:43:58Z
updated_at: 2026-01-15T18:48:29Z
url: https://github.com/astral-sh/uv/pull/17493
synced_at: 2026-01-15T18:49:46Z
```

# fix(settings): clarify UV_HTTP_TIMEOUT error message

---

_@tavaresgmg_

This PR improves the error message when `UV_HTTP_TIMEOUT` (and related variables like `UV_REQUEST_TIMEOUT`, `HTTP_TIMEOUT`, `UV_UPLOAD_HTTP_TIMEOUT`) contains an invalid integer.

It adds a context message `"value should be an integer number of seconds"` to the parsing error.

**Fixes #16940**

**Changes:**
- Updated `parse_integer_environment_variable` in `crates/uv-settings/src/lib.rs` to accept an optional help string.
- Updated callsites for timeout variables to provide the context.
- Updated the test `create_venv_with_invalid_http_timeout` in `crates/uv/tests/it/venv.rs` to verify the new message.

**Example Error (Before):**
```
error: Failed to parse environment variable `UV_HTTP_TIMEOUT` with invalid value `foo` : invalid digit found in string
```

**Example Error (After):**
```
error: Failed to parse environment variable `UV_HTTP_TIMEOUT` with invalid value `foo` : invalid digit found in string; value should be an integer number of seconds
```

---
