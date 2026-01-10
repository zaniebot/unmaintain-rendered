---
number: 4204
title: Improve message when distutil is not available on legacy python. 
type: issue
state: closed
author: mgaitan
labels:
  - help wanted
  - error messages
  - tracing
assignees: []
created_at: 2024-06-10T16:37:20Z
updated_at: 2025-02-19T23:41:53Z
url: https://github.com/astral-sh/uv/issues/4204
synced_at: 2026-01-10T01:23:35Z
---

# Improve message when distutil is not available on legacy python. 

---

_Issue opened by @mgaitan on 2024-06-10 16:37_

I use a local venv in python3.10 but compile requirements for python 3.8 (requirements used for dockerized apps) . I have both versions (along the default python 3.12) installed. 

Even when I have no my 3.10 venv active uv reports the following 

`warning: The requested Python version 3.8 is not available. 3.10.14 will be used` . But if I run `python3.8` it works. 


```
tin@morocha:~/lab/Project-API$ make dependencies 
UV_CUSTOM_COMPILE_COMMAND="make dependencies" uv pip compile --python-version 3.8 --generate-hashes --output-file app/requirements/requirements-dev.txt app/requirements/requirements-dev.in
warning: The requested Python version 3.8 is not available; 3.10.14 will be used to build dependencies instead.
⠙ aiohttp==3.9.5                                                                                                                                                                              ^Cmake: *** [Makefile:17: app/requirements/requirements-dev.txt] Interrupción

tin@morocha:~/lab/Project-API$ python3.8
Python 3.8.19 (default, Apr 27 2024, 21:20:48) 
[GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
tin@morocha:~/lab/Project-API$ which python3.8
/usr/bin/python3.8
tin@morocha:~/lab/Project-API$ python
Python 3.12.3 (main, Apr 10 2024, 05:33:47) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
tin@morocha:~/lab/Project-API$ which python3.10
/usr/bin/python3.10
tin@morocha:~/lab/Project-API$ python3.10
Python 3.10.14 (main, Apr 27 2024, 21:17:55) [GCC 13.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

Debugging with `--verbose` I found the cause 

```
DEBUG Starting interpreter discovery for Python 3.8
DEBUG Looking for exact match for request Python 3.8
DEBUG Searching for Python 3.8 in all sources
DEBUG Found CPython 3.10.14 at `/home/tin/lab/Project-API/.venv/bin/python3` (virtual environment)
DEBUG Querying Python at `/usr/bin/python3.8` failed with exit status exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/tin/.cache/uv/.tmpy3nPfq/python/get_interpreter_info.py", line 555, in main
    "scheme": get_scheme(),
  File "/home/tin/.cache/uv/.tmpy3nPfq/python/get_interpreter_info.py", line 415, in get_scheme
    return get_distutils_scheme()
  File "/home/tin/.cache/uv/.tmpy3nPfq/python/get_interpreter_info.py", line 357, in get_distutils_scheme
    from distutils.dist import Distribution
