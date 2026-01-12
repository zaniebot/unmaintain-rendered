```yaml
number: 15647
title: "Installing vllm nightly from S3 path fails but leaves `uv.lock` in a unparseable, bad state"
type: issue
state: closed
author: vadimkantorov
labels:
  - bug
assignees: []
created_at: 2025-09-03T00:22:00Z
updated_at: 2025-09-30T00:14:17Z
url: https://github.com/astral-sh/uv/issues/15647
synced_at: 2026-01-12T16:02:14Z
```

# Installing vllm nightly from S3 path fails but leaves `uv.lock` in a unparseable, bad state

---

_@vadimkantorov_

### Summary

Running twice `uv sync --active` leads to:
```
error: Failed to parse `uv.lock`
  Caused by: The entry for package `vllm` v0.10.1rc2.dev397+g038e9be4e has wheel `vllm-1.0.0.dev0-cp38-abi3-manylinux1_x86_64.whl` with inconsistent version: v1.0.0.dev0
```

Even if `uv sync --active` installation fails because of vllm not following filename<>metadata consistency, I think it should not leave `uv.lock` in unparseable state

My OP is here:
- https://github.com/vllm-project/vllm/issues/24126

```toml
# pyproject.toml - a single file in curdir

[build-system]
requires = ["setuptools>=65", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name =  "test_vllm"
version = "0.0.0.1"
requires-python = ">=3.12"
dependencies = [
    "vllm",
    "torch>=2.8",
    "flash-attn==2.8.2",
]

[tool.uv.sources]
vllm = { url = "https://vllm-wheels.s3.amazonaws.com/038e9be4eb7a63189c8980845d80cb96957b9919/vllm-1.0.0.dev-cp38-abi3-manylinux1_x86_64.whl" }

[tool.uv]
override-dependencies = ["outlines-core==0.2.10"]
```

Related:
- https://github.com/astral-sh/uv/issues/8082
- https://github.com/vllm-project/vllm/issues/9244

### Platform

Ubuntu-22.04

### Version

uv 0.8.13

### Python version

Python 3.12.11

---

_Label `bug` added by @vadimkantorov on 2025-09-03 00:22_

---

_Comment by @konstin on 2025-09-05 11:25_

We should catch this before writing the lockfile, potentially already in the resolver, and reject entries with a mismatching versions between METADATA and wheel filename.

---

_Comment by @vadimkantorov on 2025-09-05 15:26_

(on the root cause about the mismatch - I wish there was a knob to somehow "override" and provide the good metadata, the correct virtual "renamed" file name or tell uv to ignore the mismatch to work around badly packaged wheel files. because of this legacy pip behavior - I guess these packaging bug happen and it would be good to allow end-user to hack around using some explicit knobs)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-27 23:47_

---

_Closed by @charliermarsh on 2025-09-29 23:54_

---

_Comment by @vadimkantorov on 2025-09-29 23:59_

@charliermarsh Does the linked PR fix this issue? The issue here was that if the filename check is on, uv.lock becomes unparseable

The linked PR just allows to temporarily disable the filename check (the nasty part is that it still requires the user to set an env variable, so if I'm using a direct wheel http link in my pyproject.toml, it is not sufficient). But if the check is on, the bug should still be present?

---

_Comment by @charliermarsh on 2025-09-30 00:01_

Hmm, I don't quite follow. Yes, you need to set the environment variable -- that's intentional. Why is that a problem?

---

_Comment by @vadimkantorov on 2025-09-30 00:05_

> Hmm, I don't quite follow. Yes, you need to set the environment variable -- that's intentional. Why is that a problem?

I meant it's a bit of nuisance that it can't be enabled from `pyproject.toml` itself for this case

---

And for the rest, I wonder if uv.lock still becomes unparseable for the example `pyproject.toml` in the OP if the env variable is not set?

---

_Comment by @charliermarsh on 2025-09-30 00:06_

I think it's appropriate for this to require an explicit environment variable. The lock isn't actually unparseable -- we just reject it because we run this validation, and the environment variable disables the validation.

---

_Comment by @vadimkantorov on 2025-09-30 00:12_

> The lock isn't actually unparseable

`error: Failed to parse uv.lock`, maybe the error message should be adjusted...

---

E.g. consider the following case: `UV_SKIP_WHEEL_FILENAME_CHECK=1 uv sync` is done, the wheel url gets placed into the `uv.lock`. And in the next run I forgot to again set the env variable. Likely it will complain again with `error: Failed to parse uv.lock`... I propose to:
1. adjust the error message to not say `Failed to parse`, use some other wording
2. maybe skip validate file names of wheels already in `uv.lock` - or at least use some other wording...

---

_Comment by @charliermarsh on 2025-09-30 00:14_

Respectfully, I don't think those are worth doing, the error message already includes a hint to use the environment variable.

---
