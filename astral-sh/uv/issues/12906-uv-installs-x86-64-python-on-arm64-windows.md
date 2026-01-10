---
number: 12906
title: uv installs x86_64 Python on arm64 Windows 
type: issue
state: open
author: burgholzer
labels:
  - question
assignees: []
created_at: 2025-04-15T20:45:32Z
updated_at: 2025-11-28T01:23:00Z
url: https://github.com/astral-sh/uv/issues/12906
synced_at: 2026-01-10T01:25:26Z
---

# uv installs x86_64 Python on arm64 Windows 

---

_Issue opened by @burgholzer on 2025-04-15 20:45_

### Summary

Just yesterday, GitHub announced that the new Windows 11 ARM runners are now available as public preview.
We tried to switch one of our projects over at https://github.com/munich-quantum-toolkit/core/pull/926, but noticed that our Python testing CI would fail on the new runners.
The corresponding run log can be found here: https://github.com/munich-quantum-toolkit/core/actions/runs/14478587550/job/40610220668?pr=926
It essentially fails because uv is downloading the x86 version of Python, which then lets the package build run smoothly, but fails as soon as the tests get executed because these are the wrong binaries for the host architecture.

A minimum reproducible workflow that shows the failure is
```yaml
name: UV Reproducer
on:
  push:
    branches:
      - main
  pull_request:
  merge_group:
  workflow_dispatch:

jobs:
  debug-uv:
    name: Debug UV
    runs-on: windows-11-arm
    steps:
      - name: Install the latest version of uv
        uses: astral-sh/setup-uv@v5
      - name: Check the version of uv
        run: uv version
      - name: Instal Python with uv
        run: uv python install 3.9
      - name: Check the Python installations
        run: uv python list
```
A corresponding run with that can be found here: https://github.com/munich-quantum-toolkit/core/actions/runs/14478932500/job/40611314128?pr=926

There, the `uv python install 3.9` results in 
```console
Downloading cpython-3.9.22-windows-x86_64-none (21.2MiB)
 Downloaded cpython-3.9.22-windows-x86_64-none
Installed Python 3.9.22 in 2.61s
 + cpython-3.9.22-windows-x86_64-none
```

