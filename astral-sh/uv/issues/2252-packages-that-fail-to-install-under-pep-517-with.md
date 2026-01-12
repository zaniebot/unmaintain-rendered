```yaml
number: 2252
title: Packages that fail to install under PEP 517 with build isolation
type: issue
state: open
author: charliermarsh
labels:
  - documentation
assignees: []
created_at: 2024-03-06T22:34:28Z
updated_at: 2025-07-31T13:58:30Z
url: https://github.com/astral-sh/uv/issues/2252
synced_at: 2026-01-12T15:58:36Z
```

# Packages that fail to install under PEP 517 with build isolation

---

_@charliermarsh_

I don't know that an issue is the right home for this, but I want to track the packages we _know_ can't be installed under PEP 517 when build isolation is enforced. In all cases, these packages _also_ fail under `pip install --use-pep517`.


---

_Comment by @charliermarsh on 2024-03-06 22:35_

[`playsound`](https://github.com/TaylorSMarks/playsound/blob/9cf4af20caa5ae8586f88b65659681b24f0c4e69/setup.py)

```
❯ pip install -e . --use-pep517
Obtaining file:///Users/crmarsh/workspace/puffin/playsound
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... error
  error: subprocess-exited-with-error

  × Getting requirements to build editable did not run successfully.
  │ exit code: 1
  ╰─> [31 lines of output]
      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 132, in get_requires_for_build_editable
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-n_6uhe_d/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 448, in get_requires_for_build_editable
          return self.get_requires_for_build_wheel(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-n_6uhe_d/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-n_6uhe_d/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-n_6uhe_d/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-n_6uhe_d/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 6, in <module>
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/inspect.py", line 1282, in getsource
          lines, lnum = getsourcelines(object)
                        ^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/inspect.py", line 1264, in getsourcelines
          lines, lnum = findsource(object)
                        ^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/inspect.py", line 1093, in findsource
          raise OSError('could not get source code')
      OSError: could not get source code
      [end of output]
```

---

_Comment by @charliermarsh on 2024-03-06 22:37_

Prior versions of `fastText` failed, but were fixed in https://github.com/facebookresearch/fastText/commit/de458ddea42327cf314e69f9eef40d0ec02705ab.

However, the most recent published version (`v0.9.2`) does fail:

```
❯ pip install -e . --use-pep517
Obtaining file:///Users/crmarsh/workspace/puffin/fastText
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... error
  error: subprocess-exited-with-error

  × Getting requirements to build editable did not run successfully.
  │ exit code: 1
  ╰─> [31 lines of output]
      /Users/crmarsh/.local/share/rtx/installs/python/3.12.1/bin/python3.12: No module named pip
      Traceback (most recent call last):
        File "<string>", line 38, in __init__
      ModuleNotFoundError: No module named 'pybind11'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 132, in get_requires_for_build_editable
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-vc8gsuu6/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 448, in get_requires_for_build_editable
          return self.get_requires_for_build_wheel(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-vc8gsuu6/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-vc8gsuu6/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-vc8gsuu6/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-vc8gsuu6/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 72, in <module>
        File "<string>", line 41, in __init__
      RuntimeError: pybind11 install failed.
      [end of output]
```

---

_Comment by @charliermarsh on 2024-03-06 22:39_

[`chumpy`](https://github.com/mattloper/chumpy/blob/51d5afd92a8ded3637553be8cef41f328a1c863a/setup.py)

```
❯ pip install -e . --use-pep517
Obtaining file:///Users/crmarsh/workspace/puffin/chumpy
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... error
  error: subprocess-exited-with-error

  × Getting requirements to build editable did not run successfully.
  │ exit code: 1
  ╰─> [29 lines of output]
      Traceback (most recent call last):
        File "<string>", line 9, in <module>
      ModuleNotFoundError: No module named 'pip'

      During handling of the above exception, another exception occurred:

      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 132, in get_requires_for_build_editable
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-31a60nip/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 448, in get_requires_for_build_editable
          return self.get_requires_for_build_wheel(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-31a60nip/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-31a60nip/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-31a60nip/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-31a60nip/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 11, in <module>
      ModuleNotFoundError: No module named 'pip'
      [end of output]
```

This also causes [`ViTPose`](https://github.com/ViTAE-Transformer/ViTPose/blob/d5216452796c90c6bc29f5c5ec0bdba94366768a/setup.py) to fail.

---

_Label `documentation` added by @charliermarsh on 2024-03-06 22:40_

---

_Label `bug` added by @charliermarsh on 2024-03-06 22:40_

---

_Label `bug` removed by @charliermarsh on 2024-03-07 21:34_

---

_Comment by @baggiponte on 2024-03-08 08:47_

I have problems with [`xformers` by Meta](https://github.com/facebookresearch/xformers) (see [issue](https://github.com/facebookresearch/xformers/issues/940)). I even tried with latest `uv` _without build isolation_:

```bash
❯ uv pip install setuptools torch numpy
Resolved 11 packages in 2ms
Installed 1 package in 23ms
 + numpy==1.26.4

❯ uv pip install xformers --no-build-isolation
Resolved 11 packages in 2ms
error: Failed to download distributions
  Caused by: Failed to fetch wheel: xformers==0.0.24
  Caused by: Failed to build: xformers==0.0.24
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
usage: setup.py [global_opts] cmd1 [cmd1_opts] [cmd2 [cmd2_opts] ...]
   or: setup.py --help [cmd1 cmd2 ...]
   or: setup.py --help-commands
   or: setup.py cmd --help

error: invalid command 'bdist_wheel'
```

---

_Comment by @charliermarsh on 2024-03-08 15:02_

@baggiponte -- I think you need to install `wheel` as well -- that's the trace you get when you're missing `wheel` (e.g., `uv pip install wheel`).

---

_Comment by @baggiponte on 2024-03-08 18:33_

> @baggiponte -- I think you need to install `wheel` as well -- that's the trace you get when you're missing `wheel` (e.g., `uv pip install wheel`).

That solves it - then I get weird C errors but definitely not uv-related.

Do you think uv should automatically recommend this kind of suggestions? A bit like the Rust compiler, that has wonderfully helpful error messages.

---

_Comment by @charliermarsh on 2024-03-08 18:39_

Yeah we should add a nice message for “missing wheel”. We have that for a few other failures. \cc @konstin 

---

_Comment by @charliermarsh on 2024-04-10 13:51_

[`youtokentome`](https://github.com/VKCOM/YouTokenToMe) at least as of f4162d846057a3118222ca04a01b84297eb8a8db.

```
❯ pip install youtokentome --use-pep517
Collecting youtokentome
  Downloading youtokentome-1.0.6.tar.gz (86 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 86.7/86.7 kB 3.5 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [20 lines of output]
      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.2/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.2/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.2/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-xop9lly4/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 5, in <module>
      ModuleNotFoundError: No module named 'Cython'
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

---

_Comment by @charliermarsh on 2024-04-11 14:07_

[`pytorch-fast-transformers`](https://github.com/idiap/fast-transformers) though perhaps intentional since it requires PyTorch at build time.

```
❯ pip install pytorch-fast-transformers --use-pep517
Collecting pytorch-fast-transformers
  Downloading pytorch-fast-transformers-0.4.0.tar.gz (93 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 93.6/93.6 kB 4.2 MB/s eta 0:00:00
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [26 lines of output]
      Traceback (most recent call last):
        File "<string>", line 19, in <module>
      ModuleNotFoundError: No module named 'torch'

      The above exception was the direct cause of the following exception:

      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.1/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-wn0uzfrt/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=['wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-wn0uzfrt/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-wn0uzfrt/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 487, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-wn0uzfrt/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 311, in run_setup
          exec(code, locals())
        File "<string>", line 22, in <module>
      ImportError: PyTorch is required to install pytorch-fast-transformers. Please install your favorite version of PyTorch, we support 1.3.1, 1.5.0 and >=1.6
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

---

_Comment by @thejcannon on 2024-05-23 19:14_

[`cchardet`](https://github.com/PyYoshi/cChardet) which doesn't work on Py311 out of the box, but _does_ work on Py311 with `cython` installed.

~But `pip --pep517` still works, `uv` doesn't (maybe because the package has no `pyproject.toml`?)~

```console
$ uv venv .venv --seed
$ .venv/bin/pip install cython==3.0.0
$ .venv/bin/pip install cchardet==2.1.7 --use-pep517
```

~succeeds~ fails with a cold cache. Succeeds without `--use-pep517`. then `uv`:

```console
$ uv venv .venv
$ uv pip install --python .venv/bin/python cython==3.0.0
$ uv pip install --python .venv/bin/python cchardet==2.1.7
```

fails.

~LMK if, because `pip` _does_ handle this, it belongs in a new issue~

---

_Comment by @charliermarsh on 2024-05-23 19:19_

`pip install cchardet==2.1.7 --use-pep517` does not work for me actually.

---

_Comment by @charliermarsh on 2024-05-23 19:20_

```
❯ python -m pip install cchardet==2.1.7 --use-pep517
Collecting cchardet==2.1.7
  Using cached cchardet-2.1.7.tar.gz (653 kB)
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Installing backend dependencies ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: cchardet
  Building wheel for cchardet (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Building wheel for cchardet (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [23 lines of output]
      running bdist_wheel
      running build
      running build_py
      creating build
      creating build/lib.macosx-14.2-arm64-cpython-311
      creating build/lib.macosx-14.2-arm64-cpython-311/cchardet
      copying src/cchardet/version.py -> build/lib.macosx-14.2-arm64-cpython-311/cchardet
      copying src/cchardet/__init__.py -> build/lib.macosx-14.2-arm64-cpython-311/cchardet
      running build_ext
      building 'cchardet._cchardet' extension
      creating build/temp.macosx-14.2-arm64-cpython-311
      creating build/temp.macosx-14.2-arm64-cpython-311/src
      creating build/temp.macosx-14.2-arm64-cpython-311/src/cchardet
      creating build/temp.macosx-14.2-arm64-cpython-311/src/ext
      creating build/temp.macosx-14.2-arm64-cpython-311/src/ext/uchardet
      creating build/temp.macosx-14.2-arm64-cpython-311/src/ext/uchardet/src
      creating build/temp.macosx-14.2-arm64-cpython-311/src/ext/uchardet/src/LangModels
      clang -Wsign-compare -Wunreachable-code -DNDEBUG -g -fwrapv -O3 -Wall -I/opt/homebrew/opt/openssl@3include -Isrc/ext/uchardet/src -I/Users/crmarsh/.local/share/rtx/installs/python/3.11.8/include/python3.11 -c src/cchardet/_cchardet.cpp -o build/temp.macosx-14.2-arm64-cpython-311/src/cchardet/_cchardet.o
      src/cchardet/_cchardet.cpp:196:12: fatal error: 'longintrepr.h' file not found
        #include "longintrepr.h"
                 ^~~~~~~~~~~~~~~
      1 error generated.
      error: command '/usr/bin/clang' failed with exit code 1
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for cchardet
Failed to build cchardet
ERROR: Could not build wheels for cchardet, which is required to install pyproject.toml-based projects
```

---

_Comment by @thejcannon on 2024-05-23 19:20_

Ahhhhhhh I'm like 90% certain `pip` pulled it from its cache on my box. 😅 

EDIT: Yeah this fails: `.venv/bin/pip install cchardet==2.1.7 --use-pep517 --no-cache`

---

_Comment by @covracer on 2024-06-24 14:48_

https://pypi.org/project/biopython/

https://github.com/astral-sh/uv/issues/4069#issuecomment-2186762048

Edit: fixed in https://github.com/biopython/biopython/pull/4749

---

_Comment by @charliermarsh on 2024-07-08 13:22_

[OpenCC](https://github.com/BYVoid/OpenCC.git@ver.1.1.7)

```
❯ pip install --use-pep517 git+https://github.com/BYVoid/OpenCC.git@ver.1.1.7
Collecting git+https://github.com/BYVoid/OpenCC.git@ver.1.1.7
  Cloning https://github.com/BYVoid/OpenCC.git (to revision ver.1.1.7) to /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-lz7b4_dp
  Running command git clone --filter=blob:none --quiet https://github.com/BYVoid/OpenCC.git /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-req-build-lz7b4_dp
  Running command git checkout -q e5d6c5f1b78e28a5797e7ad3ede3513314e544b7
  Resolved https://github.com/BYVoid/OpenCC.git to commit e5d6c5f1b78e28a5797e7ad3ede3513314e544b7
  Installing build dependencies ... done
  Getting requirements to build wheel ... error
  error: subprocess-exited-with-error

  × Getting requirements to build wheel did not run successfully.
  │ exit code: 1
  ╰─> [20 lines of output]
      Traceback (most recent call last):
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.3/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 353, in <module>
          main()
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.3/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 335, in main
          json_out['return_val'] = hook(**hook_input['kwargs'])
                                   ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/Users/crmarsh/.local/share/rtx/installs/python/3.12.3/lib/python3.12/site-packages/pip/_vendor/pyproject_hooks/_in_process/_in_process.py", line 118, in get_requires_for_build_wheel
          return hook(config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-mnle05bg/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 327, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-mnle05bg/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 297, in _get_build_requires
          self.run_setup()
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-mnle05bg/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 497, in run_setup
          super().run_setup(setup_script=setup_script)
        File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-build-env-mnle05bg/overlay/lib/python3.12/site-packages/setuptools/build_meta.py", line 313, in run_setup
          exec(code, locals())
        File "<string>", line 9, in <module>
      ModuleNotFoundError: No module named 'wheel'
      [end of output]

  note: This error originates from a subprocess, and is likely not a problem with pip.
error: subprocess-exited-with-error

× Getting requirements to build wheel did not run successfully.
│ exit code: 1
╰─> See above for output.

note: This error originates from a subprocess, and is likely not a problem with pip.
```

---

_Comment by @amanmibra on 2024-07-29 07:01_

Is the solution to just pip install?

---

_Comment by @charliermarsh on 2024-07-29 14:40_

@amanmibra -- The solution is generally to do something like:

```
uv venv
uv pip install torch setuptools
uv pip install --no-build-isolation ${package}
```

I.e., you create a virtual environment, "manually" install any build dependencies, then install the package in question with `--no-build-isolation`, so that it's built in _your_ virtual environment and has access to the installed dependencies.


---

_Comment by @astrojuanlu on 2024-07-29 16:53_

What's the `uv` logic for building packages with no `pyproject.toml`? (asking in light of #5551 - wondering if `uv` should just assume a very specific old version of setuptools... I don't know what pip does)

---

_Comment by @edmorley on 2024-07-29 17:06_

@astrojuanlu The default backend is defined here:
https://github.com/astral-sh/uv/blob/194904b34023a71b749b14f311e2151b3c53750d/crates/uv-build/src/lib.rs#L69-L76


---

_Comment by @charliermarsh on 2024-08-06 20:11_

Adding `BoltzTraP2`: https://github.com/astral-sh/uv/issues/5816

---

_Comment by @FCamborda on 2024-08-14 16:57_

> @amanmibra -- The solution is generally to do something like:
> 
> ```
> uv venv
> uv pip install torch setuptools
> uv pip install --no-build-isolation ${package}
> ```
> 
> I.e., you create a virtual environment, "manually" install any build dependencies, then install the package in question with `--no-build-isolation`, so that it's built in _your_ virtual environment and has access to the installed dependencies.

@charliermarsh Is there a similar workaround for `uv pip compile`? I landed here by googling error when trying to install torch and trorch-cluster. Am I supposed to put `torch` in the requirements.in, and then install from the output requirements.txt + torch-cluster ?

```
# requirements.in
foo
bar
...
torch
```

```shell
$ uv pip compile requirements.in --output-file requirements.txt
$ uv pip sync requirements.txt
$ uv pip install torch-cluster
```

---

_Comment by @mak448a on 2024-09-04 01:16_

> @amanmibra -- The solution is generally to do something like:
> 
> ```
> uv venv
> uv pip install torch setuptools
> uv pip install --no-build-isolation ${package}
> ```
> 
> I.e., you create a virtual environment, "manually" install any build dependencies, then install the package in question with `--no-build-isolation`, so that it's built in _your_ virtual environment and has access to the installed dependencies.

Is there a workaround for playsound? I had to use real pip instead of uv, which I'd like to avoid.

---

_Comment by @mak448a on 2024-09-26 03:20_

Ok, there's a workaround for playsound. Use uv venv --seed when creating the venv and use pip install for playsound instead of uv pip install.

---

_Comment by @bboydflo on 2024-10-27 15:04_

Tried the following on a macos m1, using uv 0.4.27

```
uv venv -p 3.11.9
uv pip install setuptools
uv pip install --no-build-isolation imgui==2.0.0
```

This is unfortunately failing. The only option I have would be to use a python version installed via `pyvenv`.  That seems to work

---

_Comment by @zanieb on 2024-10-27 16:04_

@bboydflo can you open a new issue with details including the error message?

---

_Comment by @AbdealiLoKo on 2025-01-29 14:29_

I found `pyspark==2.4.8` has a similar issue (fixed in pyspark 3):
```
venv/bin/pip install 'pyspark<3' --use-pep517                         # fails ❌ 
uv pip install 'pyspark<3' --python venv                              # fails ❌ 
venv/bin/pip install 'pyspark<3'                                      # works ✅ 
uv pip install pyspark --no-build-isolation --python venv             # works ✅ 
```
Using:
```
Python 3.10.14
pip 25.0
uv 0.5.24
setuptools 75.8.0 (from: venv/bin/pip list | grep setuptools)
```

---

_Comment by @astrojuanlu on 2025-01-29 16:28_

`pip install 'pyspark<3'` fails in the same way as `uv pip install 'pyspark<3'` for me: `AttributeError: module 'pypandoc' has no attribute 'convert'`. Doesn't look like a uv-specific thing

---

_Comment by @fortminors on 2025-01-30 10:51_

I was having problems with packages `flash-attn` and `auto-gptq`, as they both require `torch` and `setuptools` to be installed **before** their build begins. I tried many things (tool.uv.dependency-metadata, following [that page](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation) in the docs and playing around with pyproject.toml), but the only thing that worked and seems like a nice solution is using [dependency groups](https://docs.astral.sh/uv/concepts/projects/dependencies/#dependency-groups).

So my final `pyproject.toml` looks similar to this
```toml
[project]
name = "project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"

[dependency-groups]
build = [
    "setuptools>=75.8.0",
]
direct = [
    "torch>=2.5.1",
]
main = [
    "flash-attn>=2.7.3",
    "auto-gptq",
    # And all of your other dependencies
]

[tool.uv]
default-groups = ["build", "direct", "main"]
no-build-isolation-package = ["flash-attn", "auto-gptq"]

[tool.uv.sources]
auto-gptq = { git = "https://github.com/PanQiWei/AutoGPTQ.git" }

```

And then you can just do

Install build time dependencies (that are also used in project and should not be deleted after build, such as torch)
```python
uv sync --no-group main 
```
Install the whole project (including all groups declared in default-groups)
```python
uv sync
```

In my opinion, this is much better than declaring dependencies through `optional-dependencies` as suggested by [docs](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation), because they are actually not optional - someone using `flash-attn` will most likely use `torch` in the project (not just at the build phase) and the solution with groups won't require to add --extra to every uv sync call and making sure that we [retain the build dependencies](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation:~:text=retain%20the%20build%20dependencies)

@charliermarsh @zanieb Perhaps we can update the documentation with this solution for build isolation?

Hope this helps someone

---

_Comment by @JakkuSakura on 2025-03-10 10:10_

This is still relevant today. I use pipxu which uses uv
```logs
  × Failed to build `pybuild @ file:///home/jakku/Dev/pybuild`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_editable` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
          import setuptools.build_meta as backend
      ModuleNotFoundError: No module named 'setuptools'

      hint: This usually indicates a problem with the package or the build environment.
DEBUG Released lock at `/home/jakku/snap/code/185/.local/share/pipxu/venvs/1/.lock`
```

I have setup
```python
[build-system]
requires = [
    "setuptools>=61.0",
]
build-backend = "setuptools.build_meta"
```
and I'm using python 3.13.2

---

_Comment by @saagarjha on 2025-06-06 14:35_

@fortminors I don't think your solution with dependency groups actually works as you describe. If you try either command on a fresh install you'll get an error that setuptools is not available.

---

_Comment by @zanieb on 2025-07-31 13:58_

We've added a new `extra-build-dependencies` feature that you can use to add `setuptools`, `pip`, etc. to build environments so you don't need to turn off build isolation. See #14735 

---
