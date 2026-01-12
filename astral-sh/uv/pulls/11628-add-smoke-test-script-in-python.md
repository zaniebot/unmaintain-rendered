```yaml
number: 11628
title: Add smoke test script in Python
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/smoke-test
created_at: 2025-02-19T17:48:58Z
updated_at: 2025-03-28T14:44:36Z
url: https://github.com/astral-sh/uv/pull/11628
synced_at: 2026-01-12T16:09:55Z
```

# Add smoke test script in Python

---

_@zanieb_

I wanted to consolidate these anyway, and apparently it's a huge pain to make a Windows task fail early via GitHub's PowerShell setup so I implement this in Python instead.

---

_Label `testing` added by @zanieb on 2025-02-19 17:48_

---

_Marked ready for review by @zanieb on 2025-02-19 18:02_

---

_Comment by @zanieb on 2025-02-19 18:02_

Great it failed on aarch64 as desired! https://github.com/astral-sh/uv/actions/runs/13419135061/job/37487489440?pr=11628



---

_@zanieb reviewed on 2025-02-19 18:06_

---

_Review comment by @zanieb on `scripts/smoke-test/commands.sh`:1 on 2025-02-19 18:06_

We probably _could_ just have a list of shell scripts here and actually run them with a shell, but just doing something that fulfills the current needs right now.

---

_@charliermarsh reviewed on 2025-02-27 00:04_

---

_Review comment by @charliermarsh on `scripts/smoke-test/commands.sh`:1 on 2025-02-27 00:04_

Can we just inline this as a string into `smoke-test/__main__.py`? It feels a little safer than reading and executing this file.

---

_@konstin approved on 2025-02-27 08:52_

---

_@zanieb reviewed on 2025-03-11 21:02_

---

_Review comment by @zanieb on `scripts/smoke-test/commands.sh`:1 on 2025-03-11 21:02_

I wanted syntax highlighting, but... okay :)

---

_@zanieb reviewed on 2025-03-11 21:05_

---

_Review comment by @zanieb on `scripts/smoke-test/commands.sh`:1 on 2025-03-11 21:05_

I did this but... idk I actually do care about syntax highlighting so there can be comments. I don't want to clutter `__main__.py` as we add more commands and I don't want to have to deal with Python syntax.

---

_@zanieb reviewed on 2025-03-11 21:14_

---

_Review comment by @zanieb on `scripts/smoke-test/commands.sh`:1 on 2025-03-11 21:14_

I think I'm going to do what I said above and just invoke separate shell scripts.

---

_Comment by @zanieb on 2025-03-12 17:19_

_sigh_ they don't have `sh` on aarch64 Windows runners...

---

_@zanieb reviewed on 2025-03-12 17:23_

---

_Review comment by @zanieb on `scripts/smoke-test/commands.sh`:1 on 2025-03-12 17:23_

Going back because `sh` isn't available on aarch64. We could switch in the future once it is.

---

_Merged by @zanieb on 2025-03-27 20:35_

---

_Closed by @zanieb on 2025-03-27 20:35_

---

_Branch deleted on 2025-03-27 20:35_

---

_Review comment by @petebachant on `regular_file; echo foo; #`:1 on 2025-03-28 14:10_

Is this file supposed to exist?

---

_@petebachant reviewed on 2025-03-28 14:10_

---

_@zanieb reviewed on 2025-03-28 14:44_

---

_Review comment by @zanieb on `regular_file; echo foo; #`:1 on 2025-03-28 14:44_

No, thanks!

---
