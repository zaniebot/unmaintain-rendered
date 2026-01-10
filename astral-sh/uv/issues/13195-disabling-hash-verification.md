---
number: 13195
title: Disabling hash verification
type: issue
state: closed
author: kevmo314
labels:
  - bug
  - external
assignees: []
created_at: 2025-04-29T13:33:59Z
updated_at: 2025-05-14T07:32:31Z
url: https://github.com/astral-sh/uv/issues/13195
synced_at: 2026-01-10T01:25:29Z
---

# Disabling hash verification

---

_Issue opened by @kevmo314 on 2025-04-29 13:33_

### Question

Hi, I have a very simple `pyproject.toml`:

```
[project]
name = "project"
version = "0.0.1"
requires-python = ">=3.13.0"
dependencies = [
  "torch>=2.6.0",
  "triton",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu128", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]
triton = { git = "https://github.com/triton-lang/triton", rev = "main" }


[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```

However when installing with `uv sync` I get:

```
$ UV_NO_VERIFY_HASHES=1 uv sync
Resolved 27 packages in 4ms
  × Failed to download `torch==2.7.0+cu128`
  ╰─▶ Hash mismatch for `torch==2.7.0+cu128`

      Expected:
        sha256:d2f69f909da5dc52113ec66a851d62079f3d52c83184cf64beebdf12ca2f705c
        sha256:58c749f52ddc9098155c77d6c74153bb13d8978fd6e1063b5d7b41d4644f5af5
        sha256:78e13c26c38ae92d6841cf9ce760d7e9d52bca3e3183de371812e84274b054dc
        sha256:3559e98be824c2b12ab807319cd61c6174d73a524c9961317de8e8a44133c5c5

      Computed:
        sha256:633f35e8b1b1f640ef5f8a98dbd84f19b548222ce7ba8f017fe47ce6badc106a
  help: `torch` (v2.7.0+cu128) was included because `project` (v0.0.1) depends on `torch`
```

Perhaps this is happening because I'm on aarch64? How do I get uv to just install the package?

### Platform

Linux 6.8.0-1013-nvidia-64k aarch64 GNU/Linux

### Version

uv 0.6.17

---

_Label `question` added by @kevmo314 on 2025-04-29 13:33_

---

_Comment by @konstin on 2025-04-29 14:32_

It looks like there are hashes missing for aarch64 in https://download.pytorch.org/whl/torch/ (no `#sha256=` on some of them), and uv currently expects either no hashes or all hashes:

