---
number: 2450
title: "`uv venv` is broken on `macos-latest` github CI runner"
type: issue
state: closed
author: yashgorana
labels:
  - macos
  - virtualenv
assignees: []
created_at: 2024-03-14T06:33:02Z
updated_at: 2024-03-14T12:37:47Z
url: https://github.com/astral-sh/uv/issues/2450
synced_at: 2026-01-10T01:23:17Z
---

# `uv venv` is broken on `macos-latest` github CI runner

---

_Issue opened by @yashgorana on 2024-03-14 06:33_

Just creating the virtual env triggers an error from uv version 0.1.19. From the JSON dump, it seems like uv is incorrectly determining the platform arch to be i386.

```
bash-3.2$ sw_vers
ProductName:    macOS
ProductVersion: 12.7.3
BuildVersion:   21H1015

bash-3.2$ uname -a
Darwin Mac-1710396159726.local 21.6.0 Darwin Kernel Version 21.6.0: Sun Dec 17 22:55:27 PST 2023; root:xnu-8020.240.18.706.2~1/RELEASE_X86_64 x86_64

bash-3.2$ uv --version
uv 0.1.20 (ea8fc8280 2024-03-13)

bash-3.2$ uv venv
  × Querying Python at `/Users/runner/hostedtoolcache/Python/3.12.2/x64/bin/python3` did not return the expected data: unknown variant `i386`, expected one of `aarch64`,
  │ `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x` with exit status: 0
  │ --- stdout:
  │ {"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.12.2", "os_name": "posix", "platform_machine": "x86_64",
  │ "platform_python_implementation": "CPython", "platform_release": "21.6.0", "platform_system": "Darwin", "platform_version": "Darwin Kernel Version 21.6.0:
  │ Sun Dec 17 22:55:27 PST 2023; root:xnu-8020.240.18.706.2~1/RELEASE_X86_64", "python_full_version": "3.12.2", "python_version": "3.12", "sys_platform":
  │ "darwin"}, "base_prefix": "/Library/Frameworks/Python.framework/Versions/3.12", "base_exec_prefix": "/Library/Frameworks/Python.framework/Versions/3.12",
  │ "prefix": "/Library/Frameworks/Python.framework/Versions/3.12", "base_executable": "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3.12",
  │ "sys_executable": "/Library/Frameworks/Python.framework/Versions/3.12/bin/python3", "stdlib": "/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12",
  │ "scheme": {"platlib": "/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages", "purelib":
  │ "/Library/Frameworks/Python.framework/Versions/3.12/lib/python3.12/site-packages", "include": "/Library/Frameworks/Python.framework/Versions/3.12/include/python3.12",
  │ "scripts": "/Library/Frameworks/Python.framework/Versions/3.12/bin", "data": "/Library/Frameworks/Python.framework/Versions/3.12"}, "virtualenv": {"purelib":
  │ "lib/python3.12/site-packages", "platlib": "lib/python3.12/site-packages", "include": "include/site/python3.12", "scripts": "bin", "data": ""}, "platform": {"os":
  │ {"name": "macos", "major": 12, "minor": 7}, "arch": "i386"}}
  │ --- stderr:

  │ ---
```

Works fine when uv is reverted to 0.1.18

```
bash-3.2$ pip install --upgrade uv==0.1.18 > /dev/null

bash-3.2$ uv --version
uv 0.1.18 (43dc9c87a 2024-03-13)

bash-3.2$ uv venv
Using Python 3.12.2 interpreter at: /Library/Frameworks/Python.framework/Versions/3.12/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

Bug probably is due to this PR - https://github.com/astral-sh/uv/pull/2381

---

_Label `macos` added by @AlexWaygood on 2024-03-14 07:27_

---

_Comment by @AlexWaygood on 2024-03-14 07:31_

I'm also seeing this in CI for typeshed-stats: https://github.com/AlexWaygood/typeshed-stats/actions/runs/8273839124/job/22638277336 (https://github.com/AlexWaygood/typeshed-stats/issues/204)

And for typeshed: https://github.com/python/typeshed/issues/11594#issuecomment-1996257396 

---

_Label `virtualenv` added by @AlexWaygood on 2024-03-14 07:33_

---

_Referenced in [python/typeshed#11598](../../python/typeshed/pulls/11598.md) on 2024-03-14 10:51_

---

_Referenced in [AlexWaygood/typeshed-stats#204](../../AlexWaygood/typeshed-stats/issues/204.md) on 2024-03-14 10:53_

---

_Referenced in [mxstack/mxmake#33](../../mxstack/mxmake/pulls/33.md) on 2024-03-14 11:03_

---

_Comment by @jensens on 2024-03-14 11:07_

This fails when uv 0.1.20 is used in Github Actions with `macos-latest` runners. My configuration is:
```
jobs:
  build:
    strategy:
      fail-fast: false
      matrix:
        python-version:
        - "3.8"
        - "3.9"
        - "3.10"
        - "3.11"
        - "3.12"
        os:
          - ubuntu-latest
          - macos-latest
          - windows-latest
...
```
The only failing combinations (with above mentioned error) are 
- "3.11" with "macos-latest" 
- "3.12" with "macos-latest" 

---

_Comment by @tom-engineering on 2024-03-14 11:32_

I think this is also preventing the version of `uv` from updating in Homebrew: https://github.com/Homebrew/homebrew-core/actions/runs/8276916341/job/22646364312

```
Full test uv output
  /usr/local/Homebrew/Library/Homebrew/vendor/bundle/ruby/3.1.0/bin/bundle clean
  ==> Testing uv
  ==> /usr/local/Cellar/uv/0.1.20/bin/uv pip compile -q requirements.in
  error: Querying Python at `/usr/bin/python3` did not return the expected data: unknown variant `i386`, expected one of `aarch64`, `armv7l`, `powerpc64le`, `powerpc64`, `x86`, `x86_64`, `s390x` with exit status: 0
  ```

---

_Referenced in [astral-sh/uv#2454](../../astral-sh/uv/pulls/2454.md) on 2024-03-14 12:28_

---

_Closed by @konstin on 2024-03-14 12:37_

---

_Referenced in [zenml-io/zenml#2529](../../zenml-io/zenml/pulls/2529.md) on 2024-03-14 14:11_

---
