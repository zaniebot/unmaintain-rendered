---
number: 10425
title: "`uv pip install --require-hashes` fails when (transitive) dependencies aren't pinned to latest version"
type: issue
state: closed
author: cpswan
labels:
  - bug
assignees: []
created_at: 2025-01-09T13:32:45Z
updated_at: 2025-01-13T10:18:52Z
url: https://github.com/astral-sh/uv/issues/10425
synced_at: 2026-01-10T01:24:53Z
---

# `uv pip install --require-hashes` fails when (transitive) dependencies aren't pinned to latest version

---

_Issue opened by @cpswan on 2025-01-09 13:32_

uv version 0.5.16

## Description

When I `uv pip install --require-hashes -r requirements.txt` I get:

```log
⠙ urllib3==2.3.0
error: In `--require-hashes` mode, all requirements must be pinned upfront with `==`, but found: `urllib3`
```

But my `requirements.txt` does specify a version and hashes for `urllib3`:

```txt
urllib3==2.2.3 ; python_version >= "3.8" \
    --hash=sha256:ca899ca043dcb1bafa3e262d73aa25c465bfb49e0bd9dd5d59f1d0acba2f8fac \
    --hash=sha256:e7d814a81dad81e6caf2ec9fdedb284ecc9c73076b62654547cc64ccdcae26e9
```

This doesn't happen if I have a `requirements.txt` that pins to `urllib3==2.3.0`, which is the latest version.

## Hypothesis

The dependency solver is trying to pull in the latest version of `urllib3` rather than the version pinned in `requirements.txt`, and (obviously) not finding pinning (or hashes) for that version.

## How I got here (reproduction)

I start out with a `pyproject.toml`:

```toml
[project]
name = "test-require-hashes"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "requests>=2.32.3",
]
```

Then generate a [poetry.lock](https://gist.github.com/cpswan/bed14d6fe28a0aef29698e23cb3d43d8#file-poetry-lock) file with `poetry lock`. Inside that I can see:

```txt
[[package]]
name = "requests"
version = "2.32.3"
description = "Python HTTP for Humans."
optional = false
python-versions = ">=3.8"
groups = ["main"]
files = [
    {file = "requests-2.32.3-py3-none-any.whl", hash = "sha256:70761cfe03c773ceb22aa2f671b4757976145175cdfca038c02654d061d6dcc6"},
    {file = "requests-2.32.3.tar.gz", hash = "sha256:55365417734eb18255590a9ff9eb97e9e1da868d4ccd6402399eaf68af20a760"},
]
```

NB I'm using poetry 2.0.0 here as it respects PEP 621. I've also recreated this with poetry 1.8.2, but the pyproject.toml needs to use the old style [tool.poetry] rather than [project].

Then generate a [requirements.txt](https://gist.github.com/cpswan/bed14d6fe28a0aef29698e23cb3d43d8#file-requirements-txt) from the poetry.lock with `poetry export --output requirements.txt`, which contains:

```txt
urllib3==2.2.3 ; python_version >= "3.8" \
    --hash=sha256:ca899ca043dcb1bafa3e262d73aa25c465bfb49e0bd9dd5d59f1d0acba2f8fac \
    --hash=sha256:e7d814a81dad81e6caf2ec9fdedb284ecc9c73076b62654547cc64ccdcae26e9
```

Finally ask uv to install from that requirements.txt with --require-hashes `uv pip install --require-hashes -r requirements.txt`

### Python 3.8 is deprecated

Yes, I know, if I specify `requires-python = ">=3.9"` then I get a poetry.lock and requirements.txt that use urllib3==2.3.0

Whilst using 3.8 seems to be what surfaced this issue for me I don't think it's the fundamental cause.

---

_Referenced in [atsign-foundation/at_python#310](../../atsign-foundation/at_python/pulls/310.md) on 2025-01-09 13:40_

---

_Comment by @cpswan on 2025-01-09 14:10_

I meant to also add the [`--verbose` logs](https://gist.github.com/cpswan/bed14d6fe28a0aef29698e23cb3d43d8#file-verbose-log), which ends:

```log
<snip lines 1-76>
DEBUG Searching for a compatible version of urllib3 (>=1.21.1, <3)
DEBUG Found installed version of urllib3==2.3.0 that satisfies >=1.21.1, <3
DEBUG Selecting: urllib3==2.3.0 [installed] (installed)
DEBUG Released lock at `/home/chris/testing/uv_require_hashes/test_require_hashes/.venv/.lock`
error: In `--require-hashes` mode, all requirements must be pinned upfront with `==`, but found: `urllib3`
```

Other mentions of urllib3:

```log
<snip lines 1-5>
DEBUG At least one requirement is not satisfied: urllib3==2.2.3 ; python_full_version >= '3.8'
<snip lines 7-13>
DEBUG Adding direct dependency: urllib3{python_full_version >= '3.8'}>=2.2.3, <2.2.3+
<snip lines 15-55>
DEBUG Found fresh response for: https://pypi.org/simple/urllib3/
<snip lines 57-66>
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
<snip lines 68-71>
DEBUG Found installed version of urllib3==2.3.0 that satisfies >=1.21.1, <3
<snip lines 73-74>
DEBUG Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3
<snip lines 76-81
```

So the puzzle here is how `Adding direct dependency: urllib3{python_full_version >= '3.8'}>=2.2.3, <2.2.3+` isn't used to satisfy `Adding transitive dependency for requests==2.32.3: urllib3>=1.21.1, <3` and instead it goes off on a search that ends with `Found installed version of urllib3==2.3.0 that satisfies >=1.21.1, <3`

---

_Comment by @charliermarsh on 2025-01-09 14:12_

Is this the entire `requirements.txt` file?
```
urllib3==2.2.3 ; python_version >= "3.8" \
    --hash=sha256:ca899ca043dcb1bafa3e262d73aa25c465bfb49e0bd9dd5d59f1d0acba2f8fac \
    --hash=sha256:e7d814a81dad81e6caf2ec9fdedb284ecc9c73076b62654547cc64ccdcae26e9
```

---

_Comment by @cpswan on 2025-01-09 14:15_

> Is this the entire requirements.txt file?

No, I linked to a [gist with the entire requirements.txt](https://gist.github.com/cpswan/bed14d6fe28a0aef29698e23cb3d43d8#file-requirements-txt) file as pasting huge blobs of text into issues distracts from the narrative.

---

_Comment by @charliermarsh on 2025-01-09 14:27_

Thanks! Missed it.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-09 14:35_

---

_Label `bug` added by @charliermarsh on 2025-01-09 17:52_

---

_Referenced in [astral-sh/uv#10441](../../astral-sh/uv/pulls/10441.md) on 2025-01-09 18:44_

---

_Closed by @charliermarsh on 2025-01-09 20:37_

---

_Closed by @charliermarsh on 2025-01-09 20:37_

---

_Comment by @cpswan on 2025-01-13 10:18_

Thanks for the quick resolution on this @charliermarsh :)

---

_Referenced in [atsign-foundation/at_python#313](../../atsign-foundation/at_python/pulls/313.md) on 2025-01-13 10:19_

---

_Referenced in [astral-sh/uv#10833](../../astral-sh/uv/pulls/10833.md) on 2025-01-21 23:15_

---
