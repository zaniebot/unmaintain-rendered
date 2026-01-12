```yaml
number: 2038
title: Managing dependencies on top of system site-packages
type: issue
state: closed
author: fbergroth
labels:
  - question
assignees: []
created_at: 2024-02-28T08:43:23Z
updated_at: 2025-04-15T09:15:12Z
url: https://github.com/astral-sh/uv/issues/2038
synced_at: 2026-01-12T15:58:34Z
```

# Managing dependencies on top of system site-packages

---

_@fbergroth_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hi,

I'm currently working with the docker image `l4t-pytorch`, which already includes certain system libraries like `torch==2.0.0a0+ec3941ad.nv23.2`.

My goal is to add extra dependencies on top of this and manage them with uv, while pinning the preinstalled version of torch.

I'm trying to this via a venv with system-site-packages and declaring a dependency on `torch==2.0.0a0`, which fails since it can't resolve dependencies.

Is there any trick to do this or something I might have overlooked?

Thanks


---

_Comment by @zanieb on 2024-02-28 18:09_

Hi! It sounds like you need a couple things that are in-progress still:

- #1483 
- #2046 
- https://github.com/astral-sh/uv/issues/1855

---

_Label `question` added by @zanieb on 2024-02-28 18:09_

---

_Comment by @fbergroth on 2024-02-28 20:46_

Thanks for having a look. I believe I can already work around the first of the two linked issues, and I'm not sure the last one applies.

My core issue seems to be that the version of torch already installed has a pre-release identifier (I don't think the local version identifier is the issue here). I'm trying to use the system version by pinning torch to this version (`2.0.0a0`), but `uv pip compile` fails since it can't find it in any package index. I'm not sure of the best way to handle this.

---

_Comment by @fbergroth on 2024-02-29 13:48_

I think the scenario is too odd and doesn't really make sense to support.

---

_Closed by @fbergroth on 2024-02-29 13:48_

---

_Comment by @YodaEmbedding on 2025-04-15 00:25_

Is this supported?

```bash
$ uv venv --system-site-packages

$ uv run --no-sync python -c 'import torch; print(f"{torch.__version__=}")'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'torch'

$ source .venv/bin/activate
$ python -c 'import torch; print(f"{torch.__version__=}")'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ModuleNotFoundError: No module named 'torch'
```

---

_Comment by @konstin on 2025-04-15 09:15_

It's not supported yet, see https://github.com/astral-sh/uv/issues/2500

---
