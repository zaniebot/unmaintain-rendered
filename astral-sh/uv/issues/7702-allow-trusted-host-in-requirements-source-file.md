```yaml
number: 7702
title: Allow trusted-host in requirements source file
type: issue
state: closed
author: gma2th
labels:
  - compatibility
assignees: []
created_at: 2024-09-26T09:32:41Z
updated_at: 2024-09-26T18:53:42Z
url: https://github.com/astral-sh/uv/issues/7702
synced_at: 2026-01-10T04:45:10Z
```

# Allow trusted-host in requirements source file

---

_Issue opened by @gma2th on 2024-09-26 09:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Description 

uv allows to specifiy `--index-url` in the requirements source file for `uv pip install` or `uv pip compile`.
However it doesn't allow to specify `--trusted-host`.
This is useful when the host doesn't support https.

## Example

```
$ cat req.in
--index-url https://pypi.org/simple

$ uv pip install -r req.in 
Audited 1 package in 1ms

$ cat req.in
--index-url https://pypi.org/simple
--trusted-host pypi.org

$ uv pip install -r req.in
error: Unexpected '-', expected '-c', '-e', '-r' or the start of a requirement at req.in:2:1
```

```
uv version
uv 0.4.16 (e81ed8ec5 2024-09-24)
```

---

_Comment by @zanieb on 2024-09-26 12:37_

Generally we don't want to add options to requirements files and it seems less than ideal to do so for trusting an insecure host. Why not pass the option during invocation?

---

_Comment by @gma2th on 2024-09-26 13:19_

To not having to pass it every time one want to install/compile the requirements.

---

_Comment by @zanieb on 2024-09-26 14:26_

Yeah but can you see how it is a security concern to allow that?

---

_Comment by @charliermarsh on 2024-09-26 18:53_

Yeah I'd prefer not to support this. It doesn't seem like the right security / usability tradeoff. You can already put this in your `pyproject.toml` if you want to avoid passing it repeatedly.

---

_Closed by @charliermarsh on 2024-09-26 18:53_

---

_Label `compatibility` added by @charliermarsh on 2024-09-26 18:53_

---
