```yaml
number: 6849
title: uv created a new venv instead of using the one I was already in
type: issue
state: closed
author: antonysouthworth-halter
labels:
  - duplicate
assignees: []
created_at: 2024-08-30T02:56:26Z
updated_at: 2024-09-08T18:19:35Z
url: https://github.com/astral-sh/uv/issues/6849
synced_at: 2026-01-12T15:59:08Z
```

# uv created a new venv instead of using the one I was already in

---

_@antonysouthworth-halter_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```bash
asdf local python 3.12.5
python -m venv venv
. venv/bin/activate
uv add ruff # <- this creates a new venv at .venv?? Why??
```

# uv version:

```bash
$ uv --version
uv 0.4.0 (d9bd3bc7a 2024-08-28)
```

# my machine

```bash
$ uname -a
Darwin Antonys-MacBook-Pro.local 23.6.0 Darwin Kernel Version 23.6.0: Mon Jul 29 21:13:00 PDT 2024; root:xnu-10063.141.2~1/RELEASE_X86_64 x86_64 i386 Darwin
```

---

_Comment by @antonysouthworth-halter on 2024-08-30 03:11_

(yes, I know this is documented behaviour at https://docs.astral.sh/uv/concepts/projects/#project-environments, but if I ran into this issue I am betting others will end up here as well)

---

_Comment by @zanieb on 2024-08-30 03:32_

Hi! This is a duplicate of https://github.com/astral-sh/uv/issues/6612

---

_Label `duplicate` added by @zanieb on 2024-08-30 03:32_

---

_Closed by @charliermarsh on 2024-09-08 18:19_

---
