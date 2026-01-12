```yaml
number: 6342
title: Incorrect update all tools command in blog post
type: issue
state: closed
author: denkasyanov
labels:
  - documentation
assignees: []
created_at: 2024-08-21T15:43:58Z
updated_at: 2024-08-21T17:06:44Z
url: https://github.com/astral-sh/uv/issues/6342
synced_at: 2026-01-12T15:59:03Z
```

# Incorrect update all tools command in blog post

---

_@denkasyanov_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

### Issue 

According to blog post https://astral.sh/blog/uv-unified-python-packaging:

> `uv tool upgrade` to upgrade all installed tools to their latest supported versions.

According to [docs](https://docs.astral.sh/uv/guides/tools/#upgrading-tools) it is `uv tool upgrade --all`

I couldn't find a repo for blog posts to make a PR, so leaving it here

### Proposed solution

Looks like

"or `uv tool upgrade` to upgrade all installed tools to their latest supported versions."

should be

"or `uv tool upgrade --all` to upgrade all installed tools to their latest supported versions." in the [blog post](https://astral.sh/blog/uv-unified-python-packaging)

### Motivation

Maybe some people like me are more comfortable to follow along the blog post than docs. This change will align it with docs.

Confirmed to work according to the docs on version `uv 0.3.0 (dd1934c9c 2024-08-20)`

---

_Comment by @charliermarsh on 2024-08-21 15:46_

Ah thanks, that makes sense.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-21 15:46_

---

_Label `documentation` added by @charliermarsh on 2024-08-21 15:46_

---

_Comment by @charliermarsh on 2024-08-21 17:06_

(Just pushed a fix, should deploy shortly.)

---

_Closed by @charliermarsh on 2024-08-21 17:06_

---
