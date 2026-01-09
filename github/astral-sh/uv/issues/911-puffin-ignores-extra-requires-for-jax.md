---
number: 911
title: Puffin ignores extra requires for jax
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-01-14T14:29:04Z
updated_at: 2024-01-14T17:16:56Z
url: https://github.com/astral-sh/uv/issues/911
synced_at: 2026-01-07T13:12:16-06:00
---

# Puffin ignores extra requires for jax

---

_Issue opened by @konstin on 2024-01-14 14:29_

`jax==0.4.23` has an extra `cuda12_pip`, that should pull in `jaxlib` and other libraries, but resolving  `jax[cuda12_pip]==0.4.23` will only select the 4 default requirements.

```
Provides-Extra: cuda12_pip
Requires-Dist: jaxlib ==0.4.23+cuda12.cudnn89 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cublas-cu12 >=12.2.5.6 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cuda-cupti-cu12 >=12.2.142 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cuda-nvcc-cu12 >=12.2.140 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cuda-runtime-cu12 >=12.2.140 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cudnn-cu12 >=8.9 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cufft-cu12 >=11.0.8.103 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cusolver-cu12 >=11.5.2 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-cusparse-cu12 >=12.1.2.141 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-nccl-cu12 >=2.18.3 ; extra == 'cuda12_pip'
Requires-Dist: nvidia-nvjitlink-cu12 >=12.2 ; extra == 'cuda12_pip'
```

```shell
$ virtualenv -p 3.10 --clear .venv
$ cargo run -q -- pip-install "jax[cuda12_pip]==0.4.23"
Resolved 5 packages in 976ms
Downloaded 5 packages in 4.35s
Installed 5 packages in 23ms
 + jax==0.4.23
 + ml-dtypes==0.3.2
 + numpy==1.26.3
 + opt-einsum==3.3.0
 + scipy==1.11.4
```


---

_Label `bug` added by @konstin on 2024-01-14 14:29_

---

_Comment by @charliermarsh on 2024-01-14 15:35_

Can you fix this?

---

_Comment by @charliermarsh on 2024-01-14 15:42_

Actually, I can take a look.

---

_Comment by @charliermarsh on 2024-01-14 15:45_

The issue is the extra normalization, I think? We normalize the extra to `cuda12-pip`... But we don't normalize it within the markers. Looking into it.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-14 15:46_

---

_Comment by @charliermarsh on 2024-01-14 15:49_

This is a really good bug.

---

_Referenced in [astral-sh/uv#915](../../astral-sh/uv/pulls/915.md) on 2024-01-14 16:08_

---

_Closed by @charliermarsh on 2024-01-14 17:16_

---
