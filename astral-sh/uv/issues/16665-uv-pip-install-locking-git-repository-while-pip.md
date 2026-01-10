---
number: 16665
title: "`uv pip install` locking git repository, while `pip` does not"
type: issue
state: closed
author: Luthaf
labels:
  - question
assignees: []
created_at: 2025-11-10T13:51:39Z
updated_at: 2025-11-12T14:06:15Z
url: https://github.com/astral-sh/uv/issues/16665
synced_at: 2026-01-10T01:26:08Z
---

# `uv pip install` locking git repository, while `pip` does not

---

_Issue opened by @Luthaf on 2025-11-10 13:51_

### Summary

This is a follow up to #9687, now with a way to reproduce the issue!

See https://github.com/Luthaf/uv-git-lock for the example code and a way to reproduce. Two things seems necessary for this issue: 

- installing multiple local packages at the same time `uv pip install ./lib_a ./lib_b`;
- having a "dirty" git repository state

Running the example above produces the following:

```
$ uv venv
Using CPython 3.12.8 interpreter at: /opt/miniforge3/bin/python
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate

$ uv pip install ./lib_a ./lib_b
Resolved 2 packages in 0.90ms
      Built uv-git-lock-b @ file://~/code/tests/uv-git-lock/lib_b
      Built uv-git-lock-a @ file://~/code/tests/uv-git-lock/lib_a
Prepared 2 packages in 679ms
Installed 2 packages in 1ms
 + uv-git-lock-a==1.0.0 (from file://~/code/tests/uv-git-lock/lib_a)
 + uv-git-lock-b==1.0.0 (from file://~/code/tests/uv-git-lock/lib_b)

$ echo "# a comment" >> lib_a/setup.py
$ uv pip install ./lib_a ./lib_b
Resolved 2 packages in 0.94ms
  x Failed to build `uv-git-lock-a @ file:///~/code/tests/uv-git-lock/lib_a`
  |-> The build backend returned an error
  `-> Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "~/.cache/uv/builds-v0/.tmpa300qa/lib/python3.12/site-packages/setuptools/build_meta.py", line 331, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "~/.cache/uv/builds-v0/.tmpa300qa/lib/python3.12/site-packages/setuptools/build_meta.py", line 301, in _get_build_requires
          self.run_setup()
        File "~/.cache/uv/builds-v0/.tmpa300qa/lib/python3.12/site-packages/setuptools/build_meta.py", line 317, in run_setup
          exec(code, locals())
        File "<string>", line 12, in <module>
      RuntimeError: failed to run git, stderr=fatal: Unable to create '~/code/tests/uv-git-lock/.git/index.lock': File exists.

      Another git process seems to be running in this repository, e.g.
      an editor opened by 'git commit'. Please make sure all processes
      are terminated then try again. If it still fails, a git process
      may have crashed in this repository earlier:
      remove the file manually to continue.
      , stdout=lib_a/setup.py: needs update


      hint: This usually indicates a problem with the package or the build environment.
```

With `pip install`:

