```yaml
number: 15060
title: "`platform_machine` environment marker not handled correctly"
type: issue
state: open
author: ghost
labels:
  - bug
assignees: []
created_at: 2025-08-04T14:28:52Z
updated_at: 2026-01-03T22:11:17Z
url: https://github.com/astral-sh/uv/issues/15060
synced_at: 2026-01-12T16:02:03Z
```

# `platform_machine` environment marker not handled correctly

---

_@ghost_

### Summary

It looks like uv interprets the `platform_machine` as bitness of the Python interpreter, however this should not be the case according the specification. `platform_machine` means the platform of the OS.

For example this is explained in the [PEP 780 draft](https://peps.python.org/pep-0780/#motivation):

>Another important ABI feature that is not yet covered by environment markers is the bitness of the interpreter. In most cases, the sys_platform or platform_system markers are enough, because there is only a single bitness in use per platform. This is not the case on Windows however: both 32-bit and 64-bit Python interpreters are widely used on x86-64 Windows

So running a Python 32bit interpreter on 64bit Windows actually returns "AMD64" as platform:

```
> py -3-32 -c "import platform;print(platform.machine())"
AMD64
```

Simple example how uv is handling this marker wrong:

```toml
[project]
name = "test"
version = "1.0"
requires-python = ">=3.10"
dependencies = ["sqlalchemy>=2,<3"]


[tool.uv]
environments = [
    "sys_platform == 'win32' and platform_machine == 'AMD64' and implementation_name == 'cpython'",
]
```

Running `uv lock` should include the win32 wheels in this case and running `uv sync --python cpython-3.11-windows-x86-none` should install the wheels with Python extensions modules included. However the actual behavior is that the win32 wheels are missing in the lock files and the pyd files are missing in the virtual environment. 

This is probably wrong on Linux, too. 

With 
```
[tool.uv]
environments = [
    "implementation_name == 'cpython' and sys_platform == 'linux' and platform_machine == 'x86_64'",
]
```
you get wheels for greenlet for ppc64le, s390x and x86_64 in the lock file. I would expect that first two should not be in the lock file.

### Platform

Windows 11 x86_64

### Version

uv 0.8.4 (e176e1714 2025-07-30)

### Python version

Python 3.11.9

---

_Label `bug` added by @ghost on 2025-08-04 14:28_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-05 11:06_

---

_Comment by @charliermarsh on 2025-08-05 11:08_

> you get wheels for greenlet for ppc64le, s390x and x86_64 in the lock file. I would expect that first two should not be in the lock file.

(I'm not sure that's related. We just don't implement filters for those architectures right now. Wheel filtering is considered an optimization, in that it's acceptable (though not optimal) to include "too many" wheels. Filtering out necessary wheels should be considered a bug, though.)


---

_Comment by @TomFryersMidsummer on 2025-08-11 09:59_

> > you get wheels for greenlet for ppc64le, s390x and x86_64 in the lock file. I would expect that first two should not be in the lock file.

> (I'm not sure that's related. We just don't implement filters for those architectures right now. Wheel filtering is considered an optimization, in that it's acceptable (though not optimal) to include "too many" wheels. Filtering out necessary wheels should be considered a bug, though.)

This would be a nice thing to address nonetheless: our Python code only runs on a few of our servers, so we specify `environments = ["platform_machine == 'x86_64' and sys_platform == 'linux'"]`. It's a bit unfortunate that 22 % of our `uv.lock` (and about half our most recent `uv sync -U` diff) is wheels for platforms we'll almost certainly never use.

Would it be worth me creating a separate issue for this bit?

---

_Comment by @NiuBlibing on 2025-08-25 03:36_

It confuses me. I'm adding vllm nightly version on x86 platform, but it shows that it depends on s390x.

```
[test@manjaro vllm-nightly]$ uv add vllm[flashinfer,audio,video,runai]  --index https://wheels.vllm.ai/nightly --prerelease=allow
  × No solution found when resolving dependencies:
  ╰─▶ Because outlines==0.1.11 depends on outlines-core==0.1.26 and vllm==0.10.1rc2.dev195+ga71e4765c depends on outlines{platform_machine == 's390x'}==0.1.11, we can conclude
      that vllm==0.10.1rc2.dev195+ga71e4765c and outlines-core{platform_machine != 's390x'}==0.2.10 are incompatible.
      And because vllm==0.10.1rc2.dev195+ga71e4765c depends on outlines-core{platform_machine != 's390x'}==0.2.10, we can conclude that vllm==0.10.1rc2.dev195+ga71e4765c
      cannot be used.
      And because only vllm[audio]==0.10.1rc2.dev195+ga71e4765c is available and your project depends on vllm[audio], we can conclude that your project's requirements are
      unsatisfiable.

      hint: `vllm` was found on https://wheels.vllm.ai/nightly, but not at the requested version (all of:
          vllm<0.10.1rc2.dev195+ga71e4765c
          vllm>0.10.1rc2.dev195+ga71e4765c
      ). A compatible version may be available on a subsequent index (e.g., https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple). By default, uv will only consider versions
      that are published on the first index that contains a given package, to avoid dependency confusion attacks. If all indexes are equally trusted, use `--index-strategy
      unsafe-best-match` to consider all versions from all indexes, regardless of the order in which they were defined.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
[test@manjaro vllm-nightly]$ uname
Linux
[test@manjaro vllm-nightly]$ uname -a
Linux manjaro 6.16.0-5-MANJARO #1 SMP PREEMPT_DYNAMIC Thu, 07 Aug 2025 04:47:37 +0000 x86_64 GNU/Linux
```


---

_Comment by @konstin on 2025-08-25 08:41_

@NiuBlibing You're issue sounds different and it looks like it's due to universal resolution (you can set `tool.uv.environments`), please open a new issue if the problem persists.

---

_Comment by @collinanderson on 2026-01-03 22:10_

I think I created+closed a duplicate issue (#17313) of this. 

A simple example is having `ruff` as a dependency, which adds 9+ useless long wheel lines to the `uv.lock` file.

Simple reproduction (or use `tool.uv.environments`):
`uv add "ruff; sys_platform == 'linux' and platform_machine == 'x86_64'"` 
(see #17313 for `uv.lock` output)

> We just don't implement filters for those architectures right now. Wheel filtering is considered an optimization, in that it's acceptable (though not optimal) to include "too many" wheels.

It's annoying because it generates a lot of useless noise in the diff every time ruff updates (often!).

---
