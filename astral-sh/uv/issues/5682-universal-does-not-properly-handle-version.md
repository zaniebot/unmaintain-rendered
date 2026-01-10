---
number: 5682
title: "--universal does not properly handle version differences on different platforms "
type: issue
state: closed
author: gokay05
labels: []
assignees: []
created_at: 2024-08-01T04:43:53Z
updated_at: 2024-08-01T18:57:47Z
url: https://github.com/astral-sh/uv/issues/5682
synced_at: 2026-01-10T01:23:51Z
---

# --universal does not properly handle version differences on different platforms 

---

_Issue opened by @gokay05 on 2024-08-01 04:43_

I am using uv 0.2.32 to compile a requirements file, which I use to sync other devices. Some of the other devices are on other platforms, so I am using the --universal flag. However, it doesnt seem like the universal flag checks versions so even though I can compile without issues, I cannot install on the other end.

As a reproduction case, create a new folder and install onnxruntime on a Windows machine with:

```
uv init
uv add onnxruntime
```

This currently installs onnxruntime version 1.18.1. Then create a requirements files with:

```
uv pip compile --universal --output-file requirements.txt pyproject.toml
```

The file contains 1.18.1 as the version number.  Now try to install this package on a Linux machine:

```
uv pip install  --requirement requirements.txt test
```

However, this results in the following error:

No solution found when resolving dependencies:
   Because onnxruntime==1.18.1 has no wheels with a matching Python implementation tag and you require
      onnxruntime==1.18.1, we can conclude that the requirements are unsatisfiable.

Not sure what the right solution ought to be here, but the handling should be on the compile side instead of install/sync side. It seems like the last Linux wheel for the package is 1.16.3, so it could compile to that or error out and say it cannot be compiled with the universal flag. 

---

_Comment by @konstin on 2024-08-01 07:18_

What exact OS and python version are you on, what does `ldd --version` say?

---

_Comment by @paveldikov on 2024-08-01 10:51_

Does `--only-binary` make a difference in the resolution?

---

_Comment by @notatallshaw on 2024-08-01 14:36_

> uv pip install  --requirement requirements.txt test

Is the "test" here a copy and paste error? Because that will definetly cause errors.

`onnxruntime==1.18.1` is definetly available on Linux, I think you will need to provide more of the outputs such as the exact Python, the `requirements.txt`, and the verbose output of the `uv pip install ` install command:

```
$ uv venv
Using Python 3.11.8 interpreter at: .pyenv/versions/3.11.8/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

$ uv pip install "onnxruntime==1.18.1"
Resolved 9 packages in 11ms
Prepared 9 packages in 1.72s
Installed 9 packages in 470ms
 + coloredlogs==15.0.1
 + flatbuffers==24.3.25
 + humanfriendly==10.0
 + mpmath==1.3.0
 + numpy==1.26.4
 + onnxruntime==1.18.1
 + packaging==24.1
 + protobuf==5.27.3
 + sympy==1.13.1
```

---

_Comment by @gokay05 on 2024-08-01 18:24_

As [notatallshaw](https://github.com/notatallshaw) mentioned, onnxruntime is available on Linux at 1.18.1. I am on an older version of Linux and Python, so it seems I can only handle up to a "cp311-cp311-manylinux_2_26_x86_64" wheel since my version of libc is 2.26. With 1.17.0, onnxruntime switched to using libc 2.27 which is why I could only install up tp 1.16.3. 

So the resolution is correct for a general Linux machine, just not the one I am using. Since there seems to be nothing wrong with uv, I am closing the issue. Thanks for your help and keep up the good work!!

---

_Closed by @gokay05 on 2024-08-01 18:24_

---

_Comment by @notatallshaw on 2024-08-01 18:51_

One thing I do for my resolution is provide a constraint file that contains bounds that only I can know for my environment, e.g. a constraints.txt:

```
onnxruntime < 1.17.0 # Required for our Linux env which uses libc 2.26
```

And then:

```
uv pip compile --constraint constraints.txt ...
```

I prefer this over directly adding to a requirements file as seperates my hard requirements from my known constraints (which may be on transativie dependencies as well as actual dependencies).

---

_Comment by @gokay05 on 2024-08-01 18:57_

Very useful!! Thanks for the tip [notatallshaw](https://github.com/notatallshaw).

---
