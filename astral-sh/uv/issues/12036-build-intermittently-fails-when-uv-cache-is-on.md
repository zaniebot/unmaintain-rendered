```yaml
number: 12036
title: Build intermittently fails when uv cache is on NFS mount
type: issue
state: open
author: tekumara
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-03-07T09:35:20Z
updated_at: 2026-01-11T14:23:45Z
url: https://github.com/astral-sh/uv/issues/12036
synced_at: 2026-01-12T16:00:53Z
```

# Build intermittently fails when uv cache is on NFS mount

---

_@tekumara_

### Summary

When the uv cache is on an NFS network mount, the build tempdir is located inside the cache and a setuptools build intermittently fails with:
```
OSError: [Errno 39] Directory not empty: '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info'
...

OSError: [Errno 16] Device or resource busy: '.nfs0000000000fdd78100000091'
```


<details>

<summary>Full stack trace</summary>

<br/>
NB: The NFS mount is /mnt/fsx 

```
$ uv sync --all-groups
Resolved 303 packages in 0.86ms
  × Failed to build `embmem @ file:///home/compute/embmem`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_editable` failed (exit status: 1)

      [stdout]
      running editable_wheel
      creating /mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info
      writing /mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info/PKG-INFO
      writing dependency_links to /mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info/dependency_links.txt
      writing requirements to /mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info/requires.txt
      writing top-level names to /mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info/top_level.txt
      writing manifest file '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info/SOURCES.txt'
      writing manifest file '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info/SOURCES.txt'
      creating '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem-0.1.dev20+g6928d11.d20250307.dist-info'

      [stderr]
      Traceback (most recent call last):
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/command/editable_wheel.py", line 148, in run
          self._ensure_dist_info()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/command/editable_wheel.py", line 167, in _ensure_dist_info
          dist_info.run()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/command/dist_info.py", line 101, in run
          bdist_wheel.egg2dist(egg_info_dir, self.dist_info_dir)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/wheel/_bdist_wheel.py", line 613, in egg2dist
          adios(egginfo_path)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/wheel/_bdist_wheel.py", line 550, in adios
          shutil.rmtree(p)
        File "/usr/lib/python3.11/shutil.py", line 763, in rmtree
          onerror(os.rmdir, path, sys.exc_info())
        File "/usr/lib/python3.11/shutil.py", line 761, in rmtree
          os.rmdir(path, dir_fd=dir_fd)
      OSError: [Errno 39] Directory not empty: '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info'
      /mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/_distutils/dist.py:988: _DebuggingTips: Problem in editable installation.
      !!

              ********************************************************************************
              An error happened while installing `embmem` in editable mode.

              The following steps are recommended to help debug this problem:

              - Try to install the project normally, without using the editable mode.
                Does the error still persist?
                (If it does, try fixing the problem before attempting the editable mode).
              - If you are using binary extensions, make sure you have all OS-level
                dependencies installed (e.g. compilers, toolchains, binary libraries, ...).
              - Try the latest version of setuptools (maybe the error was already fixed).
              - If you (or your project dependencies) are using any setuptools extension
                or customization, make sure they support the editable mode.

              After following the steps above, if the problem still persists and
              you think this is related to how setuptools handles editable installations,
              please submit a reproducible example
              (see https://stackoverflow.com/help/minimal-reproducible-example) to:

                  https://github.com/pypa/setuptools/issues

              See https://setuptools.pypa.io/en/latest/userguide/development_mode.html for details.
              ********************************************************************************

      !!
        cmd_obj.run()
      Traceback (most recent call last):
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 200, in run_commands
          dist.run_commands()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 969, in run_commands
          self.run_command(cmd)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/dist.py", line 967, in run_command
          super().run_command(command)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/_distutils/dist.py", line 988, in run_command
          cmd_obj.run()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/command/editable_wheel.py", line 148, in run
          self._ensure_dist_info()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/command/editable_wheel.py", line 167, in _ensure_dist_info
          dist_info.run()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/command/dist_info.py", line 101, in run
          bdist_wheel.egg2dist(egg_info_dir, self.dist_info_dir)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/wheel/_bdist_wheel.py", line 613, in egg2dist
          adios(egginfo_path)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/wheel/_bdist_wheel.py", line 550, in adios
          shutil.rmtree(p)
        File "/usr/lib/python3.11/shutil.py", line 763, in rmtree
          onerror(os.rmdir, path, sys.exc_info())
        File "/usr/lib/python3.11/shutil.py", line 761, in rmtree
          os.rmdir(path, dir_fd=dir_fd)
      OSError: [Errno 39] Directory not empty: '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/build_meta.py", line 395, in _build_with_temp_dir
          self.run_setup()
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 1, in <module>
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/__init__.py", line 104, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 184, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/_distutils/core.py", line 208, in run_commands
          raise SystemExit(f"error: {exc}")
      SystemExit: error: [Errno 39] Directory not empty: '/mnt/fsx/.cache/uv/builds-v0/.tmpSgiEoz/.tmp-sn3efv5b/embmem.egg-info'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/build_meta.py", line 443, in build_editable
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/mnt/fsx/.cache/uv/builds-v0/.tmplT6vhc/lib/python3.11/site-packages/setuptools/build_meta.py", line 385, in _build_with_temp_dir
          with tempfile.TemporaryDirectory(**temp_opts) as tmp_dist_dir:
        File "/usr/lib/python3.11/tempfile.py", line 1082, in __exit__
          self.cleanup()
        File "/usr/lib/python3.11/tempfile.py", line 1086, in cleanup
          self._rmtree(self.name, ignore_errors=self._ignore_cleanup_errors)
        File "/usr/lib/python3.11/tempfile.py", line 1068, in _rmtree
          _rmtree(name, onerror=onerror)
        File "/usr/lib/python3.11/shutil.py", line 752, in rmtree
          _rmtree_safe_fd(fd, path, onerror)
        File "/usr/lib/python3.11/shutil.py", line 672, in _rmtree_safe_fd
          _rmtree_safe_fd(dirfd, fullname, onerror)
        File "/usr/lib/python3.11/shutil.py", line 703, in _rmtree_safe_fd
          onerror(os.unlink, fullname, sys.exc_info())
        File "/usr/lib/python3.11/shutil.py", line 701, in _rmtree_safe_fd
          os.unlink(entry.name, dir_fd=topfd)
      OSError: [Errno 16] Device or resource busy: '.nfs0000000000fdd78100000091'
```

</details>
  

### Platform

Ubuntu 20.04

### Version

uv 0.5.30

### Python version

Python 3.11

---

_Label `bug` added by @tekumara on 2025-03-07 09:35_

---

_Comment by @tekumara on 2025-03-07 09:35_

https://github.com/astral-sh/uv/issues/3684 could be a solution to this.

---

_Comment by @ZaberKo on 2025-10-17 16:08_

@tekumara Hi, could you explain how to solve it in detail? Thank you.

---

_Comment by @Tomorrowdawn on 2025-11-03 06:44_

I can confirm I'm experiencing the exact same issue. When using uv pip install in a virtual environment located on an NFS share, I consistently receive the "Device or resource busy" error or "Directory not empty" error. 

My colleague suggests to change uv cache dir to a local directory, however when installing some packages, like   `dill`, `antlr` , uv will remove directory (xxx.data) in env, like:

```bash
(Terminal-Agents) tangchenxia@vm-tangchenxia:~/projects/Terminal-Agents$  UV_CACHE_DIR=/tmp uv pip install -e .
Resolved 180 packages in 3.19s
      Built autopilot @ file:///home/aiops/tangchenxia/projects/Terminal-Agents
      Built antlr4-python3-runtime==4.9.3
Prepared 179 packages in 5.22s
░░░░░░░░░░░░░░░░░░░░ [0/180] Installing wheels...                                                         warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
error: Failed to install: dill-0.4.0-py3-none-any.whl (dill==0.4.0)
  Caused by: failed to remove directory `/home/aiops/tangchenxia/projects/Terminal-Agents/.venv/lib/python3.12/site-packages/dill-0.4.0.data`: Device or resource busy (os error 16)
(Terminal-Agents) tangchenxia@vm-tangchenxia:~/projects/Terminal-Agents$ source ./.venv/bin/activate
(Terminal-Agents) tangchenxia@vm-tangchenxia:~/projects/Terminal-Agents$  UV_CACHE_DIR=/tmp uv pip install -e .
Resolved 180 packages in 1.73s
      Built autopilot @ file:///home/aiops/tangchenxia/projects/Terminal-Agents
Prepared 1 package in 573ms
░░░░░░░░░░░░░░░░░░░░ [0/100] Installing wheels...                                  warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
error: Failed to install: antlr4_python3_runtime-4.9.3-py3-none-any.whl (antlr4-python3-runtime==4.9.3)
  Caused by: failed to remove directory `/home/aiops/tangchenxia/projects/Terminal-Agents/.venv/lib/python3.12/site-packages/antlr4_python3_runtime-4.9.3.data`: Device or resource busy (os error 16)
```

I can also confirm the workaround: **a regular pip install in the same venv works fine**, while uv fails with "Device or resource busy". 

<img width="1290" height="455" alt="Image" src="https://github.com/user-attachments/assets/10913be8-7a65-4c51-9310-6b4de698f3b8" />

Hoping this extra data point helps in tracking down the root cause. We're really excited about uv's speed and would love to be able to use it in our NFS-based development environments.

---

_Label `needs-mre` added by @konstin on 2025-11-05 18:16_

---

_Comment by @konstin on 2025-11-05 19:10_

I consider that a bug in the NFS implementations that they expose the `.nfs` and prevent directories from being deleted, but we may need to add some backoff and retry as workaround.

---

_Comment by @chrispy-snps on 2026-01-11 14:13_

For some reason, we are now getting this error consistently too. I think the sleep/retry would work because (1) the .nfs* lock changes each time and (2) it's not present when I check for it, which means it likely expired just after being created.

In the past, I have also had to implement sleep/retry logic to work around transient .nfs* locks. It feels terribly kludgy as a software developer...

---
