---
number: 1766
title: "Windows: symbolic link to the `exe` created by uv can not work"
type: issue
state: closed
author: j178
labels:
  - bug
  - windows
assignees: []
created_at: 2024-02-20T16:52:29Z
updated_at: 2024-02-22T15:10:04Z
url: https://github.com/astral-sh/uv/issues/1766
synced_at: 2026-01-10T01:23:08Z
---

# Windows: symbolic link to the `exe` created by uv can not work

---

_Issue opened by @j178 on 2024-02-20 16:52_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Information
```
System: Windows 11 (x86_64)
Shell: PowerShell 7
uv version: uv 0.1.5
```

The original issue is from https://github.com/mitsuhiko/rye/issues/696. 
Rye creates symbolic links to installed script under `~/.rye/shims`, but the script created by `uv` can not work. The default `pip-tools` created script works fine.

## Steps to reproduce

```sh
> uv --version
uv 0.1.5

> uv -v venv -p C:\Users\nigel\.rye\py\cpython@3.12.1\install\python.exe
 uv_interpreter::python_query::find_requested_python request="C:\\Users\\nigel\\.rye\\py\\cpython@3.12.1\\install\\python.exe", platform=Platform { os: Windows, arch: X86_64 }, cache=Cache { root: "\\\\?\\C:\\Users\\nigel\\AppData\\Local\\uv\\cache", refresh: None, _temp_dir_drop: None }
      0.002273s   0ms DEBUG uv_interpreter::interpreter Using cached markers for: \\?\C:\Users\nigel\.rye\py\cpython@3.12.1\install\python.exe
Using Python 3.12.1 interpreter at C:\Users\nigel\.rye\py\cpython@3.12.1\install\python.exe
Creating virtualenv at: .venv

> uv -v pip install pycowsay
 uv::requirements::from_source source=pycowsay
    0.002195s DEBUG uv_interpreter::virtual_env Found a virtualenv named .venv at: E:\tmp\test-uv\.venv
    0.002394s DEBUG uv_interpreter::interpreter Detecting markers for: \\?\E:\tmp\test-uv\.venv\Scripts\python.exe
    0.256957s DEBUG uv::commands::pip_install Using Python 3.12.1 environment at E:\tmp\test-uv\.venv\Scripts\python.exe
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.261629s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.12.1
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.261995s   0ms DEBUG uv_resolver::resolver Adding direct dependency: pycowsay*
   uv_resolver::resolver::choose_version package=pycowsay
     uv_resolver::resolver::package_wait package_name=pycowsay
 uv_resolver::resolver::process_request request=Versions pycowsay
   uv_client::registry_client::simple_api package=pycowsay
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\nigel\AppData\Local\uv\cache\simple-v1\pypi\pycowsay.rkyv
 uv_resolver::resolver::process_request request=Prefetch pycowsay *
          0.262916s   0ms DEBUG uv_client::cached_client Found stale response for: https://pypi.org/simple/pycowsay/
          0.262970s   0ms DEBUG uv_client::cached_client Sending revalidation request for: https://pypi.org/simple/pycowsay/
       uv_client::cached_client::revalidation_request url="https://pypi.org/simple/pycowsay/"
          0.761721s 499ms DEBUG uv_client::cached_client Found not-modified response for: https://pypi.org/simple/pycowsay/
       uv_client::cached_client::refresh_cache file=\\?\C:\Users\nigel\AppData\Local\uv\cache\simple-v1\pypi\pycowsay.rkyv
 uv_resolver::version_map::from_metadata
   uv_distribution::distribution_database::get_or_build_wheel_metadata dist=pycowsay==0.0.0.2
     uv_client::registry_client::wheel_metadata built_dist=pycowsay==0.0.0.2
       uv_client::cached_client::get_serde
         uv_client::cached_client::get_cacheable
           uv_client::cached_client::read_and_parse_cache file=\\?\C:\Users\nigel\AppData\Local\uv\cache\wheels-v0\pypi\pycowsay\pycowsay-0.0.0.2-py3-none-any.msgpack
        0.764071s 501ms DEBUG uv_resolver::resolver Searching for a compatible version of pycowsay (*)
        0.764190s 502ms DEBUG uv_resolver::resolver Selecting: pycowsay==0.0.0.2 (pycowsay-0.0.0.2-py3-none-any.whl)
   uv_resolver::resolver::get_dependencies package=pycowsay, version=0.0.0.2
     uv_resolver::resolver::distributions_wait package_id=pycowsay-0.0.0.2
              0.764488s   0ms DEBUG uv_client::cached_client Found fresh response for: https://files.pythonhosted.org/packages/eb/bd/17f894b13037499e615048a6daeb9cae28142ee85ce6eb8d24d10504860b/pycowsay-0.0.0.2-py3-none-any.whl
Resolved 1 package in 503ms
    0.765456s DEBUG uv_installer::plan Requirement already cached: pycowsay==0.0.0.2
 uv_installer::installer::install num_wheels=1
Installed 1 package in 9ms
 + pycowsay==0.0.0.2

# Create a symbolic link to pycowsay
> New-Item -Path .\pycatsay.exe -ItemType SymbolicLink -Value .\.venv\Scripts\pycowsay.exe

    Directory: E:\tmp\test-uv

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
la---           2024/2/21     0:47              0 pycatsay.exe -> .\.venv\Scripts\pycowsay.exe

> .\pycatsay.exe
Error: The system cannot find the file specified.
 (from CreateProcessA(null(), child_cmdline.into_bytes_with_nul().as_mut_ptr(),
    null(), null(), 1, 0, null(), null(), addr_of!(*si),
    child_process_info.as_mut_ptr()))
```

---

_Referenced in [astral-sh/rye#696](../../astral-sh/rye/issues/696.md) on 2024-02-20 16:52_

---

_Comment by @MichaReiser on 2024-02-20 17:10_

Thanks for reporting. Hmm, that's going to be interesting. What I understand from the error is that our shim fails to locate the `pyton.exe` path because it searches it relative to the executed command. We may need to check if the file is a symlink before resolving the python exe.

---

_Label `bug` added by @MichaReiser on 2024-02-20 17:10_

---

_Label `windows` added by @MichaReiser on 2024-02-20 17:10_

---

_Comment by @konstin on 2024-02-20 17:15_

On unix we use the absolute path as interpreter, maybe we can embed it in the binary too. Otherwise we need to find a solution that works with symlinks, junctions and hardlinks (as far as these work with exes at all).

---

_Comment by @j178 on 2024-02-20 17:16_

> On unix we use the absolute path as interpreter, maybe we can embed it in the binary too.

Yes, I agree this should be the best solution.

---

_Comment by @MichaReiser on 2024-02-20 17:21_

> On unix we use the absolute path as interpreter, maybe we can embed it in the binary too. Otherwise we need to find a solution that works with symlinks, junctions and hardlinks (as far as these work with exes at all).

I considered searching for the shebang of the script and use that hehe. But it would require unzipping the script. I don't know if there's another way to embed it except if we manipulate the binary in place which sounds scary

---

_Comment by @mitsuhiko on 2024-02-20 20:22_

It looks like `setuptools` own launcher executable "parses" the shebang.

https://github.com/pypa/setuptools/blob/569fd7b0b587409f4043f127a766131a25b366dd/launcher.c#L293-L298

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-21 11:22_

---

_Referenced in [astral-sh/uv#1803](../../astral-sh/uv/pulls/1803.md) on 2024-02-21 11:22_

---

_Closed by @MichaReiser on 2024-02-22 15:10_

---
