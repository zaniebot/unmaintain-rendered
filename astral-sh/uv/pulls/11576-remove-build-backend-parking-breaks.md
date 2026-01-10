```yaml
number: 11576
title: Remove build backend parking breaks
type: pull_request
state: closed
author: konstin
labels:
  - preview
assignees: []
draft: true
base: main
head: konsti/remove-build-backend-parking-breaks
created_at: 2025-02-17T11:47:04Z
updated_at: 2025-03-10T22:49:27Z
url: https://github.com/astral-sh/uv/pull/11576
synced_at: 2026-01-10T11:10:38Z
```

# Remove build backend parking breaks

---

_Pull request opened by @konstin on 2025-02-17 11:47_

We want to make it easier for users to try the build backend, so we allow using the build backend without requiring `UV_PREVIEW=1`. Instead, we only print a note about the preview status of the build backend. This improves using packages with the uv build backend with other PEP 517 frontends that don't have uv's `--preview` flag.

Closes #11819

---

_Label `preview` added by @konstin on 2025-02-17 11:47_

---

_Comment by @cthoyt on 2025-02-17 14:30_

much appreciated!

---

_@zanieb reviewed on 2025-02-17 14:44_

---

_Review comment by @zanieb on `python/uv/__init__.py`:9 on 2025-02-17 14:44_

Does this preclude us from moving these into a separate namespace later? (as discussed elsewhere)

---

_@konstin reviewed on 2025-02-17 15:10_

---

_Review comment by @konstin on `python/uv/__init__.py`:9 on 2025-02-17 15:10_

Imo they are still preview and as such subject to change at any time, but we can also merge the PR #11478 -> #11446 first and start with `uv_build` directly.

---

_@zanieb reviewed on 2025-02-17 17:19_

---

_Review comment by @zanieb on `python/uv/_build_backend.py`:34 on 2025-02-17 17:19_

It's kind of weird to have this in `warn_config_settings`?

---

_@zanieb approved on 2025-02-17 17:19_

---

_Converted to draft by @konstin on 2025-02-19 11:05_

---

_Comment by @konstin on 2025-03-10 22:49_

This was made unnecessary by the `uv_build` changes.

---

_Closed by @konstin on 2025-03-10 22:49_

---
