```yaml
number: 2302
title: A bit too strict pyproject.toml validation for uv 0.1.16
type: issue
state: closed
author: potiuk
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-03-08T15:34:32Z
updated_at: 2024-03-08T19:53:50Z
url: https://github.com/astral-sh/uv/issues/2302
synced_at: 2026-01-12T15:58:37Z
```

# A bit too strict pyproject.toml validation for uv 0.1.16

---

_@potiuk_

## Problem description

The uv 0.1.16 started to do quite stronger validation of pyproject.toml  (IMHO a bit too strong) and I think it's not good as it excludes projects that are using things on a bleeding edge. This covers  for example extensions of `pyproject.toml` that are not yet accepted, but already supported by popular tools. This is especially important if the features in pyproject.toml are related to build backend not frontend, because it's quite safe to assume by projects that they can add the features if they know the backend they are using supports it.

## What happened

When we try to install Airflow in editable mode inside of our image with uv 0.1.16 we get this error (all works fine with uv 0.1.15) - it's the same error with `pip uninstall` and `pip install`:

```
  #58 0.485 + xargs uv pip uninstall --python /usr/local/bin/python
  #58 0.847 error: Querying Python at `/usr/local/bin/python3` failed with status exit status: 1 with exit status: 1
  #58 0.847 --- stdout:
  #58 0.847 
  #58 0.847 --- stderr:
  #58 0.847 Traceback (most recent call last):
  #58 0.847   File "<string>", line 404, in <module>
  #58 0.847   File "<string>", line 380, in get_scheme
  #58 0.847   File "<string>", line 330, in get_distutils_scheme
  #58 0.847   File "/usr/local/lib/python3.8/site-packages/setuptools/dist.py", line 627, in parse_config_files
  #58 0.847     pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
  #58 0.847   File "/usr/local/lib/python3.8/site-packages/setuptools/config/pyprojecttoml.py", line 67, in apply_configuration
  #58 0.847     config = read_configuration(filepath, True, ignore_option_errors, dist)
  #58 0.847   File "/usr/local/lib/python3.8/site-packages/setuptools/config/pyprojecttoml.py", line 128, in read_configuration
  #58 0.847     validate(subset, filepath)
  #58 0.847   File "/usr/local/lib/python3.8/site-packages/setuptools/config/pyprojecttoml.py", line 56, in validate
  #58 0.847     raise ValueError(f"{error}\n{summary}") from None
  #58 0.847 ValueError: invalid pyproject.toml config: `project`.
  #58 0.847 configuration error: `project` must not contain {'license-files'} properties
  #58 0.847 ---
  #58 0.853 + true
  #58 0.853 + set +x
  #58 0.853 
  #58 0.853 Installing all packages in eager upgrade mode. Installation method: .
  #58 0.853 
  #58 0.854 + uv pip install --python /usr/local/bin/python --upgrade --resolution highest --editable '.[devel-ci]'
  #58 1.162 error: Querying Python at `/usr/local/bin/python` failed with status exit status: 1 with exit status: 1
  #58 1.162 --- stdout:
  #58 1.162 
  #58 1.162 --- stderr:
  #58 1.162 Traceback (most recent call last):
  #58 1.162   File "<string>", line 404, in <module>
  #58 1.162   File "<string>", line 380, in get_scheme
  #58 1.162   File "<string>", line 330, in get_distutils_scheme
  #58 1.162   File "/usr/local/lib/python3.8/site-packages/setuptools/dist.py", line 627, in parse_config_files
  #58 1.162     pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
  #58 1.162   File "/usr/local/lib/python3.8/site-packages/setuptools/config/pyprojecttoml.py", line 67, in apply_configuration
  #58 1.162     config = read_configuration(filepath, True, ignore_option_errors, dist)
  #58 1.162   File "/usr/local/lib/python3.8/site-packages/setuptools/config/pyprojecttoml.py", line 128, in read_configuration
  #58 1.162     validate(subset, filepath)
  #58 1.162   File "/usr/local/lib/python3.8/site-packages/setuptools/config/pyprojecttoml.py", line 56, in validate
  #58 1.162     raise ValueError(f"{error}\n{summary}") from None
  #58 1.162 ValueError: invalid pyproject.toml config: `project`.
  #58 1.162 configuration error: `project` must not contain {'license-files'} properties
```

Example failure in CI here: https://github.com/apache/airflow/actions/runs/8204894763/job/22440571526?pr=37995#step:6:1817

## Context 

We have an example of that in Apache Airflow. We are currently using `license-files` keys and `.glob` pattern in it coming from (still Draft) https://peps.python.org/pep-0639/ - and the license-files keys is nicely supported by `hatchling` 1.21.1 which we use as Apache Airflow build backend. This allows us to easily add multiple licences to our sdist/wheel metadata - both Apache Licence 2.0 and 3rd-partylicences of projects that we partially vendored-in. This is a legal requirement for us to add the licences when we redistribute the software and being able to do it using dedicated mechanism for that (even if it is not yet fully accepted) is pretty useful.

Here is an extract (important things only) from our pyproject.toml:

```pyproject.toml

[build-system]
requires = [
    ...
    "hatchling==1.21.1",
    ...
]
build-backend = "hatchling.build"

[project]
....
license-files.globs = ["LICENSE", "3rd-party-licenses/*.txt"]
```

I think there are other projects that are using some upcoming features - even knowing that they might get rejected or changed, so I think better approach is to soften the validation - at least to allow some new unknown keys to appear there - maybe raise a warning and allow to silence them with a flag for those who are aware of this limitation and use `uv` for their CI/development tooling at least.


---

_Comment by @charliermarsh on 2024-03-08 15:35_

