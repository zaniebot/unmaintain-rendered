```yaml
number: 5210
title: Non-default system-installed Python versions not listed
type: issue
state: closed
author: mgaitan
labels:
  - bug
assignees: []
created_at: 2024-07-19T02:36:58Z
updated_at: 2024-07-22T20:26:38Z
url: https://github.com/astral-sh/uv/issues/5210
synced_at: 2026-01-10T04:53:49Z
```

# Non-default system-installed Python versions not listed

---

_Issue opened by @mgaitan on 2024-07-19 02:36_

I have 3 different Python versions installed on my system (Ubuntu 24.04). The default is 3.12, and I have 3.8 and 3.10 installed via PPAs.

```
tin@morocha:~/lab$ ls /usr/bin/python3.*
/usr/bin/python3.10  /usr/bin/python3.12  /usr/bin/python3.12-config  /usr/bin/python3.8  /usr/bin/python3.8-config
```

I expect `uv python list` to show all these versions as available. However, only the default (3.12.3) is shown, and 3.10.14 and 3.8.19 are listed as "download available" even though they are already installed (not uv managed).

```
tin@morocha:~/lab$ uv python list
warning: `uv python list` is experimental and may change without warning.
cpython-3.12.4-linux-x86_64-gnu     <download available>
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python
cpython-3.12.3-linux-x86_64-gnu     /bin/python3
cpython-3.12.3-linux-x86_64-gnu     /bin/python
cpython-3.11.9-linux-x86_64-gnu     <download available>
cpython-3.10.14-linux-x86_64-gnu    <download available>
cpython-3.9.19-linux-x86_64-gnu     <download available>
cpython-3.8.19-linux-x86_64-gnu     <download available>
cpython-3.7.9-linux-x86_64-gnu      <download available>
```

```
tin@morocha:~/lab$ python3.10 --version
Python 3.10.14
tin@morocha:~/lab$ python3.8 --version
Python 3.8.19
```

uv version 

```
tin@morocha:~/lab$ uv --version
uv 0.2.26
```

---

_Comment by @zanieb on 2024-07-19 02:54_

Thanks for the report! I think this is solved by #5148 which will be in the next release.

---

_Label `bug` added by @charliermarsh on 2024-07-19 12:52_

---

_Comment by @mgaitan on 2024-07-22 20:23_

it's fixed !
 
```
tin@morocha:~/lab$ uv --version
uv 0.2.27
tin@morocha:~/lab$ uv python list
warning: `uv python list` is experimental and may change without warning
cpython-3.12.4-linux-x86_64-gnu     <download available>
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3.12
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu     /usr/bin/python
cpython-3.12.3-linux-x86_64-gnu     /bin/python3.12
cpython-3.12.3-linux-x86_64-gnu     /bin/python3
cpython-3.12.3-linux-x86_64-gnu     /bin/python
cpython-3.11.9-linux-x86_64-gnu     <download available>
cpython-3.10.14-linux-x86_64-gnu    /usr/bin/python3.10
cpython-3.10.14-linux-x86_64-gnu    /bin/python3.10
cpython-3.10.14-linux-x86_64-gnu    <download available>
cpython-3.9.19-linux-x86_64-gnu     <download available>
cpython-3.8.19-linux-x86_64-gnu     /usr/bin/python3.8
cpython-3.8.19-linux-x86_64-gnu     /bin/python3.8
cpython-3.8.19-linux-x86_64-gnu     <download available>
cpython-3.7.9-linux-x86_64-gnu      <download available>
```

QQ: Does it have sense to list  symbolic links? 

For instance, several of the above items points to the same executable 

```
tin@morocha:~/lab$ file /usr/bin/python3.12
/usr/bin/python3.12: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=d575bfe93b720eb1f8398b1c56ba864b3d9089bd, for GNU/Linux 3.2.0, stripped
tin@morocha:~$ file /usr/bin/python3
/usr/bin/python3: symbolic link to python3.12
```

PS: I'm eager to be able to install cpython-3.13 beta via uv! 

---

_Closed by @mgaitan on 2024-07-22 20:23_

---

_Comment by @zanieb on 2024-07-22 20:26_

Opened https://github.com/astral-sh/uv/issues/5308 for that.

Python 3.13 is WIP at https://github.com/indygreg/python-build-standalone/pull/264

---
