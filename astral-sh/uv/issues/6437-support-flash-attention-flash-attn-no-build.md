```yaml
number: 6437
title: "Support flash attention `flash-attn --no-build-isolation` with `uv sync`"
type: issue
state: closed
author: vwxyzjn
labels:
  - enhancement
  - projects
assignees: []
created_at: 2024-08-22T14:04:48Z
updated_at: 2025-10-28T14:32:51Z
url: https://github.com/astral-sh/uv/issues/6437
synced_at: 2026-01-12T15:59:04Z
```

# Support flash attention `flash-attn --no-build-isolation` with `uv sync`

---

_@vwxyzjn_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


I get the following error despite installation flash attention was successful via `uv add flash-attn --no-build-isolation`. See https://github.com/astral-sh/uv/issues/6402. Is there anyway to honor the `no-build-isolation` when doing `uv sync`? I imagine the idea is to mark the `flash-attn` dependencies somehow and run it with `no-build-isolation` after all the other dependencies have been installed.


pyproject.toml and uv.lock are here: https://gist.github.com/vwxyzjn/fba2c7ffac8e6c923ffe912a872ec9a8


```
  open-instruct git:(uv) ✗ uv sync
Using Python 3.12.5
Creating virtualenv at: .venv
⠦ fire==0.6.0                                                                                    error: Failed to download and build `flash-attn==2.6.3`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/costa/.cache/uv/builds-v0/.tmpLR2n19/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/costa/.cache/uv/builds-v0/.tmpLR2n19/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/costa/.cache/uv/builds-v0/.tmpLR2n19/lib/python3.12/site-packages/setuptools/build_meta.py", line 502, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/costa/.cache/uv/builds-v0/.tmpLR2n19/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 21, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that flash-attn==2.6.3 depends on torch, but doesn't declare it as a build dependency. If flash-attn==2.6.3 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```








---

_Comment by @zanieb on 2024-08-22 16:02_

You'll need to use `uv sync --no-build-isolation-package flash-attn` — it'd be great if we tracked this though!

---

_Label `enhancement` added by @zanieb on 2024-08-22 16:02_

---

_Label `projects` added by @zanieb on 2024-08-22 16:02_

---

_Comment by @vwxyzjn on 2024-08-22 16:32_

Wow amazing that this feature already exists!

---

_Comment by @vwxyzjn on 2024-08-22 17:12_

I just tested it out, and it seems to fail:

```
uv sync --no-build-isolation-package flash-attn
⠹ flash-attn==2.6.3                                                                                 error: Failed to download and build `flash-attn==2.6.3`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/costa/.cache/uv/builds-v0/.tmpEfmf64/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/costa/.cache/uv/builds-v0/.tmpEfmf64/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/costa/.cache/uv/builds-v0/.tmpEfmf64/lib/python3.12/site-packages/setuptools/build_meta.py", line 502, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/costa/.cache/uv/builds-v0/.tmpEfmf64/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 21, in <module>
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that flash-attn==2.6.3 depends on torch, but doesn't declare it as a build dependency. If flash-attn==2.6.3 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```
<img width="756" alt="image" src="https://github.com/user-attachments/assets/f93eb5e1-ebe7-49d5-b618-b8d43b462ea5">


---

_Comment by @charliermarsh on 2024-08-22 17:19_

I think to make this work with `uv sync`, sadly you need to do something like `uv pip install torch` prior to running `uv sync`. The build dependencies have to be available in the virtual environment _before_ you run the install.



---

_Comment by @kabouzeid on 2024-08-25 10:04_

I'm curious about what the recommended best practice is for including torch extensions in the project.

Here's my current approach:
```shell
uv sync  # torch is installed here
uv sync --extra build  # to keep things tidy: setuptools, wheel, and other stuff missing because of --no-build-isolation
uv sync --extra torchextensions --no-build-isolation  # torch extensions installed here
```

Previously, I did this, which is also not ideal:
```shell
uv sync  # torch is installed here
uv pip install setuptools wheel ...  # missing because of --no-build-isolation
uv pip install --no-build-isolation some_torch_extension
```

Your recommendation here is also not ideal, because you would need to manually make sure to `pip install` the **exact** torch version that is also in the uv.lock file:
```shell
uv pip install torch==2.3.1 --index-url https://download.pytorch.org/whl/cu121
uv pip install setuptools wheel ...  # missing because of --no-build-isolation
uv sync --no-build-isolation  # torch extensions installed here
```


---

_Comment by @vwxyzjn on 2024-08-25 13:29_

You can include wheel and setup tools in uv add I think

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 16:02_

---

_Comment by @charliermarsh on 2024-08-25 16:02_

