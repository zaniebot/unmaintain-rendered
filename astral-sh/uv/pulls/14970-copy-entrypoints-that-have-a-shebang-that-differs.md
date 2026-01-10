```yaml
number: 14970
title: "Copy entrypoints that have a shebang that differs in `python` vs `python3`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/strip-trailing-3
created_at: 2025-07-30T13:24:55Z
updated_at: 2025-07-30T16:00:18Z
url: https://github.com/astral-sh/uv/pull/14970
synced_at: 2026-01-10T06:53:02Z
```

# Copy entrypoints that have a shebang that differs in `python` vs `python3`

---

_Pull request opened by @zanieb on 2025-07-30 13:24_

In https://github.com/astral-sh/uv/issues/14919 it was reported that uv's behavior differed after the first invocation. I noticed we weren't copying entrypoints after the first invocation. It turns out the shebangs were written with `.../python` but on a subsequent invocation the `sys.executable` was `.../python3` so we didn't detect these as matching.

This is a pretty naive fix, but it seems much easier than ensuring the entry point path exactly matches the subsequent `sys.executable` we find.

I guess we should fix this in reverse too? but I think we might always prefer `python3` when loading interpreters from environments.

See #14790 for more background.

---

_Label `bug` added by @zanieb on 2025-07-30 13:24_

---

_Marked ready for review by @zanieb on 2025-07-30 13:38_

---

_@woodruffw approved on 2025-07-30 15:55_

Looks right to me!

To confirm my understanding, here's the old behavior:

```
# OK, rewritten correctly
# previous_executable: /some/random/python3
#!/some/random/python3

# BUG, not rewritten correctly
# previous_executable: /some/random/python3
#!/some/random/python
```

Whereas the new behavior would rewrite both of these to `/some/new/python3`, correct?

---

_Comment by @zanieb on 2025-07-30 16:00_

Yep! Thanks!

---

_Merged by @zanieb on 2025-07-30 16:00_

---

_Closed by @zanieb on 2025-07-30 16:00_

---

_Branch deleted on 2025-07-30 16:00_

---
