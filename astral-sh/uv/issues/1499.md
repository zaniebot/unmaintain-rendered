```yaml
number: 1499
title: "`.` only works with `--editable`"
type: issue
state: closed
author: altendky
labels:
  - duplicate
assignees: []
created_at: 2024-02-16T15:11:13Z
updated_at: 2024-03-22T21:24:50Z
url: https://github.com/astral-sh/uv/issues/1499
synced_at: 2026-01-10T05:40:31Z
```

# `.` only works with `--editable`

---

_Issue opened by @altendky on 2024-02-16 15:11_

I am in an empty directory so the failure to install due to lack of `pyproject.toml` is expected.  What is not expected is the failure to parse `.` at all without `--editable` having been specified.

```console
$ python3.11 -m venv venv
```

```console
$ venv/bin/python -m pip install 'uv @ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6'
Collecting uv@ git+https://github.com/astral-sh/uv@1ed6ba0ba0d3756312085ec21c90469985b5bbb6
  Using cached uv-0.1.2-py3-none-linux_x86_64.whl
Installing collected packages: uv
Successfully installed uv-0.1.2

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: /home/altendky/tmp/venv/bin/python -m pip install --upgrade pip
```

```console
$ source venv/bin/activate
```

```console
(venv) $ ls -la
total 52
drwxrwxr-x 12 altendky altendky 4096 Feb 16 10:09  .
drwxr-x--- 51 altendky altendky 4096 Feb 16 09:55  ..
drwxrwxr-x  4 altendky altendky 4096 Dec  5 08:29  blue
drwxrwxr-x  4 altendky altendky 4096 Dec 12 09:17  c
drwxrwxr-x 11 altendky altendky 4096 Nov 29 07:31  chia-blockchain
drwxrwxr-x  3 altendky altendky 4096 Dec  1 12:12 'chia-installers-linux-rpm-intel (3)'
drwxrwxr-x  2 altendky altendky 4096 Dec  4 10:01  hwd
drwxrwxr-x  5 altendky altendky 4096 Nov 30 15:29  it
drwxrwxr-x  2 altendky altendky 4096 Dec  4 16:06  oc
drwxrwxr-x  3 altendky altendky 4096 Dec 13 13:46  p
drwxrwxr-x  3 altendky altendky 4096 Nov 18 14:00  .pytest_cache
drwxrwxr-x  5 altendky altendky 4096 Feb 16 10:09  venv
-rw-rw-r--  1 altendky altendky  793 Nov 18 14:27  x.py
```

```console
(venv) $ python -m uv pip install .
error: Failed to parse `.`
  Caused by: Expected package name starting with an alphanumeric character, found '.'
.
^
```

```console
(venv) $ python -m uv pip install --editable .
error: Failed to build editables
  Caused by: Failed to build editable: file:///home/altendky/tmp
  Caused by: Invalid source distribution: The archive contains neither a `pyproject.toml` nor a `setup.py` file at the top level
```

```console
(venv) $ python -m pip install .
ERROR: Directory '.' is not installable. Neither 'setup.py' nor 'pyproject.toml' found.

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: pip install --upgrade pip
```

---

_Comment by @zanieb on 2024-02-16 15:14_

Thanks for the thorough report!

Please see #313 and https://github.com/astral-sh/uv/pull/1424

---

_Label `duplicate` added by @zanieb on 2024-02-16 15:14_

---

_Comment by @altendky on 2024-02-16 15:17_

I wasn't sure what to search for and so I missed it.  Thanks for the help with that.  I'll try being more explicit with the ~`file @`~ `package-name @` form.  Thanks!

---

_Closed by @altendky on 2024-02-16 15:17_

---

_Comment by @charliermarsh on 2024-02-16 15:20_

FWIW, the `.` should work ok, it's just that we require a package name, so you can either do:

```
uv pip install "foo @ ."
```

Or:

```
uv pip install -e .
```

---

_Comment by @altendky on 2024-02-16 15:26_

Thanks for the specifics.  That's exactly what I was going to try, though in my case I'm also specifying extras.  First pass didn't work, but I'll dig a bit and see if I still need to follow up.

---

_Comment by @charliermarsh on 2024-02-16 15:26_

Please do, happy to help.

---

_Comment by @henryiii on 2024-02-16 22:43_

I was testing this out as a replacement for `nox`, and this will break a lot of nox scripts which assume `pip install .` works, it's a very common idiom.

It is insanely fast though as a nox backend. :)

---

_Comment by @altendky on 2024-02-16 23:47_

I believe the idea is that the case reported here would be addressed by https://github.com/astral-sh/uv/issues/313.  At least that's why I was comfortable closing this issue.

---

_Comment by @charliermarsh on 2024-03-22 21:24_

This is supported as of `v0.1.24`. You can `uv pip install .` directly, without including a package name.

---
