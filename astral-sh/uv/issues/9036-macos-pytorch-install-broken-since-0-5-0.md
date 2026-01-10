---
number: 9036
title: macos pytorch install broken since 0.5.0
type: issue
state: closed
author: vvuk
labels: []
assignees: []
created_at: 2024-11-12T01:26:54Z
updated_at: 2024-11-12T22:13:42Z
url: https://github.com/astral-sh/uv/issues/9036
synced_at: 2026-01-10T01:24:36Z
---

# macos pytorch install broken since 0.5.0

---

_Issue opened by @vvuk on 2024-11-12 01:26_

Given workspace pyproject.toml:

```
[tool.uv.sources]
torch = [
  { index = "pytorch-cu124", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-cpu", marker = "platform_system == 'Darwin'" },
]
[[tool.uv.index]]
name = "pytorch-cpu"
url = ...
explicit = true
# similar for cu124
```

And a workspace member project's pyproject:

```
dependencies = [
  "torch==2.5.1+cu124 ; platform_system != 'Darwin'",
  "torch==2.5.1 ; platform_system == 'Darwin'",
]
```

On MacOS, with 0.5.0 (and 0.5.1), the following error occurs:

```
error: Distribution `torch==2.5.1+cpu @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

In that registry, the following files exist (I've removed all but the python 3.12 ones)
```
torch-2.5.1+cpu-cp312-cp312-linux_x86_64.whl
torch-2.5.1+cpu-cp312-cp312-win_amd64.whl
torch-2.5.1-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
torch-2.5.1-cp312-none-macosx_11_0_arm64.whl
```

Note that x64 linux and windows have `+cpu`, whereas the macOS wheels do not. Also, in case it matters, the `+cpu` wheels come before the macos one in the registry listing. This worked fine with 0.4, so I'm guessing this broke with the local version work from @ericmarkmartin. 

(Man, pytorch really is the gift that keeps on giving... ;)


---

_Comment by @charliermarsh on 2024-11-12 01:33_

I think this is actually _right_, but you might need to do `torch==2.5.1, !=2.5.1+cpu ; platform_system == 'Darwin'`?

---

_Comment by @vvuk on 2024-11-12 01:52_

That does indeed fix it; I couldn't figure out how to constrain to "without local version", makes perfect sense to just constrain to "not the +cpu" one. But man this is such a mess in pytorch. If this resolution behaviour is correct, then I guess pytorch should fix how they're publishing packages? Either move the things without +cpu to another registry dir, or make them all have +cpu... Gah. 

Thanks for the fix suggestion. Works on both 0.4.30 and 0.5.1.

---

_Closed by @vvuk on 2024-11-12 01:52_

---

_Comment by @charliermarsh on 2024-11-12 01:54_

Yeah I'm really confused about why (above) the macOS versions _don't_ have +cpu but the others do. Presumedly they're all CPU distributions? It's annoying because this has come up before :(

---

_Comment by @vvuk on 2024-11-12 01:57_

I think it's because on macos, there are no multiple-local-version wheels; there's just the single torch package. There is support for the Metal Performance Shaders hw accel framework, but it's all integrated into a single package. So I guess they omitted the +cpu because it's technically not just +cpu. Or something. :/ 

---

_Comment by @ericmarkmartin on 2024-11-12 17:18_

> I think this is actually _right_, but you might need to do `torch==2.5.1, !=2.5.1+cpu ; platform_system == 'Darwin'`?

`torch===2.5.1, platform_system == 'Darwin'` should work as well I think

---

_Comment by @daeh on 2024-11-12 22:09_

On my setup, it needs `"torch==2.5.1, !=2.5.1+cpu ; platform_system == 'Darwin'"`. 

`"torch==2.5.1 ; platform_system == 'Darwin'"` fails:
```
error: Distribution `torch==2.5.1+cpu @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

(`"torch===2.5.1, platform_system == 'Darwin'"` offends PEP's formatting rules)


MacOS 14.7.1 ARM (M2 Pro)
Python 3.12
uv 0.5.1 (Homebrew 2024-11-08)

---

_Referenced in [astral-sh/uv#8930](../../astral-sh/uv/issues/8930.md) on 2024-11-13 15:49_

---
