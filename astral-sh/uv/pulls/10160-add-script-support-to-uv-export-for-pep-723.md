```yaml
number: 10160
title: "Add `--script` support to `uv export` for PEP 723 scripts"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/script-export
created_at: 2024-12-25T19:18:25Z
updated_at: 2025-01-08T21:48:55Z
url: https://github.com/astral-sh/uv/pull/10160
synced_at: 2026-01-12T16:09:09Z
```

# Add `--script` support to `uv export` for PEP 723 scripts

---

_@charliermarsh_

## Summary

You can now run `uv export --script main.py` to show the dependency tree for a given script. If a lockfile doesn't exist, it will create one.

Closes https://github.com/astral-sh/uv/issues/8609.

Closes https://github.com/astral-sh/uv/issues/9657.


---

_Label `enhancement` added by @charliermarsh on 2024-12-25 19:18_

---

_Label `no-build` added by @charliermarsh on 2024-12-25 19:18_

---

_Label `no-test` added by @charliermarsh on 2024-12-25 19:18_

---

_Converted to draft by @charliermarsh on 2024-12-25 19:18_

---

_Label `no-build` removed by @charliermarsh on 2024-12-25 22:20_

---

_Label `no-test` removed by @charliermarsh on 2024-12-25 22:20_

---

_Marked ready for review by @charliermarsh on 2024-12-25 22:20_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-26 20:02_

---

_Comment by @zanieb on 2025-01-08 18:06_

> If a lockfile doesn't exist, it will create one.

And it will stick around? I might find that surprising, since it alters future invocations.

---

_Comment by @charliermarsh on 2025-01-08 18:06_

Yeah, it will stick around.

---

_Comment by @zanieb on 2025-01-08 18:07_

Could we avoid that? Did you have a motivation for that behavior other than simplicity?

---

_Comment by @charliermarsh on 2025-01-08 18:08_

I think we can avoid it if you prefer, yeah.

---

_Comment by @charliermarsh on 2025-01-08 18:11_

(It seems like a reasonable change to me.)

---

_Comment by @zanieb on 2025-01-08 18:12_

I do prefer for now. Thanks!

---

_Comment by @charliermarsh on 2025-01-08 21:20_

@zanieb -- I made the same change for `uv tree` -- we don't persist the lockfile unless it already existed.

---

_@zanieb approved on 2025-01-08 21:24_

---

_Merged by @charliermarsh on 2025-01-08 21:48_

---

_Closed by @charliermarsh on 2025-01-08 21:48_

---

_Branch deleted on 2025-01-08 21:48_

---
