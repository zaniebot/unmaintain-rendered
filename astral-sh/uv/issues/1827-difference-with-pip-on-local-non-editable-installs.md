---
number: 1827
title: Difference with pip on local non-editable installs
type: issue
state: closed
author: hsheth2
labels: []
assignees: []
created_at: 2024-02-21T19:54:19Z
updated_at: 2024-03-22T21:26:13Z
url: https://github.com/astral-sh/uv/issues/1827
synced_at: 2026-01-10T01:23:09Z
---

# Difference with pip on local non-editable installs

---

_Issue opened by @hsheth2 on 2024-02-21 19:54_


```sh
$ uv pip install '.'
error: Failed to parse `.`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
.

# whereas pip allows this
$ pip install '.'  # works as expected
```

I saw #1000 - if I do `uv pip install 'package-name @ .'` then it works, but pip doesn't like that syntax. Ideally there'd be something that works uniformly across both.

```
$ uv --version
0.1.6
```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-02-21 19:55_

👍 This is the same as https://github.com/astral-sh/uv/issues/313 -- we'll lift this restriction eventually, it just made life easier for reasons that relate to implementation details.

---

_Closed by @charliermarsh on 2024-02-21 19:55_

---

_Comment by @charliermarsh on 2024-03-22 21:26_

This is supported as of `v0.1.24`. You can `uv pip install .` directly, without including a package name.

---

_Referenced in [astral-sh/uv#10941](../../astral-sh/uv/issues/10941.md) on 2025-01-24 18:10_

---