```html
 <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp310-cp310-manylinux_2_28_aarch64.whl" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp310-cp310-manylinux_2_28_aarch64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp310-cp310-manylinux_2_28_x86_64.whl#sha256=ac1849553ee673dfafb44c610c60cb60a2890f0e117f43599a526cf777eb8b8c" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp310-cp310-manylinux_2_28_x86_64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp310-cp310-win_amd64.whl#sha256=c52c4b869742f00b12cb34521d1381be6119fa46244791704b00cc4a3cb06850" data-dist-info-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab" data-core-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab">torch-2.7.0+cu128-cp310-cp310-win_amd64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp311-cp311-manylinux_2_28_aarch64.whl" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp311-cp311-manylinux_2_28_aarch64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp311-cp311-manylinux_2_28_x86_64.whl#sha256=c4bbc0b4be60319ba1cefc90be9557b317f0b3c261eeceb96ca6e0343eec56bf" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp311-cp311-manylinux_2_28_x86_64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp311-cp311-win_amd64.whl#sha256=bf88f647d76d79da9556ca55df49e45aff1d66c12797886364343179dd09a36c" data-dist-info-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab" data-core-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab">torch-2.7.0+cu128-cp311-cp311-win_amd64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp312-cp312-manylinux_2_28_aarch64.whl" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp312-cp312-manylinux_2_28_aarch64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp312-cp312-manylinux_2_28_x86_64.whl#sha256=7c0f08d1c44a02abad389373dddfce75904b969a410be2f4e5109483dd3dc0ce" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp312-cp312-manylinux_2_28_x86_64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp312-cp312-win_amd64.whl#sha256=1704e5dd66c9221e4e8b6ae2d80cbf54e129571e643f5fa9ca78cc6d2096403a" data-dist-info-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab" data-core-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab">torch-2.7.0+cu128-cp312-cp312-win_amd64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp313-cp313-manylinux_2_28_aarch64.whl" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp313-cp313-manylinux_2_28_aarch64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp313-cp313-manylinux_2_28_x86_64.whl#sha256=d2f69f909da5dc52113ec66a851d62079f3d52c83184cf64beebdf12ca2f705c" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp313-cp313-manylinux_2_28_x86_64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp313-cp313-win_amd64.whl#sha256=58c749f52ddc9098155c77d6c74153bb13d8978fd6e1063b5d7b41d4644f5af5" data-dist-info-metadata="sha256=5328f06cf7c42c53b380ab25403cb75bbdc4cce7c73229da2f02c9b68476807b" data-core-metadata="sha256=5328f06cf7c42c53b380ab25403cb75bbdc4cce7c73229da2f02c9b68476807b">torch-2.7.0+cu128-cp313-cp313-win_amd64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp313-cp313t-manylinux_2_28_aarch64.whl" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp313-cp313t-manylinux_2_28_aarch64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp313-cp313t-manylinux_2_28_x86_64.whl#sha256=78e13c26c38ae92d6841cf9ce760d7e9d52bca3e3183de371812e84274b054dc" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp313-cp313t-manylinux_2_28_x86_64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp313-cp313t-win_amd64.whl#sha256=3559e98be824c2b12ab807319cd61c6174d73a524c9961317de8e8a44133c5c5" data-dist-info-metadata="sha256=5328f06cf7c42c53b380ab25403cb75bbdc4cce7c73229da2f02c9b68476807b" data-core-metadata="sha256=5328f06cf7c42c53b380ab25403cb75bbdc4cce7c73229da2f02c9b68476807b">torch-2.7.0+cu128-cp313-cp313t-win_amd64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp39-cp39-manylinux_2_28_aarch64.whl" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp39-cp39-manylinux_2_28_aarch64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp39-cp39-manylinux_2_28_x86_64.whl#sha256=f446f97b20cb070747b103fb640df941b88cb68c8d3b01538287d05d56a7e874" data-dist-info-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022" data-core-metadata="sha256=86107f52ff49e63a75c9d345374cac30140e053a12ffa728d676fee836e69022">torch-2.7.0+cu128-cp39-cp39-manylinux_2_28_x86_64.whl</a><br/>
    <a href="/whl/cu128/torch-2.7.0%2Bcu128-cp39-cp39-win_amd64.whl#sha256=8614a167d6a163273fb130f586802f3243479862b53ee2843941c10cc5761da6" data-dist-info-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab" data-core-metadata="sha256=d51356a768c4e35d5219c34e4d984071fcc0d2ab58673cbc969b75989d312fab">torch-2.7.0+cu128-cp39-cp39-win_amd64.whl</a><br/>
```

CC @charliermarsh 

---

_Referenced in [astral-sh/uv#13318](../../astral-sh/uv/issues/13318.md) on 2025-05-06 22:30_

---

_Referenced in [pytorch/pytorch#153469](../../pytorch/pytorch/issues/153469.md) on 2025-05-13 15:33_

---

_Comment by @konstin on 2025-05-13 15:34_

Reported upstream at https://github.com/pytorch/pytorch/issues/153469

---

_Comment by @atalman on 2025-05-13 16:36_

Looks like its missing from both:
https://download.pytorch.org/whl/cu128/torch/index.html
and
https://download.pytorch.org/whl/torch/index.html

This is the wheel to check: ``torch-2.7.0+cu128-cp310-cp310-manylinux_2_28_aarch64.whl``

---

_Label `question` removed by @konstin on 2025-05-14 07:30_

---

_Label `bug` added by @konstin on 2025-05-14 07:30_

---

_Label `external` added by @konstin on 2025-05-14 07:30_

---

_Comment by @konstin on 2025-05-14 07:32_

This has been fixed by the torch maintainers by adding the hash to the index.

We're still considering switching making hashing more robust to missing hashes, but for now the torch installation works afain.

---

_Closed by @konstin on 2025-05-14 07:32_

---
