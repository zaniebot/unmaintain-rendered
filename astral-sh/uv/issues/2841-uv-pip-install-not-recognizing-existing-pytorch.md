---
number: 2841
title: "`uv pip install` not recognizing existing Pytorch installed by `conda` in Pytorch docker image"
type: issue
state: closed
author: StellarStorm
labels:
  - needs-mre
assignees: []
created_at: 2024-04-05T21:36:40Z
updated_at: 2024-05-07T22:29:42Z
url: https://github.com/astral-sh/uv/issues/2841
synced_at: 2026-01-10T01:23:22Z
---

# `uv pip install` not recognizing existing Pytorch installed by `conda` in Pytorch docker image

---

_Issue opened by @StellarStorm on 2024-04-05 21:36_

Good afternoon!
I've just discovered `uv` and am finding it quite helpful - thanks for all the work on it!

I'm building a Docker image based on the [pytorch 2.2.2 Docker image](https://hub.docker.com/layers/pytorch/pytorch/2.2.2-cuda12.1-cudnn8-runtime/images/sha256-923f687790bec78081c357e71dcd5dcef80b0cc00f6c34484902a5e83362c854?context=explore) and I've found something I don't expect. When trying to install certain packages that depend upon pytorch, `uv` wants to re-install torch. This is despite the Docker image being built upstream with torch already present and installed from conda.

For example, this will add torch==2.2.2, despite it already being present on the system:

`CONDA_PREFIX=/opt/conda uv pip install monai[all]  `

I've also tried with the `--system` flag instead of setting `CONDA_PREFIX`, but get the same result.

Looking at the output from `uv pip list`, there's a difference in what is sees vs. the system-wide `pip`:

```bash
root@colima-amd64:/workspace# CONDA_PREFIX=/opt/conda uv pip list | grep torch
torchelastic              0.2.2
root@colima-amd64:/workspace# pip list | grep torch
torch                     2.2.2
torchaudio                2.2.2
torchelastic              0.2.2
torchvision               0.17.2
```
This is my uv/pip version information:
```bash
root@colima-amd64:/workspace# uv --version
uv 0.1.29
root@colima-amd64:/workspace# pip --version
pip 24.0 from /opt/conda/lib/python3.10/site-packages/pip (python 3.10)
```
The Docker image is based upon Ubuntu 22.04. Python is installed by the upstream Pytorch Docker devs with conda. I installed uv with pip.

```bash
root@colima-amd64:/workspace# conda --version
conda 23.9.0

root@colima-amd64:/workspace# which pip
/opt/conda/bin/pip
root@colima-amd64:/workspace# which conda
/opt/conda/bin/conda
root@colima-amd64:/workspace# which uv
/opt/conda/bin/uv
```

Thank you!

---

_Comment by @charliermarsh on 2024-04-07 00:29_

That's interesting... Are you able to include a minimal Dockerfile that reproduces the issue? Happy to look into it.

---

_Label `needs-mre` added by @charliermarsh on 2024-04-07 00:29_

---

_Comment by @StellarStorm on 2024-04-09 18:23_

Sure - here's the minimal dockerfile (can't attach for some reason):
```text
FROM pytorch/pytorch:2.2.2-cuda12.1-cudnn8-runtime

RUN pip install uv \
    && CONDA_PREFIX=/opt/conda uv pip install monai --verbose
```

Saved to "Dockerfile" and built with `docker buildx build --platform linux/amd64 --rm --network=host --progress=plain -t uv_test .`

Thanks!

---

_Comment by @StellarStorm on 2024-04-11 13:54_

@charliermarsh I suspect #2928  and #2991 may be related

---

_Referenced in [astral-sh/uv#2928](../../astral-sh/uv/issues/2928.md) on 2024-04-13 16:42_

---

_Comment by @samypr100 on 2024-04-13 18:44_

See https://github.com/astral-sh/uv/issues/2928#issuecomment-2053727555 that explains this issue.

---

_Referenced in [astral-sh/uv#3341](../../astral-sh/uv/issues/3341.md) on 2024-05-03 03:40_

---

_Referenced in [astral-sh/uv#3380](../../astral-sh/uv/pulls/3380.md) on 2024-05-06 00:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-06 01:04_

---

_Closed by @charliermarsh on 2024-05-06 13:47_

---

_Closed by @charliermarsh on 2024-05-06 13:47_

---

_Comment by @StellarStorm on 2024-05-07 22:29_

Seems to work great now with uv 0.1.40, thanks! 🎉

---
