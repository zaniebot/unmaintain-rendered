```yaml
number: 3002
title: "Deduplicate symbolic links between `purelib` and `platlib`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/suse
created_at: 2024-04-11T23:03:27Z
updated_at: 2024-04-12T21:08:57Z
url: https://github.com/astral-sh/uv/pull/3002
synced_at: 2026-01-12T16:05:21Z
```

# Deduplicate symbolic links between `purelib` and `platlib`

---

_@charliermarsh_

## Summary

This PR adds system install tests to verify the behavior described in #2798. It turns out this behavior _also_ affects Fedora and Amazon Linux, we just didn't have the right conditions enabled (specifically, you need to create the virtualenv with `python -m venv` to get these symlinks), so the test suite was expanded to capture that.

The issue itself is also fixed by way of deduplicating the `site-packages` entries.

Closes: https://github.com/astral-sh/uv/issues/2798

---

_Label `testing` added by @charliermarsh on 2024-04-11 23:03_

---

_Review requested from @zanieb by @charliermarsh on 2024-04-11 23:52_

---

_Renamed from "Add a system install test for OpenSUSE" to "Deduplicate symbolic links between `purelib` and `platlib`" by @charliermarsh on 2024-04-11 23:54_

---

_Label `testing` removed by @charliermarsh on 2024-04-11 23:54_

---

_Label `bug` added by @charliermarsh on 2024-04-11 23:54_

---

_Marked ready for review by @charliermarsh on 2024-04-11 23:54_

---

_Comment by @Daverball on 2024-04-12 06:44_

I can confirm, that this does fix the specific issue I was having. Thanks!

---

_@zanieb approved on 2024-04-12 14:13_

Nice thank you!

---

_Merged by @charliermarsh on 2024-04-12 21:08_

---

_Closed by @charliermarsh on 2024-04-12 21:08_

---

_Branch deleted on 2024-04-12 21:08_

---
