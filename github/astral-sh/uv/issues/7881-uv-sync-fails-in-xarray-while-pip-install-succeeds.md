---
number: 7881
title: "`uv sync` fails in xarray, while `pip install` succeeds"
type: issue
state: closed
author: max-sixty
labels:
  - question
  - great writeup
assignees: []
created_at: 2024-10-02T22:46:09Z
updated_at: 2024-10-03T00:17:00Z
url: https://github.com/astral-sh/uv/issues/7881
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync` fails in xarray, while `pip install` succeeds

---

_Issue opened by @max-sixty on 2024-10-02 22:46_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Thanks for making `uv`! I'm trying it out for [xarray](https://github.com/pydata/xarray). We've had great success with `ruff`.

Currently this doesn't succeed:

```
 uv sync --extra=complete
Resolved 109 packages in 6ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: llvmlite==0.36.0
  Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/maximilian/.cache/uv/builds-v0/.tmpXiRhdh/lib/python3.11/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/maximilian/.cache/uv/builds-v0/.tmpXiRhdh/lib/python3.11/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/maximilian/.cache/uv/builds-v0/.tmpXiRhdh/lib/python3.11/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/maximilian/.cache/uv/builds-v0/.tmpXiRhdh/lib/python3.11/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 55, in <module>
  File "<string>", line 52, in _guard_py_ver
RuntimeError: Cannot install on Python version 3.11.10; only versions >=3.6,<3.10 are supported.
```

It's surprising that it's attempting to select an old version of `llvmlite`. When I run `pip install .[complete]` in the venv, I get a successful install, with version `0.43.0` of `llvmlite`.

Happy to debug a bit. I had a browse through https://docs.astral.sh/uv/pip/compatibility/. Is there anything I should look at / any way of understanding why `uv` doesn't succeed there? Is a dependency giving bad information that `pip` is ignoring?

Rather than me bugging you, one great model would be to print why it's choosing that package, and someone like me could open an issue on the offending dependency (but maybe not easy...)

Thanks!

---

_Comment by @charliermarsh on 2024-10-02 22:50_

Without looking, my guess is that it's something like [this](https://github.com/astral-sh/uv/issues/6281#issuecomment-2316240724), where we end up solving in a different order than pip and thus end up picking a newer version of something that leads to us using an old llvmlite.

---

_Comment by @zanieb on 2024-10-02 22:51_

`uv sync` performs [universal resolution](https://docs.astral.sh/uv/concepts/resolution/#universal-resolution) while pip does not. pip also has some different heuristics for selecting versions during resolution. Did you try `uv pip install`? That could help narrow it down.

If you `uv add 'llvmlite>0.36.0'` you'll either get an error from the resolver explaining why we had to backtrack to the older version or it'll succeed indicating it's a difference in resolver heuristics.

---

_Comment by @charliermarsh on 2024-10-02 22:52_

If you run `uv tree --invert`, you can see the following which may help:

```
llvmlite v0.36.0
└── numba v0.53.1
    ├── numbagg v0.8.2
    │   ├── xarray v0.1.0 (extra: accel)
    │   ├── xarray v0.1.0 (extra: dev)
    │   └── xarray v0.1.0 (extra: complete)
    └── sparse v0.15.4
        ├── xarray v0.1.0 (extra: dev)
        ├── xarray v0.1.0 (extra: complete)
        └── xarray v0.1.0 (extra: etc)
```

---

_Label `question` added by @zanieb on 2024-10-02 22:52_

---

_Comment by @zanieb on 2024-10-02 22:55_

Thanks for such a kindly written issue btw.

---

_Label `great writeup` added by @zanieb on 2024-10-02 22:55_

---

_Comment by @charliermarsh on 2024-10-02 22:58_

I would need to look at the `--verbose` logs to _understand_ exactly what's going on, but it's something like: we're prioritizing the most recent version of NumPy, which is then causing us to require older versions of Numba, which is then requiring us to require older versions of llvmlite. If you constrain `numpy==2.0.2`, you get the most recent Numba and llvmlite versions. (I figured this out by running `uv tree` and applying various constraints, then re-running.)

---

_Comment by @max-sixty on 2024-10-02 23:15_

Thank you very much for the fast response! You're going to put LLMs out of a job already.

Forcing the latest version of `numba` makes it work:

```diff
diff --git a/pyproject.toml b/pyproject.toml
index 4f33eff8..7d18a7d3 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -23,6 +23,7 @@ requires-python = ">=3.10"
 
 dependencies = [
   "numpy>=1.24",
+  "numba==0.60.0",
   "packaging>=23.1",
   "pandas>=2.1",
 ]
