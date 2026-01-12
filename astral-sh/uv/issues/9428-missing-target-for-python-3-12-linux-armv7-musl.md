```yaml
number: 9428
title: missing target for python 3.12-linux-armv7-musl
type: issue
state: closed
author: karmingc
labels: []
assignees: []
created_at: 2024-11-25T21:38:30Z
updated_at: 2025-02-17T15:32:50Z
url: https://github.com/astral-sh/uv/issues/9428
synced_at: 2026-01-12T15:59:50Z
```

# missing target for python 3.12-linux-armv7-musl

---

_@karmingc_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Currently using `uv` and trying to install python 3.12, but seeing the following error when it tries to install 3.12 on armv7 architecture on alpine linux.

```
error: No download found for request: cpython-3.12-linux-armv7-musl
```


---

_Comment by @charliermarsh on 2024-11-25 21:39_

Thanks -- this is a duplicate of #6890. We don't provide musl builds right now. I suggest using a non-musl glibc, or installing Python separately.

---

_Closed by @charliermarsh on 2024-11-25 21:40_

---

_Comment by @cmosguy on 2025-02-17 15:32_

@charliermarsh hi there, are you able to create another target: 

`error: No download found for request: cpython-3.8.6-linux-powerpc64le-gnu`

---
