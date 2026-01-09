---
number: 12448
title: Install stopped working for django-modelclone for all uv versions - Setuptools update?
type: issue
state: closed
author: inoa-jboliveira
labels:
  - bug
  - duplicate
assignees: []
created_at: 2025-03-24T19:49:05Z
updated_at: 2025-03-24T20:11:22Z
url: https://github.com/astral-sh/uv/issues/12448
synced_at: 2026-01-07T13:12:18-06:00
---

# Install stopped working for django-modelclone for all uv versions - Setuptools update?

---

_Issue opened by @inoa-jboliveira on 2025-03-24 19:49_

### Summary

I had this library installing correctly for ages, but I believe something happened today (???) that it is no longer installable via uv. I tried several different uv versions ranging from 0.5.21 to latest 0.6.9 and they all behave the same.

The package `django-modelclone` hasn't been updated since 2018 and uses a deprecated functionality of setuptools which is error referring to.

What baffles me is that a couple days ago, it was installing fine. I did try changing the setuptools version inside the venv, but it might be that uv is not picking up the version from the env, but the latest version.

Just 5 hours ago setuptools launched a major version: 78.0.1 https://pypi.org/project/setuptools/#history

If that is the case, it is quite unsafe to use a version of setuptools not locked in the environment.

Repro:
```
uv init foo -p 3.9
cd foo
uv add django-modelclone
```

Output:
```python
Using CPython 3.9.20
Creating virtual environment at: .venv
  × Failed to build `django-modelclone==0.7.1`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/build_meta.py", line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/build_meta.py", line 304, in _get_build_requires
          self.run_setup()
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/build_meta.py", line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/build_meta.py", line 320, in run_setup
          exec(code, locals())
        File "<string>", line 4, in <module>
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/_distutils/core.py", line 160, in setup
          dist.parse_config_files()
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/_virtualenv.py", line 20, in parse_config_files
          result = old_parse_config_files(self, *args, **kwargs)
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/dist.py", line 730, in parse_config_files
          self._parse_config_files(filenames=inifiles)
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/dist.py", line 599, in _parse_config_files
          opt = self._enforce_underscore(opt, section)
        File ".cache/uv/builds-v0/.tmpt0Pz6X/lib/python3.9/site-packages/setuptools/dist.py", line 629, in _enforce_underscore
          raise InvalidConfigError(
      setuptools.errors.InvalidConfigError: Invalid dash-separated key 'description-file' in 'metadata' (setup.cfg), please use the underscore name 'description_file' instead.

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```




### Platform

Linux, Windows (I tested both)

### Version

0.6.9

### Python version

3.9

---

_Label `bug` added by @inoa-jboliveira on 2025-03-24 19:49_

---

_Comment by @zanieb on 2025-03-24 19:50_

See #12440 

---

_Label `duplicate` added by @zanieb on 2025-03-24 19:50_

---

_Comment by @zanieb on 2025-03-24 19:51_

> If that is the case, it is quite unsafe to use a version of setuptools not locked in the environment.

There is no standard to do otherwise, unfortunately. We're working on pushing the ecosystem forward here, but that is the current state.

See #12446 or use `--no-build-isolation`.

---

_Comment by @inoa-jboliveira on 2025-03-24 20:10_

Thank you for the reply.

Closed as duplicate.

---

_Closed by @inoa-jboliveira on 2025-03-24 20:10_

---