ModuleNotFoundError: No module named 'distutils.dist'
---
DEBUG Found CPython 3.12.3 at `/usr/bin/python3` (search path)
DEBUG Found CPython 3.12.3 at `/usr/bin/python` (search path)
DEBUG Querying Python at `/bin/python3.8` failed with exit status exit status: 1
--- stdout:
```

so after installing  `sudo apt install python3.8-distutils` it works. 

The error message could be clearer and indicate the exact issue, such as the missing distutils module, to help users resolve the problem more efficiently.


---

_Label `error messages` added by @zanieb on 2024-06-10 16:38_

---

_Label `tracing` added by @zanieb on 2024-06-10 16:39_

---

_Comment by @zanieb on 2024-06-10 16:40_

Thanks for the report. It should be relatively straight-forward to add a try/except with a better message in `get_interpreter_info` if someone is interested.

---

_Comment by @mgaitan on 2024-09-12 02:44_

is this fixed in https://github.com/astral-sh/uv/pull/7239 ?

---

_Comment by @acarapetis on 2024-11-11 03:47_

> is this fixed in #7239 ?

Nope, ran into this today with uv 0.5.1.

---

_Label `help wanted` added by @zanieb on 2024-11-11 15:08_

---

_Comment by @KyeRussell on 2025-01-17 01:59_

Hi. Just wanted to note that I've also hit this (on current latest `uv`: `0.5.20`). I don't mean to uselessly "+1!" 😁. I thought that it'd be worth noting my experience, both for prioritisation purposes and to hopefully help anyone else.

- I'm a seasoned Python developer (but evidently not one that does enough monkeying around with internals!), and I had little to no idea what was going on here.
- Google didn't help very much. I only ended up here after sifting through uv's GH issues.

I ran into this when 'moving' from Ubuntu (server) 20.04 to 24.04. To be clear, I mean a fresh install (well, really, changing my `Dockerfile`'s `FROM`). Not an actual system 'upgrade'!

In both cases, I'm using Python 3.9 (via the 'deadsnakes' PPA). `uv`'s Python binaries aren't suitable for our purposes.

- On Ubuntu 20.04, I can _just_ install the `python3.9` package, install `uv`, and set up a new environment pointing at my installed `python3.9` binary.
- On Ubuntu 24.04, I _also_ need to install the `python3.9-distutils` package. If I don't do so, I get the aforementioned very cryptic error implying that the interpreter (whose path I explicitly provide) doesn't actually exist.

This is all easily reproducible in a fresh Ubuntu Server 24.04 installation, FWIW.

---

_Comment by @zanieb on 2025-01-17 04:01_

I can look into adding better messaging here.

I don't really understand why the Python distributions aren't coming with distutils though? It was only removed from the Python standard library in 3.12, why is it missing from older Python versions here? Are the distributions stripping it from the standard library, or am I misunderstanding?

A brief Dockerfile reproduction would be helpful.

---

_Assigned to @zanieb by @zanieb on 2025-01-17 04:01_

---

_Comment by @zanieb on 2025-01-17 05:08_

This seems like some bespoke deadsnakes packaging. I tried to reproduce but I can't seem to configure deadsnakes. If someone procures a containerized reproduction, I'll fix it.

---

_Comment by @KyeRussell on 2025-01-17 05:08_

Working on it now :)

---

_Comment by @KyeRussell on 2025-01-17 05:33_

Heh, OK. So, it's a little different to what I thought / described.

When I previously isolated this, I showed the issue occurring in Ubuntu 24.04 + deadsnakes' `python3.9`.

This _is_ true, but it _also_ happens in Ubuntu 20.04 + deadsnakes' `python3.9`. It didn't affect me until now as I was unknowingly installing the [`python3-distutils` Ubuntu package](https://launchpad.net/ubuntu/focal/+package/python3-distutils) (_not_ a deadsnakes package) as a transient dependency of something else I had installed. In 24.04, I'm no longer installing this package. In fact, it seems to not be available for Ubuntu 24.04 at all. Ubuntu 24.04's system Python is 3.12.

So, seemingly, `python3-distutils` was available to uv / deadsnakes' Python 3.9, which is why I never saw this earlier. Which...seems a little scary. I don't know. I really know next to nothing about how Python is typically packaged.

Anyway, I've created a reproduction repository here: https://github.com/KyeRussell/uv-4204-reproduction. it contains:
- A series of Dockerfiles showing working and not-working scenarios for both Ubuntu 20.04 and 24.04.
- An [operational GitHub Actions workflow](https://github.com/KyeRussell/uv-4204-reproduction/actions) that tries to build each Dockerfile.

---

_Comment by @zanieb on 2025-01-17 05:43_

Thank you!

I need to figure out if we _should_ be failing or if we should just switchover to sysconfig when distutils is not installed.

---

_Referenced in [astral-sh/uv#10713](../../astral-sh/uv/pulls/10713.md) on 2025-01-17 15:53_

---

_Comment by @RomainBrault on 2025-01-21 15:05_

This one broke my project.

I a following pep 639. But when I'm using Python 3.9, setuptools/distutils cannot parse my pyproject.toml:

```bash
❯ uv sync
error: Querying Python at `/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/bin/python3.9` failed with exit status exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 609, in main
    "scheme": get_scheme(use_sysconfig_scheme),
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 407, in get_scheme
    return get_distutils_scheme()
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 363, in get_distutils_scheme
    d.parse_config_files()
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/dist.py", line 648, in parse_config_files
    pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 72, in apply_configuration
    config = read_configuration(filepath, True, ignore_option_errors, dist)
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 140, in read_configuration
    validate(subset, filepath)
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 61, in validate
    raise ValueError(f"{error}\n{summary}") from None
