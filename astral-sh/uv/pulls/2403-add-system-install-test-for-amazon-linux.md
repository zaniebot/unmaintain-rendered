```yaml
number: 2403
title: Add system install test for Amazon Linux
type: pull_request
state: closed
author: zanieb
labels:
  - testing
assignees: []
draft: true
base: main
head: zb/amazon
created_at: 2024-03-13T03:03:00Z
updated_at: 2024-03-13T15:05:56Z
url: https://github.com/astral-sh/uv/pull/2403
synced_at: 2026-01-12T16:05:01Z
```

# Add system install test for Amazon Linux

---

_@zanieb_

Supersedes https://github.com/astral-sh/uv/pull/1534

---

_Label `testing` added by @zanieb on 2024-03-13 03:03_

---

_Comment by @zanieb on 2024-03-13 03:41_

@charliermarsh this looks legitimately wrong? https://github.com/astral-sh/uv/actions/runs/8259113036/job/22592520139

Same failure over in #2402 

---

_Comment by @charliermarsh on 2024-03-13 03:51_

Not necessarily. It depends on what `sys.path` is. Where does `pip` install into? It's possible that that's the right place, and the directory just doesn't exist yet. (I genuinely don't know, I'm just saying it's possible we're wrong to assume the directory has to exist in advance.)

---

_Comment by @zanieb on 2024-03-13 03:58_

In CentOS (I just have the image around locally):

```
[root@bf9d80fad201 /]# python3 -c "import sys; print(sys.path)"
['', '/usr/lib64/python39.zip', '/usr/lib64/python3.9', '/usr/lib64/python3.9/lib-dynload', '/usr/lib64/python3.9/site-packages', '/usr/lib/python3.9/site-packages']
[root@d4f5af1c6ac6 /]# ls /usr/local/lib/python3.9/site-packages
ls: cannot access '/usr/local/lib/python3.9/site-packages': No such file or directory
[root@d4f5af1c6ac6 /]# python3 -m pip list
Package    Version
---------- -------
pip        20.2.4
setuptools 50.3.2
[root@bf9d80fad201 /]# python3 -m pip install pylint
WARNING: Running pip install with root privileges is generally not a good idea. Try `python3 -m pip install --user` instead.
Collecting pylint
  Downloading pylint-3.1.0-py3-none-any.whl (515 kB)
     |████████████████████████████████| 515 kB 2.2 MB/s 
Collecting mccabe<0.8,>=0.6
  Downloading mccabe-0.7.0-py2.py3-none-any.whl (7.3 kB)
Collecting astroid<=3.2.0-dev0,>=3.1.0
  Downloading astroid-3.1.0-py3-none-any.whl (275 kB)
     |████████████████████████████████| 275 kB 10.6 MB/s 
Collecting tomlkit>=0.10.1
  Downloading tomlkit-0.12.4-py3-none-any.whl (37 kB)
Collecting typing-extensions>=3.10.0; python_version < "3.10"
  Downloading typing_extensions-4.10.0-py3-none-any.whl (33 kB)
Collecting platformdirs>=2.2.0
  Downloading platformdirs-4.2.0-py3-none-any.whl (17 kB)
Collecting tomli>=1.1.0; python_version < "3.11"
  Downloading tomli-2.0.1-py3-none-any.whl (12 kB)
Collecting dill>=0.2; python_version < "3.11"
  Downloading dill-0.3.8-py3-none-any.whl (116 kB)
     |████████████████████████████████| 116 kB 11.4 MB/s 
Collecting isort!=5.13.0,<6,>=4.2.5
  Downloading isort-5.13.2-py3-none-any.whl (92 kB)
     |████████████████████████████████| 92 kB 662 kB/s 
Installing collected packages: mccabe, typing-extensions, astroid, tomlkit, platformdirs, tomli, dill, isort, pylint
Successfully installed astroid-3.1.0 dill-0.3.8 isort-5.13.2 mccabe-0.7.0 platformdirs-4.2.0 pylint-3.1.0 tomli-2.0.1 tomlkit-0.12.4 typing-extensions-4.10.0
[root@bf9d80fad201 /]# python3 -m pip show pylint
Name: pylint
Version: 3.1.0
Summary: python code static checker
Home-page: None
Author: None
Author-email: Python Code Quality Authority <code-quality@python.org>
License: GPL-2.0-or-later
Location: /usr/local/lib/python3.9/site-packages
Requires: platformdirs, isort, tomli, dill, astroid, tomlkit, typing-extensions, mccabe
Required-by: 
```

Suggests you're right — https://github.com/astral-sh/uv/issues/2404

---

_Comment by @charliermarsh on 2024-03-13 04:00_

Yeah it does look like `pip` is installing into the same place. Although why isn't `/usr/local/lib/python3.9/site-packages` on `sys.path`?

---

_Comment by @zanieb on 2024-03-13 04:08_

Oh yeah I read `/usr/lib/python3.9/site-packages` as `/usr/local/lib/...` umm I'm not sure :)

---

_Comment by @charliermarsh on 2024-03-13 04:12_

Can you check that `import pylint` or whatever works in that Python?

---

_Comment by @charliermarsh on 2024-03-13 04:16_

Or you can share the Dockerfile and I can debug it.

---

_Comment by @zanieb on 2024-03-13 14:13_

`[root@d4f5af1c6ac6 /]# python3 -c "import pylint"` works

I'm just copy-pasting the setup commands for CentOS from the CI workflow

---

_Comment by @zanieb on 2024-03-13 14:14_

Interestingly now the `sys.path` includes it

```
[root@d4f5af1c6ac6 /]# python3 -c "import sys; print(sys.path)" 
['', '/usr/lib64/python39.zip', '/usr/lib64/python3.9', '/usr/lib64/python3.9/lib-dynload', '/usr/local/lib/python3.9/site-packages', '/usr/lib64/python3.9/site-packages', '/usr/lib/python3.9/site-packages']
```

---

_Comment by @zanieb on 2024-03-13 14:15_

It gets added when you `pip` install something :)

---

_Comment by @zanieb on 2024-03-13 14:16_

Or, more concretely, when it exists
```
[root@f381759817d2 /]# python3 -c "import sys; print(sys.path)"
['', '/usr/lib64/python39.zip', '/usr/lib64/python3.9', '/usr/lib64/python3.9/lib-dynload', '/usr/lib64/python3.9/site-packages', '/usr/lib/python3.9/site-packages']
[root@f381759817d2 /]# mkdir -p '/usr/local/lib/python3.9/site-packages'
[root@f381759817d2 /]# python3 -c "import sys; print(sys.path)"
['', '/usr/lib64/python39.zip', '/usr/lib64/python3.9', '/usr/lib64/python3.9/lib-dynload', '/usr/local/lib/python3.9/site-packages', '/usr/lib64/python3.9/site-packages', '/usr/lib/python3.9/site-packages']
```

---

_Comment by @zanieb on 2024-03-13 15:05_

Done in #2413 instead

---

_Closed by @zanieb on 2024-03-13 15:05_

---

_Comment by @charliermarsh on 2024-03-13 15:05_

Thank you!

---
