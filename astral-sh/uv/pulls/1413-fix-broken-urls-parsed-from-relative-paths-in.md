```yaml
number: 1413
title: Fix broken URLs parsed from relative paths in registries
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/relative-html
created_at: 2024-02-16T02:29:11Z
updated_at: 2024-02-16T04:37:10Z
url: https://github.com/astral-sh/uv/pull/1413
synced_at: 2026-01-12T16:04:37Z
```

# Fix broken URLs parsed from relative paths in registries

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/1388

Fixes incorrect handling of relative paths returned by indexes without an explicit `<base>`.

`Url.join` will drop the last segment in an url e.g. `http://foo/bar` -> `http://foo/baz` if there is not a trailing slash but what we want is `http://foo/bar/baz`. We don't add the trailing `/` in `base_url_join_relative` because flat indexes are `http://foo/bar.html` and we _want_ `bar.html` to be replaced.

---

_Renamed from "Add `SimpleHtml` test case for AWS CodeArtifact" to "Add test cases for AWS CodeArtifact simple API responses" by @zanieb on 2024-02-16 02:37_

---

_Renamed from "Add test cases for AWS CodeArtifact simple API responses" to "Fix broken URLs parsed from relative paths in registries" by @zanieb on 2024-02-16 04:24_

---

_Marked ready for review by @zanieb on 2024-02-16 04:24_

---

_@zanieb reviewed on 2024-02-16 04:25_

---

_Review comment by @zanieb on `crates/pypi-types/src/base_url.rs`:48 on 2024-02-16 04:25_

I changed this when I was handling another error kind in here. Think it makes sense to leave it for the future, I prefer enum error wrappers.

---

_@charliermarsh approved on 2024-02-16 04:32_

---

_Merged by @zanieb on 2024-02-16 04:37_

---

_Closed by @zanieb on 2024-02-16 04:37_

---

_Branch deleted on 2024-02-16 04:37_

---
