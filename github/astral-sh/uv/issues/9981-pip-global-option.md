---
number: 9981
title: pip --global-option
type: issue
state: closed
author: bdols
labels:
  - question
assignees: []
created_at: 2024-12-17T19:27:17Z
updated_at: 2025-05-20T19:01:14Z
url: https://github.com/astral-sh/uv/issues/9981
synced_at: 2026-01-07T13:12:18-06:00
---

# pip --global-option

---

_Issue opened by @bdols on 2024-12-17 19:27_

is it possible to supply "global-options" to the build step?

I'm looking at a project that is doing the following:
CC="cc -mavx2" pip install pillow-simd==9.0.0.post3   --global-option="build_ext"    --global-option="--enable-zlib"    --global-option="--enable-jpeg"  --global-option="--enable-tiff"

If I "uv add pillow-simd==9.0.0.post3", I can see the _imaging.so doesn't contain the jpeg symbols


---

_Comment by @zanieb on 2024-12-17 19:34_

That option is setuptools specific and is being deprecated, see

- https://github.com/pypa/pip/issues/11859

I think the idea is to use `--config-settings` instead? But honestly I'm confused about the state of `setuptools` behaviors... e.g

- https://github.com/pypa/setuptools/issues/2491

---

_Comment by @bdols on 2024-12-18 00:10_

with setuptools 75.6.0, I'm able to, at least, pass these options, like so:
CC="cc -mavx2" uv add -vv -n pillow-simd==9.0.0.post3 -C--build-option="build_ext" -C--build-option="--enable-webp" -C--build-option="--enable-webpmux" -C--build-option="--enable-jpeg2000" -C--build-option="--enable-zlib" -C--build-option="--enable-jpeg"
I verified by adding -C--build-option="--enable-tiff" and the build failed because it couldn't find libtiff.

but how do I declare these options in the toml config?

---

_Comment by @Xiddoc on 2024-12-18 18:08_

I just finished wrestling `uv` for a few hours on a similar problem. I was trying to download and build the latest development version of `mypy`, then building it using the `setup.py` file (compiles it with `mypyc`, making it much faster).

The actual shell command I essentially wanted to run was:
```shell
python setup.py bdist_wheel --use-mypyc
```

And I found that the following `pyproject.toml` settings do just that:
```toml
[tool.uv]
config-settings = { --build-option = "--use-mypyc" }
```

Hopefully this is actually the solution to the problem, and that I didn't just get lucky with my build 😅

---

_Label `question` added by @zanieb on 2024-12-18 18:25_

---

_Closed by @zanieb on 2025-01-06 20:16_

---

_Comment by @brainfo on 2025-05-20 19:01_

Hi, anyone has exprience in installing apex?

They have building https://github.com/NVIDIA/apex:

```
git clone https://github.com/NVIDIA/apex
cd apex
# if pip >= 23.1 (ref: https://pip.pypa.io/en/stable/news/#v23-1) which supports multiple `--config-settings` with the same key... 
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --config-settings "--build-option=--cpp_ext" --config-settings "--build-option=--cuda_ext" ./
# otherwise
pip install -v --disable-pip-version-check --no-cache-dir --no-build-isolation --global-option="--cpp_ext" --global-option="--cuda_ext" ./
```

I am using uv 0.6.12. how to specify --config-settings or  --global-option?

---
