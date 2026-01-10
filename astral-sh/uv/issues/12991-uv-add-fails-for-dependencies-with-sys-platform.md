```yaml
number: 12991
title: "`uv add` fails for dependencies with sys_platform in escaped quotes, like `sys_platform != \\\"win32\\\"`"
type: issue
state: closed
author: bartlomiejcieszkowski
labels:
  - bug
assignees: []
created_at: 2025-04-20T18:46:06Z
updated_at: 2025-04-20T20:38:16Z
url: https://github.com/astral-sh/uv/issues/12991
synced_at: 2026-01-10T03:41:47Z
```

# `uv add` fails for dependencies with sys_platform in escaped quotes, like `sys_platform != \"win32\"`

---

_Issue opened by @bartlomiejcieszkowski on 2025-04-20 18:46_

### Summary

So basically to fail installation on windows we need to do:
`uv init example && cd example && uv add gitlint`

the log output is as follows:
```
C:\Projects\zephyr-uv>uv add gitlint
  × Failed to build `sh==1.14.3`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File "C:\Users\bart\AppData\Local\uv\cache\builds-v0\.tmpMeaVoA\Lib\site-packages\setuptools\build_meta.py", line 331,
      in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\bart\AppData\Local\uv\cache\builds-v0\.tmpMeaVoA\Lib\site-packages\setuptools\build_meta.py", line 301,
      in _get_build_requires
          self.run_setup()
        File "C:\Users\bart\AppData\Local\uv\cache\builds-v0\.tmpMeaVoA\Lib\site-packages\setuptools\build_meta.py", line 512,
      in run_setup
          super().run_setup(setup_script=setup_script)
        File "C:\Users\bart\AppData\Local\uv\cache\builds-v0\.tmpMeaVoA\Lib\site-packages\setuptools\build_meta.py", line 317,
      in run_setup
          exec(code, locals())
        File "<string>", line 5, in <module>
        File "C:\Users\bart\AppData\Local\uv\cache\sdists-v9\pypi\sh\1.14.3\WzP4_GLkZg2VjhLoX7chJ\src\sh.py", line 37, in <module>
          import fcntl
      ModuleNotFoundError: No module named 'fcntl'

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and
        syncing.
```

verbose output is here:
[uv_add_gitlint_verbose.txt](https://github.com/user-attachments/files/19826553/uv_add_gitlint_verbose.txt)

as we can see uv tries to add `sh==1.14.3` and fails

if we take a look at the gitlint project [permalink](https://github.com/jorisroovers/gitlint/blob/4d9119760056492eabc201bfad5de2f9e660b85f/pyproject.toml#L131):

```
# QA
[tool.hatch.envs.qa]
description = """
Integration test environment.
Run a set of integration tests against any gitlint binary (not just the one from local source).
"""
detached = true
dependencies = [
    "pytest==7.2.1",
    "arrow==1.2.3",
    "sh==1.14.3; sys_platform != \"win32\"",
    "pdbr==0.8.2; sys_platform != \"win32\"",
]
```

we can see that the sh is not to be installed on `win32` platform

we have here escaped quotation - which im suspecting uv doesn't handle correctly

_regular `pip install gitlint` works_


### Platform

Windows 11 x86_64

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

Python 3.12.9

---

_Label `bug` added by @bartlomiejcieszkowski on 2025-04-20 18:46_

---

_Comment by @charliermarsh on 2025-04-20 18:53_

I think the diagnostic here is not quite correct. You can verify that we handle escaping correctly by `uv pip install -r pyproject.toml` with:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.13"
dependencies = ["sh==1.14.3; sys_platform == \"win32\""]
```

Instead, what you're experiencing is that we resolve for all environments to produce a lockfile, and `sh` does not publish a wheel, so we have to try and build it to get the metadata. You can read all about it here, including marking environments as excluded: https://docs.astral.sh/uv/concepts/resolution/#limited-resolution-environments

---

_Comment by @bartlomiejcieszkowski on 2025-04-20 20:38_

ok, so this is not a bug, but expected behavior, closing then

thx

---

_Closed by @bartlomiejcieszkowski on 2025-04-20 20:38_

---