ValueError: invalid pyproject.toml config: `project.license`.
configuration error: `project.license` must be valid exactly by one definition (2 matches found):

    - keys:
        'file': {type: string}
      required: ['file']
    - keys:
        'text': {type: string}
      required: ['text']
---
```

this has some strange side effects, I cannot even list or find python 3.9 (even though it was installed with uv).

```bash
❌2 ❯ uv python find --verbose
DEBUG uv 0.5.21 (54bb5a38a 2025-01-21)
DEBUG Found project root: `/home/XXX/XXX/XXX/XXX`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/home/XXX/XXX/XXX/XXX/.python-versions`
DEBUG Using Python request `3.9` from version file at `.python-versions`
DEBUG Searching for Python 3.9 in virtual environments, managed installations, or search path
DEBUG Found `cpython-3.10.16-linux-x86_64-gnu` at `/home/XXX/XXX/XXX/XXX/.venv/bin/python3` (virtual environment)
DEBUG Skipping interpreter at `.venv/bin/python3` from virtual environment: does not satisfy request `3.9`
DEBUG Searching for managed installations at `/home/XXX/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.14.0a4-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.13.1-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.12.8-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.11.11-linux-x86_64-gnu`
DEBUG Skipping incompatible managed installation `cpython-3.10.16-linux-x86_64-gnu`
DEBUG Found managed installation `cpython-3.9.21-linux-x86_64-gnu`
DEBUG Failed to inspect Python interpreter from managed installations at `/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/bin/python3.9`
DEBUG Skipping bad interpreter at /home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/bin/python3.9 from managed installations: Querying Python at `/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/bin/python3.9` failed with exit status exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 609, in main
    "scheme": get_scheme(use_sysconfig_scheme),
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 407, in get_scheme
    return get_distutils_scheme()
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 363, in get_distutils_scheme
    d.parse_config_files()
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/dist.py", line 648, in parse_config_files
    pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 72, in apply_configuration
    config = read_configuration(filepath, True, ignore_option_errors, dist)
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 140, in read_configuration
    validate(subset, filepath)
  File "/home/XXX/.local/share/uv/python/cpython-3.9.21-linux-x86_64-gnu/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 61, in validate
    raise ValueError(f"{error}\n{summary}") from None
ValueError: invalid pyproject.toml config: `project.license`.
configuration error: `project.license` must be valid exactly by one definition (2 matches found):

    - keys:
        'file': {type: string}
      required: ['file']
    - keys:
        'text': {type: string}
      required: ['text']
---
DEBUG Found `cpython-3.12.7-linux-x86_64-gnu` at `/usr/bin/python3` (search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.9`
DEBUG Found `cpython-3.12.7-linux-x86_64-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.9`
DEBUG Found `cpython-3.12.7-linux-x86_64-gnu` at `/bin/python3` (search path)
DEBUG Skipping interpreter at `/bin/python3` from search path: does not satisfy request `3.9`
DEBUG Found `cpython-3.12.7-linux-x86_64-gnu` at `/bin/python` (search path)
DEBUG Skipping interpreter at `/bin/python` from search path: does not satisfy request `3.9`
error: No interpreter found for Python 3.9 in virtual environments, managed installations, or search path
```

I cannot create a venv using `uv venv` from my project either.

There's no problem with Python >= 3.10.

Everything was fine before this PR (#10713).

---

_Comment by @zanieb on 2025-01-21 15:37_

@RomainBrault I'm not sure how this could have changed from that pull request.

---

_Comment by @charliermarsh on 2025-01-21 15:59_

@zanieb -- I think that's real failure. The codepath that you moved the import out of has to disable the setuptools shim.

---

_Comment by @zanieb on 2025-01-21 16:01_

Oh interesting, that makes sense. I'll see what I can do.

---

_Comment by @RomainBrault on 2025-01-21 16:26_

I'm not sure I understand everything, I inferred it from the logs. Here's some more evidence.

```bash
❌2 ❯ TMPDIR=~/XXX cargo install --quiet --git https://github.com/astral-sh/uv.git --rev=4f31b44eac746f030a8569482554898aa01afc62 uv
XXX on  main [!] via 🐍 v3.12.7 took 7m54s
❯ uv sync
error: Querying Python at `/home/XXX/XXX/XXX/XXX/.venv/bin/python3` failed with exit status exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 1, in <module>
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 609, in main
    "scheme": get_scheme(use_sysconfig_scheme),
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 407, in get_scheme
    return get_distutils_scheme()
  File "/home/XXX/.cache/uv/.tmpXXX/python/get_interpreter_info.py", line 363, in get_distutils_scheme
    d.parse_config_files()
  File "/home/XXX/XXX/XXX/XXX/.venv/lib/python3.9/site-packages/_virtualenv.py", line 20, in parse_config_files
    result = old_parse_config_files(self, *args, **kwargs)
  File "/home/XXX/XXX/XXX/XXX/.venv/lib/python3.9/site-packages/setuptools/dist.py", line 652, in parse_config_files
    pyprojecttoml.apply_configuration(self, filename, ignore_option_errors)
  File "/home/XXX/XXX/XXX/XXX/.venv/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 72, in apply_configuration
    config = read_configuration(filepath, True, ignore_option_errors, dist)
  File "/home/XXX/XXX/XXX/XXX/.venv/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 140, in read_configuration
    validate(subset, filepath)
  File "/home/XXX/XXX/XXX/XXX/.venv/lib/python3.9/site-packages/setuptools/config/pyprojecttoml.py", line 61, in validate
    raise ValueError(f"{error}\n{summary}") from None
