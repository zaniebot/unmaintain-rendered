```yaml
number: 8732
title: Sanitize filenames during zip extraction
type: pull_request
state: merged
author: charliermarsh
labels:
  - security
assignees: []
merged: true
base: main
head: charlie/sanitize
created_at: 2024-10-31T17:54:39Z
updated_at: 2024-10-31T19:12:52Z
url: https://github.com/astral-sh/uv/pull/8732
synced_at: 2026-01-10T12:08:44Z
```

# Sanitize filenames during zip extraction

---

_Pull request opened by @charliermarsh on 2024-10-31 17:54_

## Summary

Based on the example in `async-zip`: https://github.com/Majored/rs-async-zip/blob/527bda9d58c1ba1fa973a0faeb68dce91fa4ffe4/examples/file_extraction.rs#L33

Closes: https://github.com/astral-sh/uv/issues/8731.

## Test Plan

Created https://github.com/astral-sh/sanitize-wheel-test.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-10-31 17:55_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-31 17:55_

---

_Review requested from @konstin by @charliermarsh on 2024-10-31 17:55_

---

_Label `security` added by @charliermarsh on 2024-10-31 17:55_

---

_Converted to draft by @charliermarsh on 2024-10-31 18:07_

---

_Marked ready for review by @charliermarsh on 2024-10-31 18:42_

---

_Comment by @charliermarsh on 2024-10-31 18:45_

Should be good to go.

---

_Review comment by @BurntSushi on `crates/uv-extract/src/stream.rs`:24 on 2024-10-31 18:49_

Is this comment right that it's only doing sanitization on Windows? I thought it was also doing sanitization on Unix as well?

---

_Review comment by @BurntSushi on `crates/uv-extract/src/stream.rs`:42 on 2024-10-31 18:50_

Do we expect this to be a hot path? I guess it's probably inconsequential compared to the actual task of extraction.

---

_@BurntSushi approved on 2024-10-31 18:50_

---

_Comment by @charliermarsh on 2024-10-31 19:03_

I decided to opt for consistency with the synchronous `zip` crate: we now entirely skip files that aren't _enclosed_.

---

_Merged by @charliermarsh on 2024-10-31 19:12_

---

_Closed by @charliermarsh on 2024-10-31 19:12_

---

_Branch deleted on 2024-10-31 19:12_

---
