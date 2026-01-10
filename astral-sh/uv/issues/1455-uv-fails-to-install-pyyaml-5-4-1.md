---
number: 1455
title: "`uv` fails to install `pyyaml==5.4.1`"
type: issue
state: closed
author: sbrugman
labels:
  - bug
assignees: []
created_at: 2024-02-16T08:54:30Z
updated_at: 2025-06-27T11:11:59Z
url: https://github.com/astral-sh/uv/issues/1455
synced_at: 2026-01-10T01:23:06Z
---

# `uv` fails to install `pyyaml==5.4.1`

---

_Issue opened by @sbrugman on 2024-02-16 08:54_

`uv pip install "pyyaml<6"`

```
Resolved 1 package in 0ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: pyyaml==5.4.1
  Caused by: Failed to build: pyyaml==5.4.1
  Caused by: Build backend failed to determine extra requires with `build_wheel()`:
--- stdout:
running egg_info
writing lib3/PyYAML.egg-info/PKG-INFO
writing dependency_links to lib3/PyYAML.egg-info/dependency_links.txt
writing top-level names to lib3/PyYAML.egg-info/top_level.txt
--- stderr:
Traceback (most recent call last):
  File "<string>", line 10, in <module>
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 271, in <module>
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/__init__.py", line 103, in setup
    return distutils.core.setup(**attrs)
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/core.py", line 185, in setup
    return run_commands(dist)
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/core.py", line 201, in run_commands
    dist.run_commands()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/dist.py", line 969, in run_commands
    self.run_command(cmd)
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/dist.py", line 963, in run_command
    super().run_command(command)
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
    cmd_obj.run()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/command/egg_info.py", line 321, in run
    self.find_sources()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/command/egg_info.py", line 329, in find_sources
    mm.run()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/command/egg_info.py", line 550, in run
    self.add_defaults()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/command/egg_info.py", line 588, in add_defaults
    sdist.add_defaults(self)
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/command/sdist.py", line 102, in add_defaults
    super().add_defaults()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/command/sdist.py", line 251, in add_defaults
    self._add_defaults_ext()
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/command/sdist.py", line 336, in _add_defaults_ext
    self.filelist.extend(build_ext.get_source_files())
  File "<string>", line 201, in get_source_files
  File "/private/var/folders/ns/sj_tmscs2b10hq8jbzxvx54r0000gq/T/.tmpF4F30H/.venv/lib/python3.10/site-packages/setuptools/_distutils/cmd.py", line 107, in __getattr__
    raise AttributeError(attr)
AttributeError: cython_sources
---
```

---

`pip install "pyyaml<6"`

```
Collecting pyyaml<6
  Using cached PyYAML-5.4.1-cp310-cp310-macosx_12_0_arm64.whl
Installing collected packages: pyyaml
  Attempting uninstall: pyyaml
    Found existing installation: PyYAML 6.0
    Uninstalling PyYAML-6.0:
      Successfully uninstalled PyYAML-6.0
Successfully installed pyyaml-5.4.1
```

---

_Label `bug` added by @dhruvmanila on 2024-02-16 09:33_

---

_Comment by @BurntSushi on 2024-02-16 15:16_

I'm able to reproduce this, thank you!

---

_Assigned to @BurntSushi by @BurntSushi on 2024-02-16 15:16_

---

_Comment by @BurntSushi on 2024-02-21 14:39_

OK, so I took a deeper dive into this and I found what appears to be a pretty nasty rat's nest of build problems with PyYAML and Cython:

* https://github.com/yaml/pyyaml/issues/601
* https://github.com/cython/cython/issues/4568
* https://github.com/yaml/pyyaml/pull/731

So then I wondered, but if `pip` works, why doesn't `uv`? So I tried `pip` and it actually fails for me:

```
$ pip install "pyyaml<6"
Collecting pyyaml<6
  Downloading PyYAML-5.4.1.tar.gz (175 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 175.1/175.1 kB 4.8 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [54 lines of output]
      running egg_info
      writing lib3/PyYAML.egg-info/PKG-INFO
      writing dependency_links to lib3/PyYAML.egg-info/dependency_links.txt
      writing top-level names to lib3/PyYAML.egg-info/top_level.txt
      Traceback (most recent call last):
        File "/home/andrew/astral/issues/uv/i1455/pip/.venv/lib/python3.11/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/home/andrew/astral/issues/uv/i1455/pip/.venv/lib/python3.11/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/home/andrew/astral/issues/uv/i1455/pip/.venv/lib/python3.11/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 271, in <module>
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/__init__.py", line 103, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 185, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 201, in run_commands
          dist.run_commands()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 969, in run_commands
          self.run_command(cmd)
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/dist.py", line 963, in run_command
          super().run_command(command)
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 321, in run
          self.find_sources()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 329, in find_sources
          mm.run()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 550, in run
          self.add_defaults()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/command/egg_info.py", line 588, in add_defaults
          sdist.add_defaults(self)
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/command/sdist.py", line 102, in add_defaults
          super().add_defaults()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/command/sdist.py", line 251, in add_defaults
          self._add_defaults_ext()
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/command/sdist.py", line 336, in _add_defaults_ext
          self.filelist.extend(build_ext.get_source_files())
                               ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "<string>", line 201, in get_source_files
        File "/tmp/pip-build-env-ter9lqc8/overlay/lib/python3.11/site-packages/setuptools/_distutils/cmd.py", line 107, in __getattr__
          raise AttributeError(attr)
      AttributeError: cython_sources
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error
```

And in particular, this is precisely the same error that `uv pip install "pyyaml<6"` fails with. So to me, this seems likely not a problem with `uv`, although it is still somewhat mysterious that your `pip install` command worked. But, because of the known build errors with PyYAML and Cython, it seems likely that this isn't an issue on our end. Of course, I'm sure we'd welcome a deeper analysis here!

Note that installing `pyyaml>=6` works:

```
$ uv pip install "pyyaml>=6"
Resolved 1 package in 59ms
Downloaded 1 package in 47ms
Installed 1 package in 6ms
 + pyyaml==6.0.1
```

---

_Closed by @BurntSushi on 2024-02-21 14:39_

---

_Referenced in [a2cps/python-vbr#24](../../a2cps/python-vbr/pulls/24.md) on 2024-10-10 17:55_

---

_Referenced in [RedHatQE/openshift-python-wrapper#2202](../../RedHatQE/openshift-python-wrapper/issues/2202.md) on 2024-11-27 16:38_

---

_Comment by @HernandoR on 2025-01-14 01:51_

is ther any workaround here, to install pyyaml with uv?

---

_Comment by @zanieb on 2025-01-14 01:57_

This error is reproducible with `pip` — it's not a uv error. You'll need to report the failure to pyyaml.

---

_Comment by @HernandoR on 2025-01-14 02:21_

based on [pyyaml#724](https://github.com/yaml/pyyaml/issues/724); current work around is `--no-build-isolation` with preinstalled cython<3. Is it possible to address this method in `pyproject.toml`, such that uv can solve it? 

my dependencies will solve to `pyyaml==5.4.1` so manally installing would not work for me 

---

_Comment by @zanieb on 2025-01-14 03:18_

You can set [`no-build-isolation-package`](https://docs.astral.sh/uv/reference/settings/#no-build-isolation-package)

```toml
[project]
name = "example"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = ["pyyaml<6"]

[tool.uv]
no-build-isolation-package = ["pyyaml"]

[dependency-groups]
pyyaml-build = ["cython<3", "setuptools"]
```

```
❯ uv sync --only-group pyyaml-build
Resolved 4 packages in 5ms
Prepared 1 package in 263ms
Installed 2 packages in 11ms
 + cython==3.0.11
 + setuptools==75.8.0
❯ uv sync
Resolved 4 packages in 1ms
   Built pyyaml==5.4.1
Prepared 1 package in 812ms
Uninstalled 2 packages in 46ms
Installed 1 package in 1ms
 - cython==0.29.37
 + pyyaml==5.4.1
 - setuptools==75.8.0
```

---

_Comment by @tux3 on 2025-06-27 11:11_

For any future visitors, this happens when trying to run `uvx some_package` when some_package depends on pyyaml 5

`uvx --with pyyaml==5.3.1 somepackage` is another way to workaround

---
