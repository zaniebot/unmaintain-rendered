```yaml
number: 14290
title: default the test buckets dir to the canonicalized temp dir
type: pull_request
state: merged
author: oconnor663
labels:
  - testing
assignees: []
merged: true
base: main
head: jack/test_buckets_in_tmp
created_at: 2025-06-26T18:11:29Z
updated_at: 2025-06-26T21:56:21Z
url: https://github.com/astral-sh/uv/pull/14290
synced_at: 2026-01-12T16:11:07Z
```

# default the test buckets dir to the canonicalized temp dir

---

_@oconnor663_

Previously we were using the XDG data dir to avoid symlinks, but there's no particular guarantee that that's not going to be a symlink too. Using the (canonicalized) temp dir by default is also slightly nicer for a couple reasons: It's sometimes faster (an in-memory tempfs on e.g. Arch), and it makes overriding `$TMPDIR` or `%TMP%` sufficient to control where tests put temp files, without needing to override `UV_INTERNAL__TEST_DIR` too.

---

_Comment by @zanieb on 2025-06-26 18:32_

Don't you need to unset `UV_INTERNAL__TEST_DIR` to test this?

---

_Comment by @oconnor663 on 2025-06-26 18:37_

https://github.com/astral-sh/uv/actions/runs/15908534044 is a CI run on a scratch branch that [uses `temp_dir` directly without checking `UV_INTERNAL__TEST_DIR`](https://github.com/astral-sh/uv/commit/882ee5c6630eaf4dca8ab1dad5a08a1f54a04e6c). It passed except for a `cargo shear` warning, which is fixed here.

If you think this is a reasonable change, I could go ahead and remove `UV_INTERNAL__TEST_DIR` entirely as part of this PR?

---

_Marked ready for review by @oconnor663 on 2025-06-26 19:29_

---

_Review requested from @charliermarsh by @oconnor663 on 2025-06-26 19:30_

---

_Review requested from @zanieb by @oconnor663 on 2025-06-26 19:30_

---

_Comment by @oconnor663 on 2025-06-26 20:20_

Ah no, I'm probably breaking something on Windows. I got lazy and assumed that Windows failures were timeouts after the macOS runner passed. Incorrect!

---

_Comment by @oconnor663 on 2025-06-26 21:07_

Ok, "canonicalize -> simple_canonicalize" fixed it.

---

_@zanieb approved on 2025-06-26 21:09_

Fine with me

---

_Label `testing` added by @zanieb on 2025-06-26 21:09_

---

_Merged by @oconnor663 on 2025-06-26 21:56_

---

_Closed by @oconnor663 on 2025-06-26 21:56_

---

_Branch deleted on 2025-06-26 21:56_

---