I added some docs for this here: https://github.com/astral-sh/uv/pull/6607

---

_Closed by @charliermarsh on 2024-08-26 18:21_

---

_Comment by @eladrich on 2024-12-11 09:40_

Hi, I was following the suggested solution that was also introduced in the [docs](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation) of specifying flash-attn as an extra dependency and use the following two commands
```bash
uv sync --extra build
uv sync --extra build --extra compile
```

My main challenge with that solution is from now on this required me to always remember to specify these flags when using uv for that project with the flash-attn dependencies, otherwise if I accidentally use a `uv sync` command the package gets uninstalled.
A different suggested solution was to separately use a `uv pip install torch` beforehand but I think that requires verifying I'm using the right torch versions in both the `uv pip` commands and `sync` ones and I wanted to avoid the `uv pip` syntax completely. 

Instead, I managed to build the environment by specifying flash-attn alongside the other dependencies (while still defining it as a `no-build-isolation-package`) and then using the `--no-install-package` to create the environment the first time.
```bash
uv sync --no-install-package flash-attn
uv sync
```
If I understand correctly this allows me to use the regular `uv sync` once the project was initialized. I was wondering whether that is a valid solution or whether it has other downsides? 

---

_Comment by @marcelroed on 2025-08-03 19:30_

For anyone still following this, it's possible to install flash-attn without the two steps (meaning `uv run` should work fine with no fuss) if you specify extra build dependencies for `flash-attn`. This isn't quite robust yet, but it works well as long as you specify your torch version in a narrow way. Some more features are due to make this more ergonomic in the future. See Charlie's latest comments in https://github.com/astral-sh/uv/issues/13959#issuecomment-3146717644. 

As of uv v0.8.4 it looks like this in practice:
```toml
[tool.uv.extra-build-dependencies]
# Will require these extra packages when building flash-attn.
# It's important that the torch version matches the one used by this package.
# Consider using == in both your dependencies and here for an exact match on the torch version.
flash-attn = ["setuptools>=80.9.0", "torch>=2.6,<2.7"]
```

With this, I can install all my packages, including `flash-attn`, without any additional steps.

---

_Comment by @finbarrtimbers on 2025-08-07 17:12_

Hey @marcelroed can you share your entire `pyproject.toml` file? I'm struggling to get this to work.

---

_Comment by @charliermarsh on 2025-08-07 17:21_

(I’m gonna overhaul the docs around flash-attn shortly as we’ve shipped some new things that should make it easier. Unfortunately I’m on my phone right now so hard to type it all out but I’ll try to post here when I’m back.)

---

_Comment by @charliermarsh on 2025-08-08 09:51_

If anyone wants to try it out, this is what we're planning to recommend:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = ["torch", "flash-attn"]

[tool.uv.extra-build-dependencies]
flash-attn = [{ requirement = "torch", match-runtime = true }]