I saw over at the python-build-standalone repository that ARM64 builds are in progress (https://github.com/astral-sh/python-build-standalone/pull/387), but not yet ready.

### Platform

Windows 11 ARM

### Version

uv 0.6.14 (a4cec56dc 2025-04-09)

### Python version

Any

---

_Label `bug` added by @burgholzer on 2025-04-15 20:45_

---

_Comment by @zanieb on 2025-04-15 22:12_

Thanks for the report!

Does the x86-64 Python _run_ on the ARM machine? Just wondering.



---

_Comment by @burgholzer on 2025-04-15 22:15_

Judging from the workflow logs from our repository (specifically, https://github.com/munich-quantum-toolkit/core/actions/runs/14478587550/job/40610220668?pr=926#step:9:1045) I would say so.
pytest seems to start but fails in its collection phase when trying to load some DLL (which, presumably, has the wrong architecture).

---

_Comment by @zooba on 2025-04-17 16:18_

> Does the x86-64 Python _run_ on the ARM machine? Just wondering.

Yep. Windows ARM64 supports emulation of both x86 and x86-64 executables (and even supports some amount of bridging between them and native DLLs, though that's special magic that Python doesn't use and honestly I've not even fully understood yet). It's just as transparent as the 32-bit emulation on 64-bit OS, including that all DLLs/PYDs need to match the architecture of the executable.

Because most of the Python ecosystem assumes it can only ever run in the native architecture, a lot of libraries will fail if they're running under emulation. We've been trying to enlighten tools and build scripts for the last couple of years, but without actual runners, most projects haven't cared, so they still need updating.

---

_Comment by @charliermarsh on 2025-04-17 16:31_

I think there was an issue elsewhere where we discussed whether x86 actually _should_ be the default for ARM Windows machines, since so few packages publish ARM Windows wheels...?

---

_Comment by @burgholzer on 2025-04-17 16:34_

> I think there was an issue elsewhere where we discussed whether x86 actually _should_ be the default for ARM Windows machines, since so few packages publish ARM Windows wheels...?

I bet that will change over time now that public runners are available. Could be wrong though.

---

_Comment by @zooba on 2025-04-17 16:50_

Yeah, I'm also betting that the runners existing will make a difference. And also hesitant to make ARM the default - but that said, ARM should certainly be the default for anyone building their packages on an ARM machine. Maybe they'll be blocked by an upstream dependency, but for the vast majority of users, they _are_ an upstream dependency for someone else.

Making ARM builds the default on python.org means millions of "normal" users (e.g. school kids and such) will get the build. Package developers are a minority in these terms, which is why they need to opt in. I'm not sure where uv fits in here, but I suspect it's a higher ratio of people who _should_ get native ARM builds by default (but probably not high enough).

---

_Comment by @geofft on 2025-04-28 17:46_

+1 to the sentiment that we should make arm64 Python available but not yet the default. I think it shouldn't be the default in uv even when astral-sh/python-build-standalone#387 lands, and I _think_ the status quo is that uv itself is arm64 but it picks the x64 binary as the best available option (by the rules in crates/uv-python/src/platform.rs), right? So I think that means that we need to implement some toggle somewhere to make uv reject arm64 Python on arm64 Windows specifically.

 I think in particular I would advocate for
* by default, people who do a `irm https://astral.sh/uv/install.ps1 | iex` on Windows arm64 should have this toggle enabled, so that they get x64 Python (and use the built-in emulation), and probably get a warning/FYI about this either when installing uv or when downloading a Python build, with an explanation what's going on (maybe a link to a tracking issue about switching the default over?)
* there should be a documented way to turn this toggle off and opt into arm64 Python on Windows
* possibly, this toggle should be default _off_ for the astral-sh/setup-uv action, since people who are using GHA on Windows arm64 are presumably doing so on purpose and can easily switch to x64 if that doesn't work for them and don't want emulation (as opposed to e.g. people with an arm64 Windows laptop)
* we give ourselves some defined criteria for Windows arm64 wheel availability (definitely at least numpy, but probably we come up with a set of wheels), and flip the toggle off at that point, but keep the toggle available for users
* ideally but optionally, if we find ourselves trying to build something from source and we notice that there's a Windows x64 wheel, we should print a very visible warning that the user might have a better experience using x64 Python

---

_Referenced in [astral-sh/python-build-standalone#387](../../astral-sh/python-build-standalone/pulls/387.md) on 2025-04-28 17:49_

---

_Referenced in [astral-sh/uv#13719](../../astral-sh/uv/pulls/13719.md) on 2025-05-29 17:11_

---

_Referenced in [astral-sh/uv#13342](../../astral-sh/uv/issues/13342.md) on 2025-06-12 20:55_

---

_Renamed from "uv installs and uses x86 Python on new Windows 11 ARM GitHub runners" to "uv installs x86_64 Python on arm64 Windows " by @zanieb on 2025-06-12 20:55_

---

_Referenced in [Toufool/AutoSplit#328](../../Toufool/AutoSplit/pulls/328.md) on 2025-06-13 01:51_

---

_Referenced in [ankitects/anki#4079](../../ankitects/anki/issues/4079.md) on 2025-06-13 05:33_

---

_Label `bug` removed by @zanieb on 2025-06-13 12:54_

---

_Label `question` added by @zanieb on 2025-06-13 12:54_

---

_Referenced in [astral-sh/python-build-standalone#386](../../astral-sh/python-build-standalone/issues/386.md) on 2025-06-13 12:55_

---

_Comment by @henryiii on 2025-06-14 04:25_

NumPy 2.3.0 came out a few days ago and has Windows ARM binaries. It is changing, if slowly. The Windows ARM runners help a ton for package building. Adding them to cibuildwheel examples in https://github.com/pypa/cibuildwheel/pull/2468. _Long_ term I'd hope ARM was the default with a way to get x86.

---

_Comment by @saschanaz on 2025-06-14 16:07_

Can we make a prompt instead though? Either of default value is "wrong" which will only confuse people. 

---

_Referenced in [astral-sh/uv#13724](../../astral-sh/uv/pulls/13724.md) on 2025-06-14 18:16_

---

_Comment by @zanieb on 2025-06-14 18:24_

The current intent is to display a note https://github.com/astral-sh/uv/pull/13724

We don't often use interactive prompts, but we could consider adding that in the future.

---

_Comment by @artiga033 on 2025-08-06 07:20_

I did not notice that uv installed a x64 python by default for me until I faild to install a old numpy with newer python today, which does not hit any prebuilt wheels.

Please note that there are also real PC users using Windows on ARM, not only in CI.

On my WoA device I only installed msvc for ARM, thus fails to build a x64 package:

<details>
  <summary>`uv pip install` log</summary>


```
  × Failed to build `numpy==1.26.4`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `mesonpy.build_wheel` failed (exit code: 1)

      [stdout]
      + F:\cache\uv\builds-v0\.tmpFKrUSS\Scripts\python.exe
      F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src\vendored-meson\meson\meson.py
      setup F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src
      F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src\.mesonpy-7mpqi2n5
      -Dbuildtype=release -Db_ndebug=if-release -Db_vscrt=md
      --native-file=F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src\.mesonpy-7mpqi2n5\meson-python-native-file.ini
      The Meson build system
      Version: 1.2.99
      Source dir: F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src
      Build dir: F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src\.mesonpy-7mpqi2n5
      Build type: native build
      Project name: NumPy
      Project version: 1.26.4
      Activating VS 17.14.8
      C compiler for the host machine: cl (msvc 19.44.35211 "用于 ARM64 的 Microsoft (R) C/C++ 优化编译器 19.44.35211
      版")
      C linker for the host machine: link link 14.44.35211.0
      C++ compiler for the host machine: cl (msvc 19.44.35211 "用于 ARM64 的 Microsoft (R) C/C++ 优化编译器
      19.44.35211 版")
      C++ linker for the host machine: link link 14.44.35211.0
      Cython compiler for the host machine: cython (cython 3.0.12)
      Host machine cpu family: aarch64
      Host machine cpu: aarch64
      Program python found: YES (F:\cache\uv\builds-v0\.tmpFKrUSS\Scripts\python.exe)
      Need python for aarch64, but found x86_64
      Run-time dependency python found: NO (tried sysconfig)

      ..\meson.build:41:12: ERROR: Python dependency not found

      A full log can be found at
      F:\cache\uv\sdists-v9\pypi\numpy\1.26.4\x21PBL-6iJI3A-Kvz-4nC\src\.mesonpy-7mpqi2n5\meson-logs\meson-log.txt

      hint: This usually indicates a problem with the package or the build environment.
  help: `numpy` (v1.26.4) was included because `matplotlib` (v3.10.5) depends on `numpy`
```

</details>

While switched to aarch64 python it works well, with an acceptable compile time for me.

Also, as an ARM Windows Laptop user, I would prefer natively running instead of skipping compilation. Moreover it's not intitive to install and even list only x64 pythons when I'm even installed an arm version of uv.


---

_Referenced in [astral-sh/uv#15693](../../astral-sh/uv/issues/15693.md) on 2025-09-10 08:45_

---

_Referenced in [agntcy/slim#914](../../agntcy/slim/pulls/914.md) on 2025-11-06 07:20_

---

_Referenced in [jcrist/msgspec#959](../../jcrist/msgspec/pulls/959.md) on 2025-11-27 02:07_

---

_Comment by @ofek on 2025-11-27 02:12_

I hope that we can soon reassess the possibility of defaulting to native ARM64 now that much time has passed. This choice of defaulting to emulation was particularly troublesome to [debug](https://github.com/jcrist/msgspec/pull/959) today.

---

_Comment by @zooba on 2025-11-27 11:39_

Upstream we're switching the default [at the same time as 3.15's release](https://discuss.python.org/t/python-on-windows-arm64/104524), and I assume uv will follow about that same time.

---

_Comment by @thomthom on 2025-11-27 22:15_

> I hope that we can soon reassess the possibility of defaulting to native ARM64 now that much time has passed. This choice of defaulting to emulation was particularly troublesome to [debug](https://github.com/jcrist/msgspec/pull/959) today.

You mention defaulting to ARM64, is there an option today to install ARM64 python via UV?

I'm not seeing ARM in `uv python list`:

```
C:\Users\thoma\Source\py-tests>uv self update
info: Checking for updates...
success: Upgraded uv from v0.9.9 to v0.9.13! https://github.com/astral-sh/uv/releases/tag/0.9.13

C:\Users\thoma\Source\py-tests>uv python list
cpython-3.15.0a2-windows-x86_64-none                 <download available>
cpython-3.15.0a2+freethreaded-windows-x86_64-none    <download available>
cpython-3.14.0-windows-x86_64-none                   C:\Users\thoma\AppData\Roaming\uv\python\cpython-3.14.0-windows-x86_64-none\python.exe
cpython-3.14.0+freethreaded-windows-x86_64-none      <download available>
cpython-3.13.9-windows-x86_64-none                   <download available>
cpython-3.13.9+freethreaded-windows-x86_64-none      <download available>
cpython-3.13.2-windows-x86_64-none                   C:\Users\thoma\AppData\Roaming\uv\python\cpython-3.13.2-windows-x86_64-none\python.exe
cpython-3.12.12-windows-x86_64-none                  C:\Users\thoma\AppData\Roaming\uv\python\cpython-3.12.12-windows-x86_64-none\python.exe
cpython-3.11.14-windows-x86_64-none                  <download available>
cpython-3.11.9-windows-x86_64-none                   C:\Users\thoma\AppData\Local\Programs\Python\Python311\python.exe
cpython-3.10.19-windows-x86_64-none                  <download available>
cpython-3.9.25-windows-x86_64-none                   <download available>
cpython-3.8.20-windows-x86_64-none                   <download available>
pypy-3.11.13-windows-x86_64-none                     <download available>
pypy-3.10.16-windows-x86_64-none                     <download available>
pypy-3.9.19-windows-x86_64-none                      <download available>
pypy-3.8.16-windows-x86_64-none                      <download available>
graalpy-3.12.0-windows-x86_64-none                   <download available>
graalpy-3.11.0-windows-x86_64-none                   <download available>
graalpy-3.10.0-windows-x86_64-none                   <download available>
```

I got one of the new Surface tablets that are not exclusively ARM. While x86_x64 emulation works, it has a noticeable overhead on most apps. (Doesn't appear to be as efficient as Apple's emulation is).

---

_Comment by @charliermarsh on 2025-11-27 22:39_

I believe you can set `UV_PYTHON=arm64`.

---

_Comment by @thomthom on 2025-11-27 23:00_

That did the trick!

```
C:\Users\thoma\Source\py-tests>set UV_PYTHON=arm64

C:\Users\thoma\Source\py-tests>uv run python --version
Using CPython 3.14.0
Removed virtual environment at: .venv
Creating virtual environment at: .venv
Python 3.14.0

C:\Users\thoma\Source\py-tests>uv run python
Python 3.14.0 (main, Nov 19 2025, 23:05:28) [MSC v.1944 64 bit (ARM64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import platform
>>> platform.machine()
'ARM64'
```

So if one want this to be the machine default, one would set that in the system env? No other config for this?

---

_Comment by @artiga033 on 2025-11-28 01:23_

> I'm not seeing ARM in uv python list:

When listing you should use `uv python list --all-arches`, and you will see the aarch64 builds.

---