```
$ git reset --hard HEAD
$ source .venv/bin/activate

$ pip install ./lib_a ./lib_b
Processing ./lib_a
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Processing ./lib_b
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: uv-git-lock-a, uv-git-lock-b
  Building wheel for uv-git-lock-a (pyproject.toml) ... done
  Created wheel for uv-git-lock-a: filename=uv_git_lock_a-1.0.0-py3-none-any.whl size=1007 sha256=d52a394b0b3d437e477fc5f592e9b25288941a24a6754ca6426cce0aed73bc6f
  Stored in directory: /private/var/folders/nz/mgmqnhrj4mzfjlnqbgn02t780000gn/T/pip-ephem-wheel-cache-d9n5p0kr/wheels/e6/16/09/58e020c851324f04ab3944e873300cad0c96376919b4e56b3a
  Building wheel for uv-git-lock-b (pyproject.toml) ... done
  Created wheel for uv-git-lock-b: filename=uv_git_lock_b-1.0.0-py3-none-any.whl size=1006 sha256=c353a3f1a9bcc49a02c7cf739fb5c378b1cd2ed0521ef31ae72fd1ce7dd8b6d1
  Stored in directory: /private/var/folders/nz/mgmqnhrj4mzfjlnqbgn02t780000gn/T/pip-ephem-wheel-cache-d9n5p0kr/wheels/32/a6/a1/7ed348d8e60f62aaeb465ca4605d608cd53c5e70d2bac87e7a
Successfully built uv-git-lock-a uv-git-lock-b
Installing collected packages: uv-git-lock-b, uv-git-lock-a
Successfully installed uv-git-lock-a-1.0.0 uv-git-lock-b-1.0.0

$ echo "# a comment" >> lib_a/setup.py

$ pip install ./lib_a ./lib_b
Processing ./lib_a
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Processing ./lib_b
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: uv-git-lock-a, uv-git-lock-b
  Building wheel for uv-git-lock-a (pyproject.toml) ... done
  Created wheel for uv-git-lock-a: filename=uv_git_lock_a-1.0.0-py3-none-any.whl size=1007 sha256=06a1472df764425c80158ba54b22c0309ad22212164ab413cd32c6231667a32b
  Stored in directory: /private/var/folders/nz/mgmqnhrj4mzfjlnqbgn02t780000gn/T/pip-ephem-wheel-cache-egzqh5sl/wheels/e6/16/09/58e020c851324f04ab3944e873300cad0c96376919b4e56b3a
  Building wheel for uv-git-lock-b (pyproject.toml) ... done
  Created wheel for uv-git-lock-b: filename=uv_git_lock_b-1.0.0-py3-none-any.whl size=1006 sha256=3489230347fd829ef63ad3ddef7772a0baad5d68abb723146b8e3a01ee76c7a4
  Stored in directory: /private/var/folders/nz/mgmqnhrj4mzfjlnqbgn02t780000gn/T/pip-ephem-wheel-cache-egzqh5sl/wheels/32/a6/a1/7ed348d8e60f62aaeb465ca4605d608cd53c5e70d2bac87e7a
Successfully built uv-git-lock-a uv-git-lock-b
Installing collected packages: uv-git-lock-b, uv-git-lock-a
  Attempting uninstall: uv-git-lock-b
    Found existing installation: uv-git-lock-b 1.0.0
    Uninstalling uv-git-lock-b-1.0.0:
      Successfully uninstalled uv-git-lock-b-1.0.0
  Attempting uninstall: uv-git-lock-a
    Found existing installation: uv-git-lock-a 1.0.0
    Uninstalling uv-git-lock-a-1.0.0:
      Successfully uninstalled uv-git-lock-a-1.0.0
Successfully installed uv-git-lock-a-1.0.0 uv-git-lock-b-1.0.0
```

### Platform

Darwin 24.4.0 arm64, also reproduced on linux

### Version

uv 0.9.8 (Homebrew 2025-11-07)

### Python version

Python 3.12.8

---

_Label `bug` added by @Luthaf on 2025-11-10 13:51_

---

_Label `bug` removed by @konstin on 2025-11-12 11:16_

---

_Label `question` added by @konstin on 2025-11-12 11:16_

---

_Comment by @konstin on 2025-11-12 11:18_

pip runs builds sequentially, while uv runs builds in parallel. This seems to be a problem with the build script, where the git commands expect that they are run sequentially. You can set `UV_CONCURRENT_BUILDS=1`, but I would recommend making the build script parallelism-safe, e.g. by waiting for a lock file in the repository root or with different git commands.

---

_Comment by @Luthaf on 2025-11-12 14:06_

Thanks for the pointer, this was indeed the issue! I fixed it by manually handling the locking.

---

_Closed by @Luthaf on 2025-11-12 14:06_

---

_Referenced in [metatensor/metatensor#1010](../../metatensor/metatensor/pulls/1010.md) on 2025-11-13 10:33_

---
