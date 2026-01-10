```yaml
number: 17346
title: Use DisplaySafeUrl to readact a URL in a trace log
type: pull_request
state: merged
author: woodruffw
labels:
  - bug
assignees: []
merged: true
base: main
head: ww/redact-url
created_at: 2026-01-07T15:42:39Z
updated_at: 2026-01-07T15:55:10Z
url: https://github.com/astral-sh/uv/pull/17346
synced_at: 2026-01-10T05:49:14Z
```

# Use DisplaySafeUrl to readact a URL in a trace log

---

_Pull request opened by @woodruffw on 2026-01-07 15:42_

## Summary

Redacts a `Url` by transforming it into a `DisplaySafeUrl` before logging. This requires a clone, since `DisplaySafeUrl` is owning. Note that this doesn't use `DisplaySafeUrl::parse`, so it doesn't include the ambiguity detections we normally perform. I could add that, although that would mean inserting a fallible API on the path here.

Ideally we'd have a lint for these -- we should flag any dataflow where a `Url` value flows into an output sink (like `trace!`). Not sure how easy this would be to add, but I can look into it.

See #17343.

## Test Plan

I'm not sure what the best way to test this is -- maybe a mock server that intentionally flakes the first request to exercise the logic? Consequently I haven't added a new test yet.

---

_Review requested from @zanieb by @woodruffw on 2026-01-07 15:42_

---

_Review requested from @konstin by @woodruffw on 2026-01-07 15:42_

---

_Assigned to @woodruffw by @woodruffw on 2026-01-07 15:42_

---

_@konstin approved on 2026-01-07 15:44_

---

_Comment by @zanieb on 2026-01-07 15:45_

On the `clone`... another case of https://github.com/astral-sh/uv/blob/62bf92132b848df77747a10c92809667c9af9e4d/crates/uv-auth/src/store.rs#L341

---

_Label `bug` added by @konstin on 2026-01-07 15:45_

---

_Comment by @woodruffw on 2026-01-07 15:49_

Oh yeah, a `DisplaySafeUrlRef` would be nice. I can do that as a follow-up.

---

_Comment by @konstin on 2026-01-07 15:49_

A lint would be nice, but I'm not sure if clippy can do that much indirection. For the cloning, I think that's totally fine here. We could do testing by capturing logs (`-vv`), and we also have the testing for network retries with test servers that fail, but I'm not sure how much reassurance they would actually provide.

---

_Comment by @zanieb on 2026-01-07 15:50_

yeah I'd skip the test

---

_Merged by @woodruffw on 2026-01-07 15:55_

---

_Closed by @woodruffw on 2026-01-07 15:55_

---

_Branch deleted on 2026-01-07 15:55_

---