```

(Forcing the latest version of `llvmlite` doesn't)

Any idea _why_ it's selecting numba 0.53.1? That version's [setup.py file](https://github.com/numba/numba/blob/0.53.1/setup.py#L370) seems to specify a much earlier numpy version as a constraint, though possibly in a format that can't be read? (Could look more unless this is obvious to you)

The case of "no version of A supports the latest version of B, so install the version of B which a version of A supports" seems like it should be a common path; I'd have expected to find that some dependency has misspecified a constraint...

---

_Comment by @charliermarsh on 2024-10-02 23:17_

I think it's because newer versions of Numba include:

```
Requires-Dist: numpy <2.1,>=1.22
```

So if we choose the latest NumPy, we end up having to backtrack to versions of Numba that _don't_ include that upper-bound.

---

_Comment by @max-sixty on 2024-10-02 23:20_

Ah yes, OK:

```
$  rip uv.lock; uv sync --extra=complete --verbose &> uv.log

DEBUG Adding transitive dependency for numba==0.60.0: llvmlite>=0.43.0.dev0, <0.44
DEBUG Adding transitive dependency for numba==0.60.0: numpy>=1.22, <2.1
DEBUG Searching for a compatible version of numba (>=0.49, <0.60.0 | >0.60.0)
DEBUG Selecting: numba==0.59.1 [compatible] (numba-0.59.1-cp310-cp310-macosx_10_9_x86_64.whl)
```

and then no upper bound at:

```
DEBUG Adding transitive dependency for numba==0.53.1: llvmlite>=0.36.0rc1, <0.37
DEBUG Adding transitive dependency for numba==0.53.1: numpy>=1.15
DEBUG Adding transitive dependency for numba==0.53.1: setuptools*
```

Let me try and fix by giving constraints in xarray for `numba`, so we get it backtracking in `numpy` instead...

---

_Comment by @max-sixty on 2024-10-02 23:30_

OK great, this works!

```diff
diff --git a/pyproject.toml b/pyproject.toml
index 4f33eff8..c6dd762d 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -28,7 +28,7 @@ dependencies = [
 ]
 
 [project.optional-dependencies]
-accel = ["scipy", "bottleneck", "numbagg", "flox", "opt_einsum"]
+accel = ["scipy", "bottleneck", "numbagg", "numba>=0.54", "flox", "opt_einsum"]
 complete = ["xarray[accel,etc,io,parallel,viz]"]
 dev = [
   "hypothesis",
```

Thank you very much! Will push this change. 

`uv` behavior totally makes sense on reflection. To get the correct-by-what-we-really-want outcome, I think it would need do something quite complex, such as discount the requirements of older packages — i.e. with context, it's quite unlikely only versions of `numba` from before 2021 are good with `numpy` from 2024...

---

_Closed by @max-sixty on 2024-10-02 23:30_

---

_Referenced in [pydata/xarray#9572](../../pydata/xarray/pulls/9572.md) on 2024-10-02 23:33_

---

_Comment by @zanieb on 2024-10-03 00:16_

Yeah we plan to improve these heuristics in the long-term but it's going to be quite the challenge

---

_Referenced in [argilla-io/distilabel#1018](../../argilla-io/distilabel/pulls/1018.md) on 2024-10-07 09:13_

---

_Referenced in [astral-sh/uv#7965](../../astral-sh/uv/issues/7965.md) on 2024-10-08 22:46_

---

_Referenced in [astral-sh/uv#8157](../../astral-sh/uv/issues/8157.md) on 2024-10-13 13:03_

---

_Referenced in [deepset-ai/haystack#8766](../../deepset-ai/haystack/issues/8766.md) on 2025-01-24 08:41_

---
