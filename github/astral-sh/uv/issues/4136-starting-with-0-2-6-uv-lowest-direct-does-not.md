---
number: 4136
title: "Starting with 0.2.6+ uv `lowest-direct` does not respect all limits that are specified conditionally"
type: issue
state: closed
author: potiuk
labels:
  - bug
assignees: []
created_at: 2024-06-07T16:11:25Z
updated_at: 2024-06-08T12:46:16Z
url: https://github.com/astral-sh/uv/issues/4136
synced_at: 2026-01-07T13:12:17-06:00
---

# Starting with 0.2.6+ uv `lowest-direct` does not respect all limits that are specified conditionally

---

_Issue opened by @potiuk on 2024-06-07 16:11_

As of recently, we are using uv to generate lowest-direct dependencies in airflow. We do it at scale (we generate separate lowest-direct dependencies for every provider we have - we have 90 of them. And it works very well, or rather worked very well till 0.2.5.  Something changed in 0.2.6 that causes uv to stop respecting at least some limits specified to be installed and attempt to install much lower versions of dependencies than the limits are.

An example of this can be seen here https://github.com/apache/airflow/actions/runs/9417755856/job/25943952035?pr=40110#step:7:43011

Due to amount of logs to be processed by github actions UI, it needs a bit of patience to see it, but generally speaking it boils down to this error (when uv 0.2.6+ is used (same behaviour is with latest 0.2.9 and versions between):

```
uv pip install --python /usr/local/bin/python --resolution lowest-direct --upgrade --editable '.[google]'

error: Failed to download and build `pandas==0.1`
  Caused by: Failed to build: `pandas==0.1`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/tmp/.tmpSYnKSk/.tmpJOVZVf/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
  File "/tmp/.tmpSYnKSk/.tmpJOVZVf/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/tmp/.tmpSYnKSk/.tmpJOVZVf/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/tmp/.tmpSYnKSk/.tmpJOVZVf/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 5, in <module>
ModuleNotFoundError: No module named 'numpy'
```

This works perfectly fine 


The thing is that it seems that uv `0.2.5` - works correctly. For Python 3.8 it will do this and this is pretty much expected.


```
- pandas==2.0.3
 + pandas==1.5.3
```

The problem might be connecte with the way how our (**dynamic** !) dependencies specify this for google `extra` 
https://github.com/apache/airflow/blob/main/generated/provider_dependencies.json#L638  - it's dynamically generated from that .json file by our hatch_build.py hook but it boils down to 

```
pandas>=1.5.3,<2.2;python_version<"3.12"
pandas>=2.1.1,<2.2;python_version>="3.12"
```

So - as expected - uv should not even attempt  to try to build pandas 0.1 because for Python 3.8 it is >=1.5.3.

It's relatively easy to reproduce using Airflow CI dev tooling/image (due to the nature of our project - 790 dependencies using Breeze dev tooling and CI image built automatically is the easiest way to reproduce the same environment our CI has):

1) checkout airflow
2) install breeze `pipx install -e ./dev/breeze` 
3) build CI image with eager upgrade to latest dependencies (`breeze ci-image build --upgrade-to-newer-dependencies)
4) enter the image `breeze shell`
5) install the right version of uv: `pip install uv==0.2.6` or `pip install uv==0.2.5`
6) run "lowest-direct" dependency resolution: `uv pip install --python /usr/local/bin/python --resolution lowest-direct --upgrade --editable '.[google]'`


* In case of uv 0.2.5 or below it will nicely downgrade dependencies to lowest direct as expected.
* In case of uv 0.2.6 or above it will fail when attempting to compile pandas==0.1 

Something must have changed 0.2.5 -> 0.2.6 to make uv stop respecting some kinds of lower bindings (I suspect the conditional python version might have something to do with it). I also made a small experiment and added (in generated/provider_dependencies.json) additional unconditional limit `pandas>=1.5.3` effectively leading to:

```
pandas>=1.5.3
pandas>=1.5.3,<2.2;python_version<"3.12"
pandas>=2.1.1,<2.2;python_version>="3.12"
```

This one also fails in 0.2.6+ but with:

```
 error: Failed to download and build `alembic==0.1.0`
    Caused by: Failed to build: `alembic==0.1.0`
    Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
```

That suggests tha indeed the conditional python_version is the one causing uv to do way more backtracking that it should. 


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @potiuk on 2024-06-07 16:13_

cc: @notatallshaw  you might be interested in that one.

---

_Referenced in [apache/airflow#40110](../../apache/airflow/pulls/40110.md) on 2024-06-07 16:17_

---

_Comment by @charliermarsh on 2024-06-07 16:22_

Thanks, will take a look.

---

_Comment by @potiuk on 2024-06-07 17:42_

For now we limit uv to == 0.2.5 (and it indeed fixed the problems we had when uv was 0.2.9 in our CI) - so nothing super-urgent. Happy to test it once there is a candidate with a fix.

---

_Comment by @charliermarsh on 2024-06-07 17:53_

Thank you @potiuk :)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-07 20:01_

---

_Label `bug` added by @charliermarsh on 2024-06-07 20:01_

---

_Comment by @charliermarsh on 2024-06-07 20:40_

I actually suspect I may have fixed this already. Need to figure out how to test.

---

_Comment by @charliermarsh on 2024-06-07 21:03_

I see the problem but not sure how to fix it yet.

---

_Comment by @charliermarsh on 2024-06-07 21:15_

The solver actually gets to the right solution. It's the pre-fetch that fails. It's very complex... We hit:

```
Adding transitive dependency for pandas-gbq==0.7.0: pandas*
```

And then we attempt to prefetch pandas:

```
Searching for prefetch metadata for: pandas (*)
```

And because it's a direct dependency, we choose the lowest-compatible version.

At this point, we know that `pandas{python_version < '3.12'} (>=1.5.3, <2.2.0)` is a constraint, but we don't know that the _base_ package is a constraint, because we don't see that until we visit `pandas{python_version < '3.12'} (>=1.5.3, <2.2.0)`.

---

_Comment by @charliermarsh on 2024-06-07 21:21_

I know one way to fix this (add non-marker versions eagerly), but it will make https://github.com/astral-sh/uv/issues/4125 harder. So I need to look at these holistically.


---

_Comment by @charliermarsh on 2024-06-07 21:23_

There are also easier things we can do in the interim, like: avoid pre-fetching unbounded dependencies.


---

_Referenced in [astral-sh/uv#4149](../../astral-sh/uv/pulls/4149.md) on 2024-06-07 21:49_

---

_Closed by @charliermarsh on 2024-06-07 22:05_

---

_Comment by @potiuk on 2024-06-08 12:46_

Confirmed: `uv` installed from main works! 

---

_Referenced in [apache/airflow#40177](../../apache/airflow/pulls/40177.md) on 2024-06-11 14:22_

---

_Referenced in [apache/airflow#42274](../../apache/airflow/pulls/42274.md) on 2024-09-24 20:47_

---

_Referenced in [astral-sh/uv#7680](../../astral-sh/uv/issues/7680.md) on 2024-09-25 07:49_

---

_Referenced in [apache/airflow#42574](../../apache/airflow/pulls/42574.md) on 2024-09-29 19:29_

---