ValueError: invalid pyproject.toml config: `project.license`.
configuration error: `project.license` must be valid exactly by one definition (2 matches found):

    - keys:
        'file': {type: string}
      required: ['file']
    - keys:
        'text': {type: string}
      required: ['text']
---
XXX on  main [!] via 🐍 v3.12.7
❌2 ❯ TMPDIR=~/XXX cargo install --quiet --git https://github.com/astral-sh/uv.git --rev=8b6383ebe85eddaf888b70a9146fa75548d7eb82 uv
XXX on  main [!] via 🐍 v3.12.7 took 7m55s
❯ uv sync
Resolved 138 packages in 2ms
Audited 133 packages in 0.38ms
XXX on  main [!] via 🐍 v3.12.7
❯
```

---

_Referenced in [astral-sh/uv#10819](../../astral-sh/uv/pulls/10819.md) on 2025-01-21 17:31_

---

_Comment by @zanieb on 2025-01-21 18:53_

Thanks @RomainBrault ! That should be resolved in #10819 — I can't reproduce to confirm.

@KyeRussell the error message should be visible once https://github.com/astral-sh/uv/pull/10716 is merged.

---

_Closed by @zanieb on 2025-01-21 18:53_

---

_Comment by @dimaqq on 2025-02-19 06:25_

I wonder if I'm hitting the same issue or different.

I've added `license = "MIT"` in my fresh project, and I'm getting:

```
ValueError: invalid pyproject.toml config: `project.license`.
configuration error: `project.license` must be valid exactly by one definition (2 matches found):

    - keys:
        'file': {type: string}
      required: ['file']
    - keys:
        'text': {type: string}
      required: ['text']

  × Failed to build `/code/otlp-json`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_sdist` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
```

I was trying to follow https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

uv 0.6.1 (Homebrew 2025-02-17)

---

_Comment by @charliermarsh on 2025-02-19 11:31_

(Just for clarity, that’s not a uv issue. That’s setuptools enforcing a certain schema on your project.)

---

_Comment by @dimaqq on 2025-02-19 23:38_

I figured as much, my uv project with maturin backend is not affected.

Is there perhaps some solution? Maybe a better backend for simple projects?

---

_Comment by @zanieb on 2025-02-19 23:41_

We're working on it! #3957 

---
