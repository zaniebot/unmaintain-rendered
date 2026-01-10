```yaml
number: 13221
title: "`uv self update` issues an unclear warning that is actually an error"
type: issue
state: closed
author: nedbat
labels:
  - error messages
assignees: []
created_at: 2025-04-30T10:24:19Z
updated_at: 2025-04-30T17:23:31Z
url: https://github.com/astral-sh/uv/issues/13221
synced_at: 2026-01-10T03:41:47Z
```

# `uv self update` issues an unclear warning that is actually an error

---

_Issue opened by @nedbat on 2025-04-30 10:24_

### Summary

```
% uv self update
warning: Self-update is only available for uv binaries installed via the standalone installation scripts.

If you installed uv with pip, brew, or another package manager, update uv with `pip install --upgrade`, `brew upgrade`, or similar.
```
Labeling this message as a warning is misleading.   The command didn't update uv because I had not installed via the standalone installation, so this is actually an error.  A warning should mean, "I did the thing, but you should be aware of...."

TBH, I wouldn't be a fan of labeling it "error" exactly either, since that sounds like something bad happened.  Perhaps a message that indicates what happened:
```
Self-update is only available for uv binaries installed via the standalone installation scripts.  Nothing updated.

If you installed ... etc.
```

### Platform

Darwin 24.4.0 arm64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

Python 3.9.22

---

_Label `bug` added by @nedbat on 2025-04-30 10:24_

---

_Assigned to @Gankra by @konstin on 2025-04-30 10:26_

---

_Comment by @zanieb on 2025-04-30 13:59_

Thanks!

I do think "error" is appropriate for consistency with the rest of the CLI, because the intent of the user (to perform an update) was not achieved and we exit with a non-zero code.

---

_Label `bug` removed by @zanieb on 2025-04-30 13:59_

---

_Label `error messages` added by @zanieb on 2025-04-30 13:59_

---

_Closed by @zanieb on 2025-04-30 17:23_

---
