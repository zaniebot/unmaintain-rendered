```yaml
number: 180
title: "Add support for `--find-links`"
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - wish
assignees: []
created_at: 2023-10-25T04:12:04Z
updated_at: 2024-01-15T03:06:44Z
url: https://github.com/astral-sh/uv/issues/180
synced_at: 2026-01-12T15:58:22Z
```

# Add support for `--find-links`

---

_@charliermarsh_

We should at least support this for local directories, I think. I doubt we need to support this for HTML pages?

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-25 04:12_

---

_Label `enhancement` added by @charliermarsh on 2023-10-25 04:12_

---

_Label `wish` added by @charliermarsh on 2023-10-25 04:12_

---

_Comment by @konstin on 2023-12-26 14:56_

The [jax CUDA installation](https://jax.readthedocs.io/en/latest/installation.html#pip-installation-gpu-cuda-installed-via-pip-easier) says:

```shell
pip install --upgrade "jax[cuda12_pip]" -f https://storage.googleapis.com/jax-releases/jax_cuda_releases.html
```

We don't support find-links and the page doesn't work as an index, the format is slightly different:

```console
$ cargo run --bin puffin -q -- pip-install --index-url https://storage.googleapis.com/jax-releases/jax_cuda_releases.html jaxlib
error: Package `jaxlib` was not found in the registry.
```

I don't know if we want to or should support that, i'm filing this because jax is an import ML libraries.

---

_Comment by @charliermarsh on 2024-01-15 03:06_

I think this is done @konstin! Nice!

---

_Closed by @charliermarsh on 2024-01-15 03:06_

---
