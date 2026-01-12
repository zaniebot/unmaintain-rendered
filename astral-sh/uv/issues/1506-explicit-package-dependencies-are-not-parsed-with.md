```yaml
number: 1506
title: "explicit `package @ .` dependencies are not parsed with `--editable` but should be"
type: issue
state: closed
author: altendky
labels:
  - bug
  - compatibility
assignees: []
created_at: 2024-02-16T15:53:47Z
updated_at: 2024-02-18T03:10:56Z
url: https://github.com/astral-sh/uv/issues/1506
synced_at: 2026-01-12T15:58:29Z
```

# explicit `package @ .` dependencies are not parsed with `--editable` but should be

---

_@altendky_

```console
$ git clone https://github.com/twisted/twisted .
Cloning into '.'...
remote: Enumerating objects: 268072, done.
remote: Counting objects: 100% (204/204), done.
remote: Compressing objects: 100% (136/136), done.
remote: Total 268072 (delta 60), reused 179 (delta 49), pack-reused 267868
Receiving objects: 100% (268072/268072), 72.18 MiB | 28.63 MiB/s, done.
Resolving deltas: 100% (202789/202789), done.
```

```console
$ git checkout 446ee139189440e890b26a29af256e9b9d0e8eba
<helpful git reminder about disabling the detached head warning snipped for brevity>

HEAD is now at 446ee13918 Fix chat.py example (#12070)
```

```console
$ ls -la
total 312
drwxrwxr-x  8 altendky altendky   4096 Feb 16 10:49 .
drwxrwxr-x 13 altendky altendky   4096 Feb 16 10:48 ..
drwxrwxr-x  2 altendky altendky   4096 Feb 16 10:49 admin
drwxrwxr-x  3 altendky altendky   4096 Feb 16 10:49 bin
-rw-rw-r--  1 altendky altendky   1418 Feb 16 10:49 codecov.yml
-rw-rw-r--  1 altendky altendky   2675 Feb 16 10:49 code_of_conduct.md
-rw-rw-r--  1 altendky altendky    828 Feb 16 10:49 CONTRIBUTING.md
-rw-rw-r--  1 altendky altendky    361 Feb 16 10:49 .coveragerc
drwxrwxr-x 15 altendky altendky   4096 Feb 16 10:49 docs
drwxrwxr-x  8 altendky altendky   4096 Feb 16 10:49 .git
-rw-rw-r--  1 altendky altendky     11 Feb 16 10:49 .gitattributes
-rw-rw-r--  1 altendky altendky    506 Feb 16 10:49 .git-blame-ignore-revs
drwxrwxr-x  4 altendky altendky   4096 Feb 16 10:49 .github
-rw-rw-r--  1 altendky altendky    225 Feb 16 10:49 .gitignore
-rw-rw-r--  1 altendky altendky    840 Feb 16 10:49 hatch_build.py
-rw-rw-r--  1 altendky altendky   1109 Feb 16 10:49 INSTALL.rst
-rw-rw-r--  1 altendky altendky   1942 Feb 16 10:49 LICENSE
-rw-rw-r--  1 altendky altendky 153171 Feb 16 10:49 NEWS.rst
-rw-rw-r--  1 altendky altendky    657 Feb 16 10:49 .pre-commit-config.yaml
-rw-rw-r--  1 altendky altendky  27847 Feb 16 10:49 pyproject.toml
-rw-rw-r--  1 altendky altendky   4521 Feb 16 10:49 README.rst
-rw-rw-r--  1 altendky altendky   1108 Feb 16 10:49 .readthedocs.yml
-rw-rw-r--  1 altendky altendky  29408 Feb 16 10:49 SECURITY.md
-rw-rw-r--  1 altendky altendky   3720 Feb 16 10:49 setup.cfg
drwxrwxr-x  3 altendky altendky   4096 Feb 16 10:49 src
-rw-rw-r--  1 altendky altendky   7136 Feb 16 10:49 tox.ini
```

