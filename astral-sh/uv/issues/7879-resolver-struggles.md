---
number: 7879
title: resolver struggles
type: issue
state: closed
author: stas00
labels: []
assignees: []
created_at: 2024-10-02T19:27:18Z
updated_at: 2024-10-02T20:24:22Z
url: https://github.com/astral-sh/uv/issues/7879
synced_at: 2026-01-10T01:24:20Z
---

# resolver struggles

---

_Issue opened by @stas00 on 2024-10-02 19:27_

using `uv-0.4.18`
```
$ uv pip install tensorrt_llm~=0.12 --pre --extra-index-url https://pypi.nvidia.com
Collecting uv
  Downloading uv-0.4.18-py3-none-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (12.9 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.9/12.9 MB 63.3 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.4.18
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv
  x No solution found when resolving dependencies:
  `-> Because there is no version of nvidia-cudnn-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==9.1.0.70 and
      torch==2.4.0 depends on nvidia-cudnn-cu12{platform_machine == 'x86_64' and platform_system == 'Linux'}==9.1.0.70, we can conclude
      that torch==2.4.0 cannot be used.
      And because only the following versions of torch are available:
          torch<2.4.0a0
          torch>=2.4.0
      and tensorrt-llm>=0.12.0 depends on torch>=2.4.0a0,<=2.4.0, we can conclude that tensorrt-llm>=0.12.0 cannot be used.
      And because only the following versions of tensorrt-llm are available:
          tensorrt-llm<=0.12.0
          tensorrt-llm==0.13.0.dev2024081300
          tensorrt-llm==0.13.0.dev2024082000
          tensorrt-llm==0.13.0.dev2024082700
          tensorrt-llm==0.13.0.dev2024090300
          tensorrt-llm==0.13.0
          tensorrt-llm==0.14.0.dev2024091000
          tensorrt-llm==0.14.0.dev2024091700
          tensorrt-llm==0.14.0.dev2024092400
          tensorrt-llm==0.14.0.dev2024092401
          tensorrt-llm==0.14.0.dev2024100100
      and you require tensorrt-llm>=0.12, we can conclude that your requirements are unsatisfiable.

      hint: `tensorrt-llm` was found on https://pypi.nvidia.com/, but not at the requested version (all of:
          tensorrt-llm>0.12.0,<0.13.0.dev2024081300
          tensorrt-llm>0.13.0.dev2024081300,<0.13.0.dev2024082000
          tensorrt-llm>0.13.0.dev2024082000,<0.13.0.dev2024082700
          tensorrt-llm>0.13.0.dev2024082700,<0.13.0.dev2024090300
          tensorrt-llm>0.13.0.dev2024090300,<0.13.0
          tensorrt-llm>0.13.0,<0.14.0.dev2024091000
          tensorrt-llm>0.14.0.dev2024091000,<0.14.0.dev2024091700
          tensorrt-llm>0.14.0.dev2024091700,<0.14.0.dev2024092400
          tensorrt-llm>0.14.0.dev2024092400,<0.14.0.dev2024092401
          tensorrt-llm>0.14.0.dev2024092401,<0.14.0.dev2024100100
          tensorrt-llm>0.14.0.dev2024100100,<1.dev0
      ). A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By default, uv will only consider
      versions that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all
      indexes are equally trusted, use `--index-strategy unsafe-best-match` to consider all versions from all indexes, regardless of the
      order in which they were defined.
```

whereas `pip` installs just fine:
```
pip install tensorrt_llm~=0.12 --pre --extra-index-url https://pypi.nvidia.com
```

I'm installing into the `nvidia/cuda:12.5.1-devel-ubuntu22.04` docker image.

here is the full repro:

```
docker run -it --gpus all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 nvidia/cuda:12.5.1-devel-ubuntu22.04

