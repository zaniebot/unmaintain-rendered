```yaml
number: 3107
title: Add test for avoiding irrelevant extras
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/ex-test
created_at: 2024-04-17T21:05:57Z
updated_at: 2024-04-19T00:47:28Z
url: https://github.com/astral-sh/uv/pull/3107
synced_at: 2026-01-10T14:43:31Z
```

# Add test for avoiding irrelevant extras

---

_Pull request opened by @charliermarsh on 2024-04-17 21:05_

## Summary

This PR adds a test that currently leads to an error, but should successfully resolve as of https://github.com/astral-sh/uv/pull/3100.

The core idea is that if we have a pinned package, we shouldn't try to build other versions of that package if we have an unconstrained variant with an extra.

---

_Label `testing` added by @charliermarsh on 2024-04-17 21:06_

---

_Marked ready for review by @charliermarsh on 2024-04-17 21:06_

---

_Comment by @zanieb on 2024-04-17 21:30_

I wish we could do these without committing tarballs so we could see the source code, but it doesn't seem worth calling a real build backend from the test... unless we had a little one in Rust. 

---

_Comment by @charliermarsh on 2024-04-17 21:34_

Yeah. I could commit... both? But I agree.

---

_Comment by @charliermarsh on 2024-04-17 21:35_

I assume you say that from both a security and clarity/code-review perspective.

---

_Comment by @zanieb on 2024-04-17 21:44_

Both plus the script used to generate the tarballs seems like the simplest path forward for now.

---

_Comment by @charliermarsh on 2024-04-17 22:17_

I didn't actually use a script. I took `scripts/package/setup_py_editable`, generated it, then modified the generated tarball for the two cases.

---

_@konstin approved on 2024-04-18 08:49_

---

_Merged by @charliermarsh on 2024-04-19 00:47_

---

_Closed by @charliermarsh on 2024-04-19 00:47_

---

_Branch deleted on 2024-04-19 00:47_

---
