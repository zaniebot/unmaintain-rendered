```yaml
number: 17041
title: coverage testing under uv markedly slower than in traditional venv
type: issue
state: open
author: jepler
labels:
  - bug
  - performance
  - great writeup
assignees: []
created_at: 2025-12-09T02:02:40Z
updated_at: 2026-01-09T12:53:23Z
url: https://github.com/astral-sh/uv/issues/17041
synced_at: 2026-01-12T16:02:43Z
```

# coverage testing under uv markedly slower than in traditional venv

---

_@jepler_

### Summary

In a particular project & configuration, `uv run` takes over 4.5x as long to run specific tests as `venv`+`pip`. Subprocesses & the `sys.monitoring` interface are implicated.

Steps to reproduce:
1. `git clone https://codeberg.org/jepler/wwvbpy.git`
1. `cd wwvbpy`
1. `git checkout 97bb1601e8ea9e246df698547f8c845156ddb1e6`
1. `docker run --rm -it -v=.:/work -w /work python:3.14-trixie bash`
1. Run the following shell script in docker:
    ```python
    coverage=coverage==7.11.2
    core=sysmon

    rm -rf .venv
    echo "venv+pip: coveragepy $coverage core $core"
    python3.14 -m venv .venv
    .venv/bin/python -m pip install -q $coverage 
    .venv/bin/python -m pip install -q .
    env PYTHONPATH=src COVERAGE_CORE=$core .venv/bin/python -m coverage run -m unittest discover -s test 2>&1 | grep 'Ran'
    rm -rf .venv

    echo "uv run: coveragepy $coverage core $core"
    env PYTHONPATH=src COVERAGE_CORE=$core uv run --python=3.14 --reinstall --refresh --no-cache --with $coverage python -m coverage run -m unittest discover -s test 2>&1 | grep 'Ran'
    rm -rf .venv
    ```

Typical result on my laptop:
```
venv+pip: coveragepy coverage==7.11.2 core sysmon
Ran 43 tests in 2.758s
uv run: coveragepy coverage==7.11.2 core sysmon
Ran 43 tests in 8.724s
```

Using `unittest ... --durations 10` and removing the grep it can be seen that the tests in `testcli` have most greatly increased in runtime. Locally, test_gen takes 0.949s with `venv`+`pip` and 4.413s with `uv`, over 4x as long. On the other hand, the 10th slowest test was `test_onset`, and took 0.092s and 0.099s under each environment, essentially unchanged.

Notable things:
 * This performance difference mostly or entirely disappears if the `ctrace` core of coveragepy is used instead
 * Likewise if coverage testing is not used at all
 * The tests that become much slower all create subprocesses of sys.executable. coveragepy ensures coverage runs in the sub-program through a .pth file.
 * Some change in coverage between 7.11.0 and 7.11.2 made the performance penalty become much larger. This is being discussed at https://github.com/coveragepy/coveragepy/issues/2082#issuecomment-3627450249 but at this point I don't think the exact relevant change is known.


### Platform

Linux 6.12.57+deb13-amd64 x86_64 GNU/Linux (debian trixie) using podman for containers

### Version

uv 0.9.16 (installed with `pip install uv`)

### Python version

Python 3.14.1 (container image python:3.14-trixie)

---

_Label `bug` added by @jepler on 2025-12-09 02:02_

---

_Comment by @zanieb on 2025-12-10 13:43_

Thanks for the report!

---

_Label `performance` added by @zanieb on 2025-12-10 13:43_

---

_Label `great writeup` added by @konstin on 2025-12-10 13:50_

---

_Comment by @konstin on 2025-12-10 13:53_

The problem is related to the temporary environment uv creates. If it uses the current venv, it's fast:

