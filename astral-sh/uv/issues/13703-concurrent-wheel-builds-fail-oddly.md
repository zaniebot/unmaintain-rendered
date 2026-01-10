---
number: 13703
title: concurrent wheel builds fail oddly
type: issue
state: closed
author: thatch
labels:
  - bug
assignees: []
created_at: 2025-05-28T18:35:43Z
updated_at: 2025-06-26T19:28:16Z
url: https://github.com/astral-sh/uv/issues/13703
synced_at: 2026-01-10T01:25:36Z
---

# concurrent wheel builds fail oddly

---

_Issue opened by @thatch on 2025-05-28 18:35_

### Summary

with `t/setup.py` containing

```py
import setuptools
setuptools.setup(name="t", version="4")
```

`uv build --wheel & sleep 0.1; uv build --wheel` fails because the invocations are stepping on each other.  I had hoped this would lock something related to the path (or the `build` subdir) and block the second until the first is done, but that does not appear to be the case.


This is a simplified repro that was originally something like

```
# x-reqs.txt
t @ t/
...
```

```
# y-reqs.txt
t @ t/
...
```

```sh
(uv venv x; VIRTUAL_ENV=x uv pip install x-reqs.txt) &
(uv venv y; VIRTUAL_ENV=y uv pip install y-reqs.txt)
wait
```

The user assumed that installs from the same local dir would work fine as long as they were to different venvs.  The bug seems to be a missing lock during wheel builds.

I can ask them to change settings, but I don't think that `-e` or a pep517 build would improve the situation here.

### Platform

Ubuntu

### Version

Jammy

### Python version

3.12

---

_Label `bug` added by @thatch on 2025-05-28 18:35_

---

_Comment by @thatch on 2025-05-28 18:49_

Sample failure, showing a collision on PKG-INFO from concurrent builds:

```
yCopying t.egg-info to build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info
xwriting manifest file 't.egg-info/SOURCES.txt'
xremoving 'build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info' (and everything under it)
yerror: [Errno 2] No such file or directory: 'build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info/PKG-INFO'
xCopying t.egg-info to build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info
```
<details>
```
$ (uv build --wheel 2>&1 | sed -e 's/^/x/') & (uv build --wheel 2>&1 | sed -e 's/^/y/')
[1] 85628
xBuilding wheel...
yBuilding wheel...
^[[Cyrunning egg_info
xrunning egg_info
ywriting t.egg-info/PKG-INFO
xwriting t.egg-info/PKG-INFO
xwriting dependency_links to t.egg-info/dependency_links.txt
ywriting dependency_links to t.egg-info/dependency_links.txt
xwriting top-level names to t.egg-info/top_level.txt
ywriting top-level names to t.egg-info/top_level.txt
xreading manifest file 't.egg-info/SOURCES.txt'
yreading manifest file 't.egg-info/SOURCES.txt'
xwriting manifest file 't.egg-info/SOURCES.txt'
ywriting manifest file 't.egg-info/SOURCES.txt'
xrunning bdist_wheel
yrunning bdist_wheel
yrunning build
xrunning build
xinstalling to build/bdist.macosx-11.0-arm64/wheel
xrunning install
yinstalling to build/bdist.macosx-11.0-arm64/wheel
yrunning install
xrunning install_egg_info
yrunning install_egg_info
yrunning egg_info
xrunning egg_info
ywriting t.egg-info/PKG-INFO
xwriting t.egg-info/PKG-INFO
ywriting dependency_links to t.egg-info/dependency_links.txt
xwriting dependency_links to t.egg-info/dependency_links.txt
ywriting top-level names to t.egg-info/top_level.txt
xwriting top-level names to t.egg-info/top_level.txt
yreading manifest file 't.egg-info/SOURCES.txt'
xreading manifest file 't.egg-info/SOURCES.txt'
ywriting manifest file 't.egg-info/SOURCES.txt'
yCopying t.egg-info to build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info
xwriting manifest file 't.egg-info/SOURCES.txt'
xremoving 'build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info' (and everything under it)
yerror: [Errno 2] No such file or directory: 'build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info/PKG-INFO'
xCopying t.egg-info to build/bdist.macosx-11.0-arm64/wheel/./t-4-py3.13.egg-info
xrunning install_scripts
xcreating build/bdist.macosx-11.0-arm64/wheel/t-4.dist-info/WHEEL
xcreating '/private/tmp/t/dist/.tmp-dnwckr5d/t-4-py3-none-any.whl' and adding 'build/bdist.macosx-11.0-arm64/wheel' to it
xadding 't-4.dist-info/METADATA'
xadding 't-4.dist-info/WHEEL'
xadding 't-4.dist-info/top_level.txt'
xadding 't-4.dist-info/RECORD'
xremoving build/bdist.macosx-11.0-arm64/wheel
xSuccessfully built dist/t-4-py3-none-any.whl
y  × Failed to build `/private/tmp/t`
y  ├─▶ The build backend returned an error
y  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)
y      hint: This usually indicates a problem with the package or the build environment.
[1]  + done       ( uv build --wheel 2>&1 | sed -e 's/^/x/'; )
```
</details>

---

_Comment by @charliermarsh on 2025-05-28 18:50_

I guess we should lock the build directory, kind of unfortunate though.

---

_Comment by @thatch on 2025-05-28 18:55_

Yeah I don't see great options here -- calling the build directory anything but "build" people would need to change gitignores.  Is there anything we can enable that would make implicit builds use a tempdir instead?

I'm reluctant to add manual locks on my end because the obvious place to do that would be at the `x` / `y` level, and that's not where the conflict is happening, it's on the dep.

---

_Referenced in [astral-sh/uv#13883](../../astral-sh/uv/issues/13883.md) on 2025-06-06 11:58_

---

_Assigned to @oconnor663 by @zanieb on 2025-06-20 14:57_

---

_Comment by @oconnor663 on 2025-06-20 21:45_

I can't always reproduce the failure with the exact steps above, but this reliably repros it with 10 concurrent builds on my machine (current `main`):

```
$ cat >setup.py <<EOF
import setuptools
setuptools.setup(name="scratch", version="4")
EOF
$ for i in `seq 10` ; do
    uv build --wheel &
done
[...]
$   × Failed to build `/tmp/tmp.wGbtw5vwhy`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)
      hint: This usually indicates a problem with the package or the build environment.
[...]
```

---

_Referenced in [astral-sh/uv#14174](../../astral-sh/uv/pulls/14174.md) on 2025-06-20 23:30_

---

_Closed by @oconnor663 on 2025-06-26 19:28_

---
