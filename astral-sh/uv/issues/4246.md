```yaml
number: 4246
title: uv compile tries to fetch unnecesary old version of a package (and fails)
type: issue
state: closed
author: jcugat
labels:
  - bug
assignees: []
created_at: 2024-06-11T18:43:46Z
updated_at: 2024-06-12T08:54:31Z
url: https://github.com/astral-sh/uv/issues/4246
synced_at: 2026-01-10T05:31:37Z
```

# uv compile tries to fetch unnecesary old version of a package (and fails)

---

_Issue opened by @jcugat on 2024-06-11 18:43_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Note that I was only able to reproduce this in my company's Artifactory and not on PyPI, not sure why yet.

Having the following minimal `uvbug.in` file:

```
torch>=2.1.2
tfx~=1.14.0
```

Running `uv pip compile uvbug.in -o uvbug.txt` returns the following error:

```
error: Failed to download `triton==0.4.1`
  Caused by: Metadata file `triton-0.4.1-cp38-cp38-manylinux2010_x86_64.whl` was not found in https://artifactory.company.com/artifactory/api/pypi/pypi/packages/packages/55/ac/2e2e994bc396edb9d2a759b76a09234c8f5e9a7a4ea464ccc218ed978fc9/triton-0.4.1-cp38-cp38-manylinux2010_x86_64.whl#sha256=56d06b5ee09a2db59f088e2e570cf7b9deb4d6065f66506df3899d917e1a02b5
```

There are two issues here I think. First one, the compilation step does not continue after this failure, which doesn't allow to output a frozen requirements file. Maybe it should ignore those download failures and continue?

Second one, is that I have no idea why it's trying to fetch that old version `0.4.1` of `triton`, since it's only depended by `torch` and that requires `triton==2.1.0`.

The full debug output with `RUST_LOG=debug` and `--verbose` is available here: https://pastecode.io/s/61pma9p0

This is using uv `0.2.10` and Python 3.8.18 on Ubuntu 24.04.

Apologies for not being able to share the same bug using the public PyPI, but if you need more info about the Artifactory setup I'll try to share it. And thanks for this amazing tool! ❤️

---

_Comment by @charliermarsh on 2024-06-11 18:51_

Thank you. I think what's happening is that we try a few triton versions, then we attempt to pre-fetch the next few. (We do some aggressive prefetching when we try and fail to find a compatible version, to avoid having to fetch versions sequentially.) So you can see `Prefetching 4 triton versions` in the logs. If we run out of compatible versions, we do continue to prefetch incompatible versions. So we end up trying to fetch `0.4.1`, and it fails.

I'm unsure on the right fix. We could make prefetching ignore these errors, or we could avoid attempting to fix incompatible versions when prefetching. I'd like @konstin's opinion on the latter.


---

_Comment by @charliermarsh on 2024-06-11 18:52_

In this specific case, actually, we should be robust to that kind of error I think. We should mark the distribution as incompatible, but we shouldn't fail.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-11 18:52_

---

_Label `bug` added by @charliermarsh on 2024-06-11 18:52_

---

_Comment by @jcugat on 2024-06-11 19:04_

Forgot to mention that this package is really corrupted, because `pip` fails but with a more specific error:

```
$ pip --version
pip 24.0 from /home/josepc/.pyenv/versions/3.8.18/envs/myproject/lib/python3.8/site-packages/pip (python 3.8)

$ pip install triton==0.4.1
Looking in indexes: https://artifactory.company.com/artifactory/api/pypi/pypi/simple/
Collecting triton==0.4.1
  Downloading https://artifactory.company.com/artifactory/api/pypi/pypi/packages/packages/55/ac/2e2e994bc396edb9d2a759b76a09234c8f5e9a7a4ea464ccc218ed978fc9/triton-0.4.1-cp38-cp38-manylinux2010_x86_64.whl (14.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 14.9/14.9 MB 89.3 MB/s eta 0:00:00
Discarding https://artifactory.company.com/artifactory/api/pypi/pypi/packages/packages/55/ac/2e2e994bc396edb9d2a759b76a09234c8f5e9a7a4ea464ccc218ed978fc9/triton-0.4.1-cp38-cp38-manylinux2010_x86_64.whl#sha256=56d06b5ee09a2db59f088e2e570cf7b9deb4d6065f66506df3899d917e1a02b5 (from https://artifactory.company.com/artifactory/api/pypi/pypi/simple/triton/): Requested triton==0.4.1 from https://artifactory.company.com/artifactory/api/pypi/pypi/packages/packages/55/ac/2e2e994bc396edb9d2a759b76a09234c8f5e9a7a4ea464ccc218ed978fc9/triton-0.4.1-cp38-cp38-manylinux2010_x86_64.whl#sha256=56d06b5ee09a2db59f088e2e570cf7b9deb4d6065f66506df3899d917e1a02b5 has inconsistent version: expected '0.4.1', but metadata has '0.4.0'
ERROR: Could not find a version that satisfies the requirement triton==0.4.1 (from versions: 0.4.1, 0.4.2, 1.0.0, 1.1.0, 1.1.1, 2.0.0.dev20221030, 2.0.0.dev20221031, 2.0.0.dev20221101, 2.0.0.dev20221103, 2.0.0.dev20221105, 2.0.0.dev20221117, 2.0.0.dev20221120, 2.0.0.dev20221202, 2.0.0.dev20230208, 2.0.0.dev20230217, 2.0.0a1, 2.0.0a2, 2.0.0, 2.0.0.post1, 2.1.0, 2.2.0, 2.3.0, 2.3.1)
ERROR: No matching distribution found for triton==0.4.1
```

---

_Closed by @charliermarsh on 2024-06-11 19:49_

---

_Comment by @konstin on 2024-06-12 08:54_

We should ignore (or rather store) errors when prefetching and only surface them when we actually try to use that version. Ignoring versions that are guaranteed to be incompatible would also be a good here (in the log we have a `triton>1` as a direct dependency), but often enough when prefetching we don't know enough yet to make this derivation, so we should avoid making our speculation's mistake an error for the user.

---
