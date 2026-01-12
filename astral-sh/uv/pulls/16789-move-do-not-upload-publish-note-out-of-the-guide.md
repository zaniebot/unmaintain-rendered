```yaml
number: 16789
title: Move do not upload publish note out of the guide into concepts
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/publish-note
created_at: 2025-11-20T14:56:35Z
updated_at: 2025-11-20T20:10:43Z
url: https://github.com/astral-sh/uv/pull/16789
synced_at: 2026-01-12T16:12:26Z
```

# Move do not upload publish note out of the guide into concepts

---

_@zanieb_

This feels a little out of place here and it seems nice to be able to link to it.

---

_Label `documentation` added by @zanieb on 2025-11-20 14:56_

---

_Review requested from @woodruffw by @zanieb on 2025-11-20 17:28_

---

_@woodruffw approved on 2025-11-20 17:55_

Nice, makes sense to me!

---

_@woodruffw reviewed on 2025-11-20 17:57_

---

_Review comment by @woodruffw on `docs/concepts/projects/build.md`:61 on 2025-11-20 17:57_

This got me thinking: apparently the `Do Not Upload` classifier actually isn't really specified anywhere (PyPUG mentions PyPI's behavior, but doesn't say anything about other indices). It's kind of unfortunate that we can't say that all indices should honor this, but oh well.

---

_Merged by @zanieb on 2025-11-20 18:33_

---

_Closed by @zanieb on 2025-11-20 18:33_

---

_Branch deleted on 2025-11-20 18:33_

---

_Review comment by @konstin on `docs/concepts/projects/build.md`:61 on 2025-11-20 19:18_

My understanding was that that's specifically for PyPI, so a `uv publish` doesn't accidentally land your internal code on PyPI, while an internal - private - index is fine.

---

_@konstin reviewed on 2025-11-20 19:18_

---

_@konstin reviewed on 2025-11-20 19:19_

---

_Review comment by @konstin on `docs/concepts/projects/build.md`:61 on 2025-11-20 19:19_

(We had longer convos about this when introducing `uv publish`, the current options are all bad in their own way)

---

_@woodruffw reviewed on 2025-11-20 20:10_

---

_Review comment by @woodruffw on `docs/concepts/projects/build.md`:61 on 2025-11-20 20:10_

> My understanding was that that's specifically for PyPI, so a `uv publish` doesn't accidentally land your internal code on PyPI, while an internal - private - index is fine.

Could be -- in that case we might be a little too conservative on pyx 😅 

https://docs.pyx.dev/publishing#private-classifiers

---
