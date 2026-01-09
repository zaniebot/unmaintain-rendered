---
number: 5941
title: "feat(builder): Add `clap::Error::remove` and a test"
type: pull_request
state: merged
author: ilyagr
labels: []
assignees: []
merged: true
base: master
head: error-remove
created_at: 2025-03-08T05:08:19Z
updated_at: 2025-03-10T21:42:06Z
url: https://github.com/clap-rs/clap/pull/5941
synced_at: 2026-01-07T13:12:20-06:00
---

# feat(builder): Add `clap::Error::remove` and a test

---

_Pull request opened by @ilyagr on 2025-03-08 05:08_

#5935

The added test covers both `Error::insert` and `Error::remove`.

If we merge this after #5942, I could make the new test use the same `assert_error` as the other ones (though I'm not sure that would be better, up to you). I could also make `assert_error` take `&Error`, but that changes a lot of call sites.


---

_@epage reviewed on 2025-03-10 16:00_

---

_Review comment by @epage on `tests/builder/error.rs`:230 on 2025-03-10 16:00_

Please remove the `Usage` part of this test.

As I said, if we want to explore that, let's explore it generally.  I'd rather not people use this as a soft justification to do so.

---
