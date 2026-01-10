```yaml
number: 16234
title: "publish: don't infer check URLs for pyx uploads"
type: pull_request
state: merged
author: woodruffw
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ww/pyx-no-check-infer
created_at: 2025-10-10T15:55:29Z
updated_at: 2025-10-13T10:15:59Z
url: https://github.com/astral-sh/uv/pull/16234
synced_at: 2026-01-10T06:36:15Z
```

# publish: don't infer check URLs for pyx uploads

---

_Pull request opened by @woodruffw on 2025-10-10 15:55_

I think I've got this right, but could use a sanity check from @charliermarsh and @konstin 🙂 

## Summary

At the moment, `uv publish` obtains a check URL for pyx-based registries in one of two flows:

1. Explicitly configured as an index URL in the index configuration
2. Implicitly discovered/inferred through `token_store.is_known_url`

The end result of either of these flows is that `uv publish` does a pre-upload check against the simple index before attempting to upload a distribution. However, this isn't needed in pyx's case, since pyx matches PyPI's behavior of idempotency on uploads -- pyx returns a 200 if the `(filename, digest)` pair has already been uploaded.

So, this PR just removes both of those flows. (2) is easy, (1) requires us to special-case the configuration lookup slightly to *ignore* the index URL if its publish URL is known as a pyx upload URL.

Notably, we *do* still honor a check URL for pyx if the user explicitly passes one in one the CLI. I think this is fine -- it *will* work in most publishing scenarios, just not ones that we're otherwise automating (i.e., Trusted Publishing).

## Test Plan

I'm going to drive some e2e testing on pyx-auth-action using builds against these changes. That'll confirm that we don't secretly introduce the check URL anywhere else.

---

_Review requested from @charliermarsh by @woodruffw on 2025-10-10 15:55_

---

_Review requested from @zanieb by @woodruffw on 2025-10-10 15:55_

---

_Review requested from @konstin by @woodruffw on 2025-10-10 15:55_

---

_Assigned to @woodruffw by @woodruffw on 2025-10-10 15:55_

---

_Label `enhancement` added by @woodruffw on 2025-10-10 15:55_

---

_@charliermarsh approved on 2025-10-10 16:13_

---

_Comment by @woodruffw on 2025-10-10 16:46_

Successful e2e test: https://github.com/astral-sh/pyx-auth-action/actions/runs/18412825129/job/52469333708?pr=14

---

_Merged by @woodruffw on 2025-10-10 16:51_

---

_Closed by @woodruffw on 2025-10-10 16:51_

---

_Branch deleted on 2025-10-10 16:51_

---

_Comment by @konstin on 2025-10-13 10:15_

> The end result of either of these flows is that `uv publish` does a pre-upload check against the simple index before attempting to upload a distribution. However, this isn't needed in pyx's case, since pyx matches PyPI's behavior of idempotency on uploads -- pyx returns a 200 if the `(filename, digest)` pair has already been uploaded.

FWIW another difference is that pre-check will skip the actual upload if the file already exists, i.e. a performance optimization.

---