* fast (uv see the installed coverage and doesn't create a layered venv): `uv venv -c -p 3.14 && uv pip install $coverage && uv run --with $coverage -v python -m coverage run -m unittest discover -s test`
* slow: `uv venv -c -p 3.14 && run --with $coverage -v python -m coverage run -m unittest discover -s test`

---

_Comment by @jepler on 2025-12-11 18:21_

OK so I actually _looked_ at the coverage output.

In the "uv slow" case, coverage gathers data on files within `.venv/lib/python3.14/site-packages/` while in the "uv fast" case it does not.

This is likely because coverage is using a heuristic that excludes items under `platlib`, but when the uv environment is layered this does not include the site-packages that are in the base venv.

Typical sysconfig in a layered venv:
```python
>>> import sysconfig
>>> for x in sysconfig.get_path_names(): print(f"{x:<20} {sysconfig.get_path(x)}")
... 
stdlib               /usr/local/lib/python3.14
platstdlib           /root/.cache/uv/builds-v0/.tmp1R7Z57/lib/python3.14
purelib              /root/.cache/uv/builds-v0/.tmp1R7Z57/lib/python3.14/site-packages
platlib              /root/.cache/uv/builds-v0/.tmp1R7Z57/lib/python3.14/site-packages
include              /usr/local/include/python3.14
scripts              /root/.cache/uv/builds-v0/.tmp1R7Z57/bin
data                 /root/.cache/uv/builds-v0/.tmp1R7Z57
```

```
# coverage report output, fast case, running test subset (*oops actually from traditional venv)
Name                    Stmts   Miss Branch BrPart  Cover
---------------------------------------------------------
src/wwvb/__init__.py      562    106    188     41    75%
src/wwvb/decode.py         50      3     28      6    88%
src/wwvb/dut1table.py      17      0      4      0   100%
src/wwvb/gen.py            49      0     16      0   100%
src/wwvb/iersdata.py       15      0      0      0   100%
src/wwvb/tz.py              3      0      0      0   100%
test/testcli.py            64      4      0      0    94%
---------------------------------------------------------
TOTAL                     760    113    236     47    80%
```
```
# partial coverage report output, slow case, running test subset
Name                                                              Stmts   Miss Branch BrPart  Cover
---------------------------------------------------------------------------------------------------
.venv/lib/python3.14/site-packages/click/__init__.py                 83     19      8      0    70%
.venv/lib/python3.14/site-packages/click/_compat.py                 317    215     86      7    27%
.venv/lib/python3.14/site-packages/click/_textwrap.py                33     24      8      0    22%
.venv/lib/python3.14/site-packages/click/_utils.py                   16      1      0      0    94%
...
src/wwvb/tz.py                                                        3      0      0      0   100%
test/testcli.py                                                      64      4      0      0    94%
---------------------------------------------------------------------------------------------------
TOTAL                                                              7397   4033   2726    347    39%
```

so coverage during the "run" phase has done a lot of extra work on 30 files in each subprocess, adding a lot of runtime.

This perhaps means it's not a uv bug, though it certainly still feels like a uv gotcha.
Is there a different/better heuristic that coveragepy should be using to exclude the files from uv layers? This is how coverage describes what it intends its default measurement set to be:
> When running your code, the coverage run command will by default measure all code, unless it is part of the Python standard library.

---

_Comment by @nedbat on 2025-12-12 14:08_

Coverage uses sysconfig to try to understand where third-party code is installed.  This is clearly too simplistic.

`uv --with` creates a more complex `sys.path`, so third-party libraries are importable, but not in places sysconfig describes.

I did an experiment creating environments and running a simple program various ways, then looking at sysconfig and where a third-party library was installed. Full details are below, but the tl;dr is that using `uv --with`, libraries are put in an `archive-v0` directory that is not mentioned in any sysconfig library entry.

I guess this isn't a uv bug, since modules can be put anywhere as long as that place is mentioned in `sys.path`.  But uv seems unusual among environment creators in this way.  Do you have advice for how to properly understand these environments?

<details>
<summary>Full details of the experment</summary>

`show-libs.py`:

```python
# /// script
# dependencies = [
#   "requests",
# ]
# ///

import sys
import sysconfig

import requests

print("sys.path:")
for p in sys.path:
    print(f"    {p}")

lib_dirs = set()
print("sysconfig:")
for scheme in sorted(sysconfig.get_scheme_names()):
    print(f"    {scheme}:")
    for k, v in sysconfig.get_paths(scheme).items():
        print(f"        {k}: {v}")
        if k.endswith("lib"):
            lib_dirs.add(v)

print(f"{requests.__file__ = }")
is_in_lib = any(requests.__file__.startswith(d) for d in lib_dirs)
print(f"requests {'is' if is_in_lib else 'IS NOT'} detected in libs")
```

`show-bug.sh`:

```bash
rm -rf .venv
echo "========= venv+pip: "
python3.14 -m venv .venv
.venv/bin/python -m pip install -q requests
.venv/bin/python show-libs.py

rm -rf .venv
echo
echo
echo "========= uv run: "
uv run --python=3.14 --reinstall --refresh --no-cache show-libs.py

rm -rf .venv
echo
echo
echo "========= uv run --with requests: "
uv run --python=3.14 --reinstall --refresh --no-cache --with=requests show-libs.py

rm -rf .venv
echo
echo
echo "========= uv run --with coverage coverage run: "
uv run --python=3.14 --reinstall --refresh --no-cache --with=requests --with=coverage coverage run show-libs.py

rm -rf .venv
```

```
% . ./show-bug.sh
========= venv+pip:
sys.path:
    /private/tmp/uvrepro
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python314.zip
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14/lib-dynload
    /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
sysconfig:
    nt:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /private/tmp/uvrepro/.venv/Lib
        purelib: /private/tmp/uvrepro/.venv/Lib/site-packages
        platlib: /private/tmp/uvrepro/.venv/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /private/tmp/uvrepro/.venv/Scripts
        data: /private/tmp/uvrepro/.venv
    nt_user:
        stdlib: /Users/ned/.local/Python
        platstdlib: /Users/ned/.local/Python
        purelib: /Users/ned/.local/Python/site-packages
        platlib: /Users/ned/.local/Python/site-packages
        include: /Users/ned/.local/Python/Include
        scripts: /Users/ned/.local/Python/Scripts
        data: /Users/ned/.local
    nt_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /private/tmp/uvrepro/.venv/Lib
        purelib: /private/tmp/uvrepro/.venv/Lib/site-packages
        platlib: /private/tmp/uvrepro/.venv/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /private/tmp/uvrepro/.venv/Scripts
        data: /private/tmp/uvrepro/.venv
    osx_framework_user:
        stdlib: /Users/ned/.local/lib/python
        platstdlib: /Users/ned/.local/lib/python
        purelib: /Users/ned/.local/lib/python/site-packages
        platlib: /Users/ned/.local/lib/python/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_home:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python
        platstdlib: /private/tmp/uvrepro/.venv/lib/python
        purelib: /private/tmp/uvrepro/.venv/lib/python
        platlib: /private/tmp/uvrepro/.venv/lib/python
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        scripts: /private/tmp/uvrepro/.venv/bin
        data: /private/tmp/uvrepro/.venv
    posix_prefix:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /private/tmp/uvrepro/.venv/lib/python3.14
        purelib: /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
        platlib: /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /private/tmp/uvrepro/.venv/bin
        data: /private/tmp/uvrepro/.venv
    posix_user:
        stdlib: /Users/ned/.local/lib/python3.14
        platstdlib: /Users/ned/.local/lib/python3.14
        purelib: /Users/ned/.local/lib/python3.14/site-packages
        platlib: /Users/ned/.local/lib/python3.14/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /private/tmp/uvrepro/.venv/lib/python3.14
        purelib: /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
        platlib: /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /private/tmp/uvrepro/.venv/bin
        data: /private/tmp/uvrepro/.venv
    venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /private/tmp/uvrepro/.venv/lib/python3.14
        purelib: /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
        platlib: /private/tmp/uvrepro/.venv/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /private/tmp/uvrepro/.venv/bin
        data: /private/tmp/uvrepro/.venv
requests.__file__ = '/private/tmp/uvrepro/.venv/lib/python3.14/site-packages/requests/__init__.py'
requests is detected in libs


========= uv run:
Installed 5 packages in 5ms
sys.path:
    /private/tmp/uvrepro
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python314.zip
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14/lib-dynload
    /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
sysconfig:
    nt:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Lib
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Lib/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Scripts
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931
    nt_user:
        stdlib: /Users/ned/.local/Python
        platstdlib: /Users/ned/.local/Python
        purelib: /Users/ned/.local/Python/site-packages
        platlib: /Users/ned/.local/Python/site-packages
        include: /Users/ned/.local/Python/Include
        scripts: /Users/ned/.local/Python/Scripts
        data: /Users/ned/.local
    nt_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Lib
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Lib/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/Scripts
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931
    osx_framework_user:
        stdlib: /Users/ned/.local/lib/python
        platstdlib: /Users/ned/.local/lib/python
        purelib: /Users/ned/.local/lib/python/site-packages
        platlib: /Users/ned/.local/lib/python/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_home:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931
    posix_prefix:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931
    posix_user:
        stdlib: /Users/ned/.local/lib/python3.14
        platstdlib: /Users/ned/.local/lib/python3.14
        purelib: /Users/ned/.local/lib/python3.14/site-packages
        platlib: /Users/ned/.local/lib/python3.14/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931
    venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931
requests.__file__ = '/var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmp0QrwcE/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages/requests/__init__.py'
requests is detected in libs


========= uv run --with requests:
Installed 5 packages in 5ms
Installed 5 packages in 3ms
sys.path:
    /private/tmp/uvrepro
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python314.zip
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14/lib-dynload
    /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
    /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/archive-v0/AV0hz1y6x3O5CM6vZoVVa/lib/python3.14/site-packages
    /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/environments-v2/show-libs-cd2a7c0478475931/lib/python3.14/site-packages
sysconfig:
    nt:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Lib
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Lib/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Scripts
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc
    nt_user:
        stdlib: /Users/ned/.local/Python
        platstdlib: /Users/ned/.local/Python
        purelib: /Users/ned/.local/Python/site-packages
        platlib: /Users/ned/.local/Python/site-packages
        include: /Users/ned/.local/Python/Include
        scripts: /Users/ned/.local/Python/Scripts
        data: /Users/ned/.local
    nt_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Lib
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Lib/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/Scripts
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc
    osx_framework_user:
        stdlib: /Users/ned/.local/lib/python
        platstdlib: /Users/ned/.local/lib/python
        purelib: /Users/ned/.local/lib/python/site-packages
        platlib: /Users/ned/.local/lib/python/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_home:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc
    posix_prefix:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc
    posix_user:
        stdlib: /Users/ned/.local/lib/python3.14
        platstdlib: /Users/ned/.local/lib/python3.14
        purelib: /Users/ned/.local/lib/python3.14/site-packages
        platlib: /Users/ned/.local/lib/python3.14/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc
    venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/builds-v0/.tmpDxxoJc
requests.__file__ = '/var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpjXhSkp/archive-v0/AV0hz1y6x3O5CM6vZoVVa/lib/python3.14/site-packages/requests/__init__.py'
requests IS NOT detected in libs


========= uv run --with coverage coverage run:
Installed 6 packages in 6ms
sys.path:
    /private/tmp/uvrepro
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python314.zip
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14/lib-dynload
    /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
    /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/archive-v0/seeD9KJTh7xRnPcuwKCx1/lib/python3.14/site-packages
    /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14/site-packages
sysconfig:
    nt:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Lib
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Lib/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Scripts
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO
    nt_user:
        stdlib: /Users/ned/.local/Python
        platstdlib: /Users/ned/.local/Python
        purelib: /Users/ned/.local/Python/site-packages
        platlib: /Users/ned/.local/Python/site-packages
        include: /Users/ned/.local/Python/Include
        scripts: /Users/ned/.local/Python/Scripts
        data: /Users/ned/.local
    nt_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/Lib
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Lib
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Lib/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Lib/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/Include
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/Scripts
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO
    osx_framework_user:
        stdlib: /Users/ned/.local/lib/python
        platstdlib: /Users/ned/.local/lib/python
        purelib: /Users/ned/.local/lib/python/site-packages
        platlib: /Users/ned/.local/lib/python/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_home:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO
    posix_prefix:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO
    posix_user:
        stdlib: /Users/ned/.local/lib/python3.14
        platstdlib: /Users/ned/.local/lib/python3.14
        purelib: /Users/ned/.local/lib/python3.14/site-packages
        platlib: /Users/ned/.local/lib/python3.14/site-packages
        include: /Users/ned/.local/include/python3.14
        scripts: /Users/ned/.local/bin
        data: /Users/ned/.local
    posix_venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO
    venv:
        stdlib: /usr/local/pyenv/pyenv/versions/3.14.2/lib/python3.14
        platstdlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14
        purelib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
        platlib: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/lib/python3.14/site-packages
        include: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        platinclude: /usr/local/pyenv/pyenv/versions/3.14.2/include/python3.14
        scripts: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO/bin
        data: /var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/builds-v0/.tmp2XtokO
requests.__file__ = '/var/folders/6j/khn0mcrj35d1k3yylpl8zl080000gn/T/.tmpJ9AZKo/archive-v0/seeD9KJTh7xRnPcuwKCx1/lib/python3.14/site-packages/requests/__init__.py'
requests IS NOT detected in libs
```

</details>


---

_Comment by @zanieb on 2025-12-12 17:02_

We do write the parent prefix to a key to the `pyvenv.cfg`, if that's helpful?

https://github.com/astral-sh/uv/blob/705b35c552669ce9a503a15d373a8566a64256d5/crates/uv/src/commands/project/environment.rs#L60-L82

> But uv seems unusual among environment creators in this way. 

I don't think there are a lot of tools that use layered environments. I am curious if coverage would have problems with `venvstacks` too though (cc @ncoghlan).



---
