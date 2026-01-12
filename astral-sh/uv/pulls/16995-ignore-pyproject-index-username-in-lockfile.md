```yaml
number: 16995
title: Ignore pyproject index username in lockfile comparison
type: pull_request
state: merged
author: jkipper
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/authenticated-update
created_at: 2025-12-05T07:43:47Z
updated_at: 2025-12-16T10:47:51Z
url: https://github.com/astral-sh/uv/pull/16995
synced_at: 2026-01-12T16:12:33Z
```

# Ignore pyproject index username in lockfile comparison

---

_@jkipper_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Pyproject.toml index url may contain a username while lockfile doesn't. Treat it as the same index to prevent unintended package updates

Fixes #16436



---

_Comment by @zanieb on 2025-12-06 00:27_

Do you have an integration test, e.g., for `uv lock` that demonstrates that this fixes https://github.com/astral-sh/uv/issues/16436 ?

---

_Comment by @jkipper on 2025-12-08 14:58_

Not directly, turns out I did not add the part that actually caused the issue in that. Still not sure why I found that correlation but it's not happening that way anymore and the username part is what I was actually seeing. (I originally reported that issue)







---

_Comment by @zanieb on 2025-12-08 17:04_

Sorry I don't quite understand what you mean.

---

_Comment by @jkipper on 2025-12-08 18:12_

I reported that issue based on my observation that it was related to which sort of packaging was used, but the upgrade was actually caused by adding a username to an index. 

So this fixes the linked issue, but the actual cause wasn't included when I reported it.  

---

_Comment by @zanieb on 2025-12-08 18:35_

I think we need an integration test case that demonstrates the change in behavior this pull request causes, i.e., at the `uv lock` level

---

_Comment by @jkipper on 2025-12-09 14:11_

Got it. Added one now.

---

_Label `bug` added by @konstin on 2025-12-16 10:33_

---

_@konstin approved on 2025-12-16 10:34_

Thank you!

---

_Merged by @konstin on 2025-12-16 10:47_

---

_Closed by @konstin on 2025-12-16 10:47_

---
