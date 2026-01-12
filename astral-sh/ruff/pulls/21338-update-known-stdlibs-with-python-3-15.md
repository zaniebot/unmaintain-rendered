```yaml
number: 21338
title: "Update known stdlibs with python `3.15`"
type: pull_request
state: closed
author: ShaharNaveh
labels:
  - python315
assignees: []
base: main
head: update-stdlibs-3-15
created_at: 2025-11-08T11:52:12Z
updated_at: 2025-11-10T09:17:24Z
url: https://github.com/astral-sh/ruff/pull/21338
synced_at: 2026-01-12T15:57:21Z
```

# Update known stdlibs with python `3.15`

---

_@ShaharNaveh_

Follow up on https://github.com/astral-sh/ruff/issues/21119#issuecomment-3462875240


---

_@ShaharNaveh reviewed on 2025-11-08 11:53_

---

_Review comment by @ShaharNaveh on `scripts/generate_known_standard_library.py`:8 on 2025-11-08 11:53_

I got an error when running from the `scripts` directory.
I can revert this if it's an issue. 

---

_Comment by @MichaReiser on 2025-11-08 16:16_

Thank you.

I'm not sure if it makes sense to update the stdlib if Ruff doesn't have support for Python 3.15. I think I'd prefer to wait for when we add Python 3.15 support once the beta is closer, to ensure that we don't ship with a stale stdlib for 3.15

---

_Comment by @ShaharNaveh on 2025-11-08 19:38_

> Thank you.
> 
> I'm not sure if it makes sense to update the stdlib if Ruff doesn't have support for Python 3.15. I think I'd prefer to wait for when we add Python 3.15 support once the beta is closer, to ensure that we don't ship with a stale stdlib for 3.15

Sure, feel free to close this PR if you think it's just noise in  the PR pool ATM.

---

What is the approach when supporting a new Python version? When will users can set `python-version = "PY315"` for example? (even if it's still considered experimental)

Does ruff have a contributer's guide on when  to implement support for new Python  versions?

---

_Comment by @MichaReiser on 2025-11-10 09:17_

> What is the approach when supporting a new Python version? When will users can set python-version = "PY315" for example? (even if it's still considered experimental)

In the past, we waited for the new Python version to be closer to beta to avoid implementing features that don't end up being stabilized (or have to change the semantics multiple times). 

> Does ruff have a contributer's guide on when to implement support for new Python versions?

We don't have a contributor guide for adding support for a new Python version but you can search for the [`python314`](https://github.com/astral-sh/ruff/pulls?q=sort%3Aupdated-desc+is%3Apr+is%3Aclosed+label%3Apython314) label to see the relevant PRs for adding Python 3.14. That should give you a good idea on how we go about it.

---

_Closed by @MichaReiser on 2025-11-10 09:17_

---

_Label `python315` added by @MichaReiser on 2025-11-10 09:17_

---
