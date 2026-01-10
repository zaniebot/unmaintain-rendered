```yaml
number: 13406
title: Avoid panics for cannot-be-a-base URLs
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-unwrap-path-segments
created_at: 2025-05-12T11:23:52Z
updated_at: 2025-05-13T02:29:28Z
url: https://github.com/astral-sh/uv/pull/13406
synced_at: 2026-01-10T11:10:41Z
```

# Avoid panics for cannot-be-a-base URLs

---

_Pull request opened by @konstin on 2025-05-12 11:23_

Following #13376, avoid `.unwrap()` on `Url::path_segments()`.

I also added some unwrap-safety comments.

---

_@konstin reviewed on 2025-05-12 11:25_

---

_Review comment by @konstin on `crates/uv-python/src/downloads.rs`:70 on 2025-05-12 11:25_

I'm not sure what caused this in #13376, a data URL maybe? The url crate docs say (`cannot_be_a_base`):

> This is the case if the scheme and `:` delimiter are not followed by a `/` slash,
> as is typically the case of `data:` and `mailto:` URLs.

---

_Comment by @charliermarsh on 2025-05-12 14:06_

Hmm, but we already check `if url.cannot_be_a_base()` on line 26?

---

_Comment by @konstin on 2025-05-12 14:23_

Good point, I switched to unwrap safety comments instead.

---

_Label `bug` added by @konstin on 2025-05-12 14:23_

---

_@charliermarsh approved on 2025-05-13 02:29_

---

_Merged by @charliermarsh on 2025-05-13 02:29_

---

_Closed by @charliermarsh on 2025-05-13 02:29_

---

_Branch deleted on 2025-05-13 02:29_

---
