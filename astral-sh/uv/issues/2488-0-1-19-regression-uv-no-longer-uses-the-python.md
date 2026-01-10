```yaml
number: 2488
title: "[0.1.19 regression] `uv` no longer uses the Python interpreter set by `pyenv`"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
assignees: []
created_at: 2024-03-16T15:40:31Z
updated_at: 2024-03-18T19:39:41Z
url: https://github.com/astral-sh/uv/issues/2488
synced_at: 2026-01-10T05:40:32Z
```

# [0.1.19 regression] `uv` no longer uses the Python interpreter set by `pyenv`

---

_Issue opened by @AlexWaygood on 2024-03-16 15:40_

From version 0.1.19 onwards, `uv` seems to no longer respect the Python interpreter set by using `pyenv local`, even though it did on previous versions. This still reproduces on the latest version (uv==0.1.21). Reproducer in the shell to show that this worked with `uv==0.1.18`, but not on later versions (the "Using Python 3.XX.X lines are the relevant ones that demonstrate the bug):

```zsh
% pyenv local 3.11                                                                                                                    ~/dev/typeshed
% py --version                                                                                                                        ~/dev/typeshed
Python 3.11.8
% pipx install uv==0.1.18 --force                                                                                                     ~/dev/typeshed
Installing to existing venv 'uv'
  installed package uv 0.1.18, installed using Python 3.12.2
  These apps are now globally available
    - uv
done! ✨ 🌟 ✨
% uv venv 311-env                                                                                                                     ~/dev/typeshed
Using Python 3.11.8 interpreter at: /Users/alexw/.pyenv/versions/3.11.8/bin/python3
Creating virtualenv at: 311-env
Activate with: source 311-env/bin/activate
% rm -rf 311-env                                                                                                                      ~/dev/typeshed
% pipx install uv==0.1.19 --force                                                                                                     ~/dev/typeshed
Installing to existing venv 'uv'
  installed package uv 0.1.19, installed using Python 3.12.2
  These apps are now globally available
    - uv
done! ✨ 🌟 ✨
% uv venv 311-env                                                                                                                     ~/dev/typeshed
Using Python 3.12.2 interpreter at: /usr/local/bin/python3
Creating virtualenv at: 311-env
Activate with: source 311-env/bin/activate
% rm -rf 311-env                                                                                                                      ~/dev/typeshed
% pipx upgrade uv                                                                                                                     ~/dev/typeshed
upgraded package uv from 0.1.19 to 0.1.21 (location: /Users/alexw/Library/Application Support/pipx/venvs/uv)
% uv venv 311-env                                                                                                                     ~/dev/typeshed
Using Python 3.12.2 interpreter at: /usr/local/bin/python3
Creating virtualenv at: 311-env
Activate with: source 311-env/bin/activate
```

---

_Label `bug` added by @AlexWaygood on 2024-03-16 15:40_

---

_Comment by @zanieb on 2024-03-16 15:57_

Hm I'm not sure what would have caused this from the [CHANGELOG](https://github.com/astral-sh/uv/blob/main/CHANGELOG.md#0119).

The next steps are perhaps:

- Ensure the expected behavior here is compliant with the proposed behavior in #2386 
- Bisect the commit that introduced the regression
- Add a system check test covering this (these do not support assertions a specific version is used yet)

---

_Comment by @AlexWaygood on 2024-03-16 16:05_

> * Ensure the expected behavior here is compliant with the proposed behavior in #2386

`/Users/alexw/.pyenv/shims` is on my `PATH` -- does that satisfy this point? It looks like using pyenv shims has been supported since at least https://github.com/astral-sh/uv/pull/1711, but perhaps we never officially declared support. I certainly found it very useful when `uv` did support pyenv shims :-)

I'll try bisecting this to a specific commit now.

---

_Comment by @AlexWaygood on 2024-03-16 16:12_

```
7964bfbb2bed50a5c7b0650a7b6799a66503a33a is the first bad commit
commit 7964bfbb2bed50a5c7b0650a7b6799a66503a33a
Author: konsti <konstin@mailbox.org>
Date:   Wed Mar 13 12:51:14 2024 +0100

    Move architecture and operating system probing to Python (#2381)
```

7964bfbb2bed50a5c7b0650a7b6799a66503a33a by @konstin

---

_Comment by @AlexWaygood on 2024-03-16 16:55_

Explicitly passing the Python version using the `--python` flag also seems to fail:

```
% python3.11
Python 3.11.8 (main, Feb 15 2024, 19:32:18) [Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
% uv venv --python 3.11                                                                                                                   ~/dev/mypy
  × Querying Python at `/Users/alexw/.pyenv/shims/python3.11` failed with status exit status: 127 with exit status: 127
  │ --- stdout:

  │ --- stderr:
  │ pyenv: python3.11: command not found

  │ The `python3.11' command exists in these Python versions:
  │   3.11.8

  │ Note: See 'pyenv help global' for tips on allowing both
  │       python2 and python3 to be found.
  │ ---
[1] % uv venv --python 3.11.8                                                                                                             ~/dev/mypy
  × Querying Python at `/Users/alexw/.pyenv/shims/python3.11` failed with status exit status: 127 with exit status: 127
  │ --- stdout:

  │ --- stderr:
  │ pyenv: python3.11: command not found

  │ The `python3.11' command exists in these Python versions:
  │   3.11.8

  │ Note: See 'pyenv help global' for tips on allowing both
  │       python2 and python3 to be found.
  │ ---
```

---

_Comment by @charliermarsh on 2024-03-16 22:06_

Don’t see any inherent reason that that change should break pyenv. I can take a look on Monday.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-17 21:26_

---

_Comment by @charliermarsh on 2024-03-18 02:13_

Oh my guess is that it's because we now run that in a temporary directory...? Rather than the current working directory?

---

_Comment by @zanieb on 2024-03-18 02:23_

Oh that seems like it would break the shim, interesting

---

_Comment by @charliermarsh on 2024-03-18 02:23_

I still wish we could concat all the files into one :)

---

_Comment by @zanieb on 2024-03-18 02:27_

We could put it in a zipapp... :)

---

_Comment by @charliermarsh on 2024-03-18 02:28_

I tried, it actually is meaningfully slower lol

---

_Closed by @charliermarsh on 2024-03-18 02:57_

---

_Comment by @AlexWaygood on 2024-03-18 18:37_

Confirmed that this is fixed in uv==0.1.22 🥳

---