Oh hmm, I don't think this was intentional (from my perspective). Let me take a look...

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-08 15:35_

---

_Comment by @charliermarsh on 2024-03-08 15:35_

Seems like it's because we're now calling `get_distutils_scheme` internally. I wonder how / why this works in pip.

---

_Label `bug` added by @charliermarsh on 2024-03-08 15:38_

---

_Label `compatibility` added by @charliermarsh on 2024-03-08 15:38_

---

_Comment by @potiuk on 2024-03-08 15:41_

> Seems like it's because we're now calling get_distutils_scheme internally. I wonder how / why this works in pip.

It works for sure with lates version of `pip`. In our CI we switched most of our CI / PROD builds to `uv` for speed, but we just have a `--use-uv/--no-use-uv` that we can swap and PROD image is built in one variant with latest `pip` so I am quite sure it works :)


---

_Comment by @charliermarsh on 2024-03-08 15:43_

Awesome :) I'll take a look!

---

_Comment by @notatallshaw on 2024-03-08 18:32_

This is weird, the error is coming from setuptools, both uv and pip are making the same `Distribution(dist_args).parse_config_files()`. I don't see anything obvious on the setuptools side that would cause this issue, looking through the changelog, prs, and issues.

It would be helpful, from both uv and pip, if on a build error it printed out the build environment, as otherwise one has to guess what happened on the build resolution. Maybe it already does that in verbose mode?

---

_Comment by @notatallshaw on 2024-03-08 18:35_

One difference I do notice actually, uv is passing in `{}` for `dist_args`, where as pip is passing in `{"name": dist_name}` and adding `dist_args["script_args"] = ["--no-user-cfg"]` for isolated environments.

---

_Comment by @charliermarsh on 2024-03-08 18:37_

My guess is that something happens in pip to prevent it from reading the config file in that case (it looks like it falls back to cwd), but not sure what. I’m probably going to disable it for us too.

---

_Comment by @charliermarsh on 2024-03-08 18:38_

Yeah but I think pip has isolated set to false by default, I could be mistaken.

---

_Comment by @notatallshaw on 2024-03-08 18:44_

I don't see any exception handeling in pip code for that, and what's weird is setuptools has supported `license-files` for years. 

If this issue is still open by this evening I'll see if I can reproduce it on uv side and put some breakpoints in the pip code and follow what's happening, as it's very difficult to statically analyze by glancing over it.

---

_Comment by @charliermarsh on 2024-03-08 18:50_

Thanks, I'll try to repro too a little later.

---

_Comment by @charliermarsh on 2024-03-08 19:03_

I think we may ultimately be calling different validation functions, we should be calling the `dist.py` in `distutils`, not in `setuptools`... Is there a shim happening somewhere?

---

_Comment by @charliermarsh on 2024-03-08 19:13_

Yeah, ok, I think it has to do with this:

```python
def warn_distutils_present():
    if 'distutils' not in sys.modules:
        return
    if is_pypy and sys.version_info < (3, 7):
        # PyPy for 3.6 unconditionally imports distutils, so bypass the warning
        # https://foss.heptapod.net/pypy/pypy/-/blob/be829135bc0d758997b3566062999ee8b23872b4/lib-python/3/site.py#L250
        return
    import warnings

    warnings.warn(
        "Distutils was imported before Setuptools, but importing Setuptools "
        "also replaces the `distutils` module in `sys.modules`. This may lead "
        "to undesirable behaviors or errors. To avoid these issues, avoid "
        "using distutils directly, ensure that setuptools is installed in the "
        "traditional way (e.g. not an editable install), and/or make sure "
        "that setuptools is always imported before distutils."
    )
```

---

_Comment by @charliermarsh on 2024-03-08 19:26_

Confirmed, our imports are getting the shimmed `setuptools` version:

```
  × Querying Python at `/opt/homebrew/bin/python3.8` failed with status exit status: 1 with exit status: 1
  │ --- stdout:
  │ get_distutils_scheme: <class 'setuptools.dist.Distribution'>
  │ get_virtualenv: /opt/homebrew/lib/python3.8/site-packages/setuptools/_distutils/dist.py
  │ {"markers": {"implementation_name": "cpython", "implementation_version": "3.8.18", "os_name": "posix", "platform_machine": "arm64", "platform_python_implementation": "CPython", "platform_release": "23.2.0", "platform_system":
  │ "Darwin", "platform_version": "Darwin Kernel Version 23.2.0: Wed Nov 15 21:54:05 PST 2023; root:xnu-10002.61.3~2/RELEASE_ARM64_T6031", "python_full_version": "3.8.18", "python_version": "3.8", "sys_platform":
  │ "darwin"}, "base_prefix": "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8", "base_exec_prefix": "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8",
  │ "prefix": "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8", "base_executable": "/opt/homebrew/bin/python3.8", "sys_executable": "/opt/homebrew/opt/python@3.8/bin/python3.8", "stdlib":
  │ "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8/lib/python3.8", "scheme": {"platlib": "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8/lib/python3.8/site-packages",
  │ "purelib": "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8/lib/python3.8/site-packages", "include":
  │ "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8/include/python3.8", "scripts": "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8/bin", "data":
  │ "/opt/homebrew/Cellar/python@3.8/3.8.18_2/Frameworks/Python.framework/Versions/3.8"}, "virtualenv": {"purelib": "lib/python3.8/site-packages", "platlib": "lib/python3.8/site-packages", "include": "include/site/python3.8", "scripts":
  │ "bin", "data": ""}}
  │ --- stderr:
  │ Traceback (most recent call last):
  │   File "<string>", line 414, in <module>
  │ ZeroDivisionError: division by zero
  │ ---
```

---

_Closed by @charliermarsh on 2024-03-08 19:53_

---