```console
$ python3.11 -m venv venv
```

```console
$ source venv/bin/activate
```

```console
(venv) $ python -m pip install 'uv @ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6'
Collecting uv@ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6
  Using cached uv-0.1.2-py3-none-linux_x86_64.whl
Installing collected packages: uv
Successfully installed uv-0.1.2

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: pip install --upgrade pip
```

```console
(venv) $ python -m uv pip install --force-reinstall --editable "twisted @ ."
error: Failed to build editables
  Caused by: Failed to build editable: file:///home/altendky/tmp/t/twisted%20@%20.
  Caused by: Source distribution not found at: /home/altendky/tmp/t/twisted @ .
```

Note how `--editable` above seems to simply use the entire package spec as a path.  The path should just be `file:///home/altendky/tmp/t` with an explicit package name of `twisted` specified unrelated to the path.  In my real case I was working with extras, though it seems to turn out that that is unrelated.

```console
(venv) $ python -m uv pip install --force-reinstall "twisted @ ."
Resolved 11 packages in 337ms
   Built twisted @ file:///home/altendky/tmp/t                                                                                                                                               Downloaded 1 package in 802ms
Installed 11 packages in 23ms
 + attrs==23.2.0
 + automat==22.10.0
 + constantly==23.10.4
 + hyperlink==21.0.0
 + idna==3.6
 + incremental==22.10.0
 - setuptools==65.5.0
 + setuptools==69.1.0
 + six==1.16.0
 + twisted==23.10.0.post0 (from file:///home/altendky/tmp/t)
 + typing-extensions==4.9.0
 + zope-interface==6.2
```

---

_Label `bug` added by @zanieb on 2024-02-16 15:59_

---

_Label `compatibility` added by @zanieb on 2024-02-16 15:59_

---

_Comment by @zanieb on 2024-02-16 15:59_

Thanks for the report! Looks like an oversight.

---

_Assigned to @charliermarsh by @zanieb on 2024-02-16 17:09_

---

_Comment by @charliermarsh on 2024-02-18 02:10_

Does `pip` support this? I can't get it to work:

```
❯ pip install -e "black @ file:///Users/crmarsh/workspace/puffin/scripts/editable-installs/black_editable"
ERROR: black @ file:///Users/crmarsh/workspace/puffin/scripts/editable-installs/black_editable is not a valid editable requirement. It should either be a path to a local project or a VCS URL (beginning with bzr+http, bzr+https, bzr+ssh, bzr+sftp, bzr+ftp, bzr+lp, bzr+file, git+http, git+https, git+ssh, git+git, git+file, hg+file, hg+http, hg+https, hg+ssh, hg+static-http, svn+ssh, svn+http, svn+https, svn+svn, svn+file).
```

---

_Comment by @altendky on 2024-02-18 03:10_

I agree, sorry.  Building upon my above example I get:

```console
(venv) $ python -m pip install --force-reinstall --editable "twisted @ ."
ERROR: twisted @ . is not a valid editable requirement. It should either be a path to a local project or a VCS URL (beginning with bzr+http, bzr+https, bzr+ssh, bzr+sftp, bzr+ftp, bzr+lp, bzr+file, git+http, git+https, git+ssh, git+git, git+file, hg+file, hg+http, hg+https, hg+ssh, hg+static-http, svn+ssh, svn+http, svn+https, svn+svn, svn+file).

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: pip install --upgrade pip
```

I got myself into this scenario as I worked around https://github.com/astral-sh/uv/issues/1499 / https://github.com/astral-sh/uv/issues/313 which were forcing me to specify a name instead of just a path for non-editable installs.  I have an install script that takes a parameter picking editable or not so I just used `.` with pip.  I guess for now I'll just have to special case the two cases of editable and not.

My apologies for leaving that out of my initial minimal recreation in which case I would have realized this shouldn't be reported.

Thanks for the prompt handling.

---

_Closed by @altendky on 2024-02-18 03:10_

---
