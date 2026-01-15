```yaml
number: 17487
title: Why needing to build when locking?
type: issue
state: open
author: remidebette
labels:
  - question
assignees: []
created_at: 2026-01-15T16:04:58Z
updated_at: 2026-01-15T16:42:03Z
url: https://github.com/astral-sh/uv/issues/17487
synced_at: 2026-01-15T16:50:27Z
```

# Why needing to build when locking?

---

_@remidebette_

### Question

Hi, 

I am trying to use uv with a package (`causal-conv1d`) that has no wheels and is known not to build without nvidia cuda and related software on your machine (`nvcc` in the error below)

I work with Mac, which does not typically come with an Nvidia GPU

Now the workflow that I would envision doing is:
* lock my libraries on local with `uv lock`, potentially with this lib as optional dependency so that I can still execute some CPU-only scripts and linting on local
* build a docker image in a CI with `uv sync --frozen`
* execute on a server with the proper GPU

But it seems I cannot uv lock without building this lib, is it by design?

pyproject.toml
```toml
[project]
name = "testuv"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.11"
dependencies = [
    "boto3>=1.42.28", # Only this works fine
    "causal-conv1d==1.5.3.post1"   # This breaks uv lock

]
```

```sh
uv lock
  × Failed to build `causal-conv1d==1.5.3.post1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]


      torch.__version__  = 2.9.1



      [stderr]
      /Users/remi/.cache/uv/builds-v0/.tmppppoU9/lib/python3.11/site-packages/torch/_subclasses/functional_tensor.py:279:
      UserWarning: Failed to initialize NumPy: No module named 'numpy' (Triggered internally at
      /Users/runner/work/pytorch/pytorch/pytorch/torch/csrc/utils/tensor_numpy.cpp:84.)
        cpu = _conversion_method_template(device=torch.device("cpu"))
      <string>:119: UserWarning: causal_conv1d was requested, but nvcc was not found.  Are you sure your environment has nvcc available?
      If you're installing within a container from https://hub.docker.com/r/pytorch/pytorch, only images whose names contain 'devel' will
      provide nvcc.
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "/Users/remi/.cache/uv/builds-v0/.tmppppoU9/lib/python3.11/site-packages/setuptools/build_meta.py", line 331, in
      get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/remi/.cache/uv/builds-v0/.tmppppoU9/lib/python3.11/site-packages/setuptools/build_meta.py", line 301, in
      _get_build_requires
          self.run_setup()
        File "/Users/remi/.cache/uv/builds-v0/.tmppppoU9/lib/python3.11/site-packages/setuptools/build_meta.py", line 317, in run_setup
          exec(code, locals())
        File "<string>", line 176, in <module>
      NameError: name 'bare_metal_version' is not defined

      hint: This usually indicates a problem with the package or the build environment.
  help: `causal-conv1d` (v1.5.3.post1) was included because `testuv` (v0.1.0) depends on `causal-conv1d==1.5.3.post1`
```

No-build does not help
```sh
uv lock --no-build
  × No solution found when resolving dependencies:
  ╰─▶ Because causal-conv1d==1.5.3.post1 has no usable wheels and your project depends on causal-conv1d==1.5.3.post1, we can conclude that
      your project's requirements are unsatisfiable.

      hint: Wheels are required for `causal-conv1d` because building from source is disabled for all packages (i.e., with `--no-build`)
```

### Platform

macOs

### Version

uv 0.9.25 (Homebrew 2026-01-13)

---

_Label `question` added by @remidebette on 2026-01-15 16:04_

---

_Comment by @konstin on 2026-01-15 16:42_

To build a lockfile, uv needs to know the dependencies of causal-conv1d. There's several mechanisms that uv supports for getting static metadata from a source distribution, or at least not build everything, which causal-conv1d doesn't support:

* Static dependencies in `PKG-INFO`: They are explicitly marked as dynamic. <https://inspector.pypi.io/project/causal-conv1d/1.6.0/packages/db/df/63a384c49743b9fc8fec4c05dbd0b515e1c1c2b07e4559acc4fc37c69223/causal_conv1d-1.6.0.tar.gz/causal_conv1d-1.6.0/PKG-INFO#line.25>
* Static dependencies in `pyproject.toml`: Not present. https://inspector.pypi.io/project/causal-conv1d/1.6.0/packages/db/df/63a384c49743b9fc8fec4c05dbd0b515e1c1c2b07e4559acc4fc37c69223/causal_conv1d-1.6.0.tar.gz/causal_conv1d-1.6.0/pyproject.toml
* Implement `prepare_metadata_for_build_wheel`: setuptools or the way it uses setuptools doesn't seem to support that.

From skimming the project, this could be solved by porting these entries to `pyproject.toml` https://github.com/Dao-AILab/causal-conv1d/blob/da6dbaa9fd5a919967f14d3fd031da1288ad5025/setup.py#L387-L390


---