apt-get update && apt-get -y install python3.10 python3-pip python-is-python3 openmpi-bin libopenmpi-dev wget git git-lfs unzip jq
export UV_SYSTEM_PYTHON=1
pip install uv
# uv pip install torch==2.4.0
uv pip install tensorrt_llm~=0.12 --pre --extra-index-url https://pypi.nvidia.com
```

if I manually do this first:
```
uv pip install torch==2.4.0
```

then the resolver works. somehow it can't figure out it can install `torch==2.4.0`


---

_Comment by @zanieb on 2024-10-02 19:36_

Did you try following the hint after the error?

> A compatible version may be available on a subsequent index (e.g., https://pypi.org/simple). By default, uv will only consider versions that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were defined.

More information in the [documentation](https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes).

---

_Comment by @stas00 on 2024-10-02 19:37_

and then it fails again doing another pip install:
```
git clone https://github.com/NVIDIA/TensorRT-Model-Optimizer/
cd TensorRT-Model-Optimizer/llm_ptq/
uv pip install -r requirements.txt
```

```
 Updated https://github.com/Dao-AILab/flash-attention.git (53a4f34)
error: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/root/.cache/uv/builds-v0/.tmpG2T258/lib/python3.10/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/root/.cache/uv/builds-v0/.tmpG2T258/lib/python3.10/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/root/.cache/uv/builds-v0/.tmpG2T258/lib/python3.10/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/root/.cache/uv/builds-v0/.tmpG2T258/lib/python3.10/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 7, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that git+https://github.com/Dao-AILab/flash-attention.git#subdirectory=csrc/rotary depends on torch, but doesn't declare it as a build dependency. If git+https://github.com/Dao-AILab/flash-attention.git#subdirectory=csrc/rotary is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```

it messes something up here with `PYTHONPATH` perhaps, since:

```
python -c "import torch"
```
works just fine.

And again switching to `pip` works fine as well.

for context: I'm trying to build a simpler and faster version of https://github.com/NVIDIA/TensorRT-Model-Optimizer/blob/main/docker/Dockerfile replacing `s/pip/uv pip/` - hence the same issue.

---

_Comment by @zanieb on 2024-10-02 19:39_

Similarly, `flash-attention` notoriously cannot be used with build isolation (see #2252). More details in [the documentation](https://docs.astral.sh/uv/pip/compatibility/#pep-517-build-isolation).

There's a dedicated hint for this too

> Caused by: This error likely indicates that git+https://github.com/Dao-AILab/flash-attention.git#subdirectory=csrc/rotary depends on torch, but doesn't declare it as a build dependency. If git+https://github.com/Dao-AILab/flash-attention.git#subdirectory=csrc/rotary is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.

---

_Comment by @stas00 on 2024-10-02 19:47_

thank you for the hints, @zanieb 

I have a feature request then: can we have a new env var that if set makes `uv` work exactly like `pip` so in any situation where `pip` succeeds `uv pip` will succeed?

I'm sort of sitting on the fence here, wanting to use `uv` for its speed, but don't have the time to constantly figure out the more precise way `uv` works and adding flags and env vars in too many places. That was the main reason I hardly every run `conda install` these days, because 99.999% of the time `pip` does the right thing out of the box, and conda was making things too restrictive and prescriptive.

---

_Comment by @zanieb on 2024-10-02 19:54_

I don't think you should need to constantly figure things out, the differences are described thoroughly in the compatibility document. I don't think we can / will provide a flag that makes uv "act exactly like pip" — we usually have strong justifications for differing on our defaults. It's an interesting suggestion though.

---

_Comment by @stas00 on 2024-10-02 20:08_

but you're `uv`'s core developer, whereas, we, users, want things to just work because we have so many more things to figure and if there is a tool that creates less friction at a cost of a slower speed I'll choose that over the much faster tool that ends up costing me a lot more time and the limited brain juice to constantly lookup docs and deal with edge cases.

Moreover I most likely will figure things out, but not everybody on my team will and they will complain and then I will need to spend even more time supporting them.

My intuition is that for a great `uv` adoption, the first step should be: exactly the same behavior as pip but faster - then let people optimize when there is an actual need to do so. 

"Keep simple things simple, and difficult things doable" or something of sorts is a good motto some projects have

---

_Comment by @charliermarsh on 2024-10-02 20:21_

For what it’s worth, that build isolation behavior you’re seeing (the second error you referenced) will almost certainly be true in future versions of pip too. They have not made the PEP 517 standard a default yet.

---

_Closed by @stas00 on 2024-10-02 20:21_

---

_Comment by @stas00 on 2024-10-02 20:24_

I think for now I will just stick to `pip` when I need to try out something new and massive like the huge dependencies of nvidia's packages and use `uv` in repetitive situations where everybody saves time and one time figuring out saves the time repeatedly on subsequent runs of the same install recipe.

---
