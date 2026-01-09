---
number: 2268
title: Passing a directory path with uv fails, while pip succeeds 
type: issue
state: closed
author: RafailFridman
labels: []
assignees: []
created_at: 2024-03-07T09:35:46Z
updated_at: 2024-03-07T14:04:20Z
url: https://github.com/astral-sh/uv/issues/2268
synced_at: 2026-01-07T13:12:17-06:00
---

# Passing a directory path with uv fails, while pip succeeds 

---

_Issue opened by @RafailFridman on 2024-03-07 09:35_

I'm following a setup script from [here](https://github.com/oppo-us-research/SpacetimeGaussians/blob/main/script/setup.sh). One step being
`pip install thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9`
This command works with pip, while 
`uv pip install thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9` fails with this message:
```
? `thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9` looks like a local directory but was passed as a package name. Did you mean `-e thirdpa
✔ `thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9` looks like a local directory but was passed as a package name. Did you mean `-e thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9`? · yes
error: failed to read from file `thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9`
  Caused by: Is a directory (os error 21)
```

I assume I missed some flag, but I didn't find it in the help. Is this feature currently supported?

uv version is 0.1.15

---

_Comment by @charliermarsh on 2024-03-07 14:04_

Hi! Right now, we require that you include the package name, as you would in a `pyproject.toml` file, like:

```txt
gaussian_rasterization_ch9 @ thirdparty/gaussian_splatting/submodules/gaussian_rasterization_ch9
```

There's more on this here: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#direct-url-dependencies-without-package-names

---

_Closed by @charliermarsh on 2024-03-07 14:04_

---
