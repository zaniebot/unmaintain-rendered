```yaml
number: 11074
title: "Propagate credentials for `<index>/simple` to `<index>/...` endpoints"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/files-index
created_at: 2025-01-29T17:52:31Z
updated_at: 2025-02-10T16:44:35Z
url: https://github.com/astral-sh/uv/pull/11074
synced_at: 2026-01-10T11:10:34Z
```

# Propagate credentials for `<index>/simple` to `<index>/...` endpoints

---

_Pull request opened by @zanieb on 2025-01-29 17:52_

Closes https://github.com/astral-sh/uv/issues/11017
Closes https://github.com/astral-sh/uv/issues/8565

Sort of an minimal implementation of https://github.com/astral-sh/uv/issues/4583

---

_Label `bug` added by @zanieb on 2025-01-29 17:52_

---

_@zanieb reviewed on 2025-01-29 17:54_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:172 on 2025-01-29 17:54_

Alternatively, we could just push credentials for the URL with `/simple` stripped and that would cover all imaginable cases?

---

_Comment by @zanieb on 2025-01-29 17:57_

Needs a test case, but I want to make sure we're aligned this approach is reasonable.

---

_Renamed from "Pre-emptively store credentials for `/files` endpoints in `/simple` index URLs" to "Propagate credentials for `<index>/simple` to `<index>/...` endpoints" by @zanieb on 2025-01-29 19:07_

---

_Marked ready for review by @zanieb on 2025-01-29 21:52_

---

_@zanieb reviewed on 2025-01-29 21:53_

---

_Review comment by @zanieb on `crates/uv/src/commands/build_frontend.rs`:503 on 2025-01-29 21:53_

We could omit `raw_url()` in favor of `root_url()` but I think this is "safer"

---

_@charliermarsh approved on 2025-01-30 01:04_

---

_Merged by @zanieb on 2025-01-30 16:22_

---

_Closed by @zanieb on 2025-01-30 16:22_

---

_Branch deleted on 2025-01-30 16:22_

---

_Comment by @jzazo on 2025-02-10 16:19_

Hi! It says at the beginning it's sort of minimal implementation for [#4583](https://github.com/astral-sh/uv/issues/4583). We have latest uv version and we can still not install packages with 401 authentication errors, problem being that package url's are queried. Should this PR fix that issue, or still unsolved? Thank you.

---

_Comment by @zanieb on 2025-02-10 16:20_

@jzazo Do you have a trailing `/` on your URL? https://github.com/astral-sh/uv/pull/11336 is not released yet

---

_Comment by @jzazo on 2025-02-10 16:26_

uv tries with a URL where there is a trailing `/`, ending in `.../_packaging/MI/pypi/simple/`. But I think uv did not try that URL but this one instead: `.../_packaging/MI/pypi/simple/poetry-core/` (not sure why trying to locate poetry, might be a relic).

---

_Comment by @zanieb on 2025-02-10 16:43_

If you remove the trailing `/` from your index URL config, it should work.

---

_Comment by @zanieb on 2025-02-10 16:44_

Though it doesn't really make sense that this would fix a request to `.../_packaging/MI/pypi/simple/poetry-core/` since it's not at `.../simple/../...` like `..../files/`.

It'd probably be better to just open an issue.. way easier to tell what's going on with all the details.

---