[tool.uv.extra-build-variables]
flash-attn = { FLASH_ATTENTION_SKIP_CUDA_BUILD = "TRUE" }
```

(You'll need to be on latest uv.)

This lets `flash-attn` build with build isolation (so, no more `--no-build-isolation`), but _also_ ensures that `torch` is included in the environment -- and, critically, that it uses the same version of `torch` as in the lockfile, which is the crucial requirement for the `flash-attn` build script. This should let you "just" `uv sync` without requiring multiple invocations.


---

_Comment by @kabouzeid on 2025-08-08 11:36_

Thanks, I just tried it and it works! However, I noticed that the wheel is not being rebuilt when I change the build dependencies:

```bash
uv sync  # builds flash-attn
uv add torch==2.6  # change build dep version
uv sync  # does not rebuild flash-attn
```

---

_Comment by @charliermarsh on 2025-08-08 12:16_

Can you explain what you mean by "not being rebuilt"? The flash-attn wheel is basically never built. The `setup.py` just downloads it from GitHub. Do you mean that it's not being reinstalled when you change your torch version?

---

_Comment by @charliermarsh on 2025-08-08 13:01_

If so, I think `--reinstall-package flash-attn` would correctly pull in the right version. I'll file an issue for the stale "retained package".

---

_Comment by @kabouzeid on 2025-08-08 14:55_

By being built I mean uv "building" the wheel instead of using the wheel in uv's cache. In this case building is just `setup.py` being executed and downloading the correct wheel.

Yes, `--reinstall-package flash-attn` works, but I think the correct solution would be to have the set of build deps (including their exact version) be part of a wheel's cache key.

---

_Comment by @zanieb on 2025-08-08 15:08_

It is a part of the wheel's cache key. The problem is when we're checking if the package is already installed.

---

_Comment by @zanieb on 2025-08-13 03:43_

We're tracking that in https://github.com/astral-sh/uv/issues/15218

---

_Comment by @Butanium on 2025-10-05 11:51_

Just to make sure: there is no magic uv flags to make this work on Windows without going through custom wheels / full build from scratch (according to https://github.com/Dao-AILab/flash-attention/issues/1469 this is what I should do)

It fails like this for me:
```py
                                                                             × Failed to build `flash-attn==2.8.3`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stdout]
      running bdist_wheel

      [stderr]
      C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\torch\_subclasses\functional_tensor.py:279: UserWarning: Failed to initialize NumPy: No module      
      named 'numpy' (Triggered internally at C:\actions-runner\_work\pytorch\pytorch\pytorch\torch\csrc\utils\tensor_numpy.cpp:81.)
        cpu = _conversion_method_template(device=torch.device("cpu"))
      C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\dist.py:759: SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: BSD License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\build_meta.py", line 432, in build_wheel
          return _build(['bdist_wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\build_meta.py", line 423, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\build_meta.py", line 404, in _build_with_temp_dir
          self.run_setup()
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\build_meta.py", line 512, in run_setup
          super().run_setup(setup_script=setup_script)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\build_meta.py", line 317, in run_setup
          exec(code, locals())
        File "<string>", line 526, in <module>
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\__init__.py", line 115, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\_distutils\core.py", line 186, in setup
          return run_commands(dist)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\_distutils\core.py", line 186, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
                 ^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\_distutils\core.py", line 202, in run_commands
          dist.run_commands()
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\_distutils\dist.py", line 1002, in run_commands
          self.run_command(cmd)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\dist.py", line 1102, in run_command
          super().run_command(command)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\setuptools\_distutils\dist.py", line 1021, in run_command
          cmd_obj.run()
        File "<string>", line 483, in run
        File "<string>", line 456, in get_wheel_url
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\packaging\version.py", line 56, in parse
          return Version(version)
                 ^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpxWQvwf\Lib\site-packages\packaging\version.py", line 200, in __init__
          match = self._regex.search(version)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
      TypeError: expected string or bytes-like object, got 'NoneType'

      hint: This usually indicates a problem with the package or the build environment.
  help: `flash-attn` (v2.8.3) was included because `nnterp[llms]` depends on `flash-attn`

C:\Users\Travail\Documents\interp\nnterp>uv pip install flash-attn --no-build-isolation

C:\Users\Travail\Documents\interp\nnterp>uv sync --no-build-isolation-package flash-attn --extra dev --extra llms
Resolved 271 packages in 4.55s                                                                                                                                                           
      Built nnterp @ file:///C:/Users/Travail/Documents/interp/nnterp
Prepared 1 package in 1.59s
Uninstalled 2 packages in 88ms
Installed 1 package in 8ms
 ~ nnterp==1.0b3.dev12+g7e614574b.d20251005 (from file:///C:/Users/Travail/Documents/interp/nnterp)
 - setuptools==79.0.1

C:\Users\Travail\Documents\interp\nnterp>uv pip show flash-attn
warning: Package(s) not found for: flash-attn

C:\Users\Travail\Documents\interp\nnterp>uv pip show flash-attn
warning: Package(s) not found for: flash-attn

C:\Users\Travail\Documents\interp\nnterp>uv sync --no-build-isolation-package flash-attn --extra dev --extra llms
Resolved 272 packages in 3.52s                                                                                                                                                           
      Built nnterp @ file:///C:/Users/Travail/Documents/interp/nnterp
Prepared 1 package in 1.54s
Uninstalled 1 package in 3ms
Installed 2 packages in 16ms
  × Failed to build `flash-attn==2.8.3`
  ├─▶ The build backend returned an error                                                                                                                                                
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
      ModuleNotFoundError: No module named 'setuptools'

      hint: This error likely indicates that `flash-attn@2.8.3` depends on `setuptools`, but doesn't declare it as a build dependency. If `flash-attn` is a first-party package,
      consider adding `setuptools` to its `build-system.requires`. Otherwise, either add it to your `pyproject.toml` under:

      [tool.uv.extra-build-dependencies]
      flash-attn = ["setuptools"]

      or `uv pip install setuptools` into the environment and re-run with `--no-build-isolation`.
  help: `flash-attn` (v2.8.3) was included because `nnterp[llms]` depends on `flash-attn`

C:\Users\Travail\Documents\interp\nnterp>uv sync --no-build-isolation-package flash-attn --extra dev --extra llms
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
Resolved 272 packages in 5ms
   Building nnterp @ file:///C:/Users/Travail/Documents/interp/nnterp
⠸ Preparing packages... (0/1)                                                                                                                                                            
^CTerminer le programme de commandes (O/N) ? 
^CTerminer le programme de commandes (O/N) ? 
^CTerminer le programme de commandes (O/N) ? 
^C
C:\Users\Travail\Documents\interp\nnterp>uv sync --extra dev --extra llms
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
Resolved 272 packages in 6ms
      Built nnterp @ file:///C:/Users/Travail/Documents/interp/nnterp
  × Failed to build `flash-attn==2.8.3`
  ├─▶ The build backend returned an error                                                                                                                                                
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stdout]
      running bdist_wheel

      [stderr]
      C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\torch\_subclasses\functional_tensor.py:279: UserWarning: Failed to initialize NumPy: No module      
      named 'numpy' (Triggered internally at C:\actions-runner\_work\pytorch\pytorch\pytorch\torch\csrc\utils\tensor_numpy.cpp:81.)
        cpu = _conversion_method_template(device=torch.device("cpu"))
      C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\dist.py:759: SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: BSD License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\build_meta.py", line 432, in build_wheel
          return _build(['bdist_wheel'])
                 ^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\build_meta.py", line 423, in _build
          return self._build_with_temp_dir(
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\build_meta.py", line 404, in _build_with_temp_dir
          self.run_setup()
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\build_meta.py", line 512, in run_setup
          super().run_setup(setup_script=setup_script)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\build_meta.py", line 317, in run_setup
          exec(code, locals())
        File "<string>", line 526, in <module>
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\__init__.py", line 115, in setup
          return distutils.core.setup(**attrs)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\_distutils\core.py", line 186, in setup
          return run_commands(dist)
                 ^^^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\_distutils\core.py", line 202, in run_commands
          dist.run_commands()
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\_distutils\dist.py", line 1002, in run_commands
          self.run_command(cmd)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\dist.py", line 1102, in run_command
          super().run_command(command)
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\setuptools\_distutils\dist.py", line 1021, in run_command
          cmd_obj.run()
        File "<string>", line 483, in run
        File "<string>", line 456, in get_wheel_url
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\packaging\version.py", line 56, in parse
          return Version(version)
                 ^^^^^^^^^^^^^^^^
        File "C:\Users\Travail\AppData\Local\uv\cache\builds-v0\.tmpdvwBNP\Lib\site-packages\packaging\version.py", line 200, in __init__
          match = self._regex.search(version)
                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
      TypeError: expected string or bytes-like object, got 'NoneType'

      hint: This usually indicates a problem with the package or the build environment.
  help: `flash-attn` (v2.8.3) was included because `nnterp[llms]` depends on `flash-attn`
```

I ended up just doing 
```toml
"flash-attn; sys_platform != 'win32'",
```
in my pyproject

---

_Comment by @c-vongpaseut on 2025-10-28 14:32_

Hello, I am using uv with workspaces and I have the following project structure 
```
my_project
├── packages
│   └── my_package
│       ├── pyproject.toml
│       └── src
│           └── my_package
│               ├── __init__.py
│               └── foo.py
├── pyproject.toml
├── README.md
├── uv.lock
└── src
    └── my_project
        └── main.py
```
I need to add flash attention in `my_package` dependencies. I followed the solution 
> If anyone wants to try it out, this is what we're planning to recommend:
> 
> [project]
> name = "project"
> version = "0.1.0"
> requires-python = ">=3.13"
> dependencies = ["torch", "flash-attn"]
> 
> [tool.uv.extra-build-dependencies]
> flash-attn = [{ requirement = "torch", match-runtime = true }]
> 
> [tool.uv.extra-build-variables]
> flash-attn = { FLASH_ATTENTION_SKIP_CUDA_BUILD = "TRUE" }
> 
> (You'll need to be on latest uv.)
> 
> This lets `flash-attn` build with build isolation (so, no more `--no-build-isolation`), but _also_ ensures that `torch` is included in the environment -- and, critically, that it uses the same version of `torch` as in the lockfile, which is the crucial requirement for the `flash-attn` build script. This should let you "just" `uv sync` without requiring multiple invocations. 

but I still encounter the error
```
ModuleNotFoundError: No module named 'torch'

      hint: This error likely indicates that `flash-attn@2.8.3` depends on `torch`, but doesn't declare it as a build dependency. If `flash-attn` is a first-party package, consider adding `torch` to its `build-system.requires`. Otherwise, either add it to your `pyproject.toml` under:

      [tool.uv.extra-build-dependencies]
      flash-attn = ["torch"]

      or `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```

However, it works if I move the `flash-attn` dependency in `my_project` dependencies. Is this normal?

Thanks a lot!




---
