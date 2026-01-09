---
number: 7325
title: Upgrade Python version installed by uv
type: issue
state: closed
author: stuartmaxwell
labels:
  - enhancement
  - uv python
assignees: []
created_at: 2024-09-12T08:36:07Z
updated_at: 2025-06-20T14:17:14Z
url: https://github.com/astral-sh/uv/issues/7325
synced_at: 2026-01-07T13:12:17-06:00
---

# Upgrade Python version installed by uv

---

_Issue opened by @stuartmaxwell on 2024-09-12 08:36_

My apologies if this has been asked and answered before, but I have searched these issues as well as the docs.

I have Python 3.12.5 installed from the following command: `uv python install 3.12`. Now that 3.12.6 is released, how do I upgrade to this version? I thought that perhaps re-running `uv python install 3.12` might install the newest minor version, but that didn't work. What is the correct workflow to upgrade to the newest version? Should I just uninstall `3.12` and then reinstall `3.12`? This doesn't appear to be addressed in the docs.

---

_Comment by @jamescooke on 2024-09-12 14:33_

Docs here https://docs.astral.sh/uv/reference/cli/#uv-python-install currently state (in the description of `--reinstall`):

> By default, uv will exit successfully if the version is already installed.

So I think you want to add `--reinstall` to get what you want - that new new latest version 👌🏻 :

```sh
uv python install --resinstall 3.12
```

---

_Assigned to @zanieb by @zanieb on 2024-09-12 15:16_

---

_Comment by @stuartmaxwell on 2024-09-12 22:09_

Thanks @jamescooke - that kinda achieves what I want, but I don't think it's great. The `--reinstall` option appears to always download and reinstall the latest version, even if the latest version is already installed. This makes sense because I'm telling `uv` to specifically reinstall it. Perhaps what is missing is an `--upgrade` option that will check if a new version is available first, and then only reinstall if needed - similar to the pip resolver.



---

_Label `enhancement` added by @zanieb on 2024-09-13 02:09_

---

_Label `uv python` added by @zanieb on 2024-09-13 02:09_

---

_Comment by @Odud on 2024-10-08 09:00_

I've got a similar issue (I think). Outside of uv and the venv I have 3.12 and 3.13 available .

``` update-alternatives --display python
python - auto mode
  link best version is /usr/local/bin/python3.13
  link currently points to /usr/local/bin/python3.13
  link python is /usr/bin/python
/usr/local/bin/python3.12 - priority 1
/usr/local/bin/python3.13 - priority 2
```

If I create a new project then uv will install 3.13

```uv run python
Using Python 3.13.0 interpreter at: /usr/local/bin/python3.13
Creating virtualenv at: .venv
Built pytest @ file:///home/test/pytest
Installed 1 package in 0.55ms
Python 3.13.0 (main, Oct 8 2024, 09:19:25) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
```

But in an existing project I can't seem to update to 3.13

``` uv python install --reinstall 3.13
Searching for Python versions matching: Python 3.13
error: No download found for request: cpython-3.13-linux-aarch64-gnu
```

Although uv seems to know that the local version is available

```uv python list
cpython-3.13.0-linux-aarch64-gnu     /usr/local/bin/python3.13
cpython-3.13.0-linux-aarch64-gnu     /usr/bin/python -> /etc/alternatives/python
cpython-3.13.0-linux-aarch64-gnu     /bin/python -> /etc/alternatives/python
cpython-3.12.5-linux-aarch64-gnu     /usr/local/bin/python3.12
cpython-3.12.5-linux-aarch64-gnu     <download available>
cpython-3.11.9-linux-aarch64-gnu     <download available>
```

---

_Comment by @jamescooke on 2024-10-08 10:32_

Hi @Odud - I think your problem warrants its own Issue.

I would pause now and create a new Issue with a description of the problem you're experiencing with not being able to upgrade to Python 3.13.

When you create that new issue by the way - sharing the output you received like from "uv seems to know that the local version is available" is helpful - however, it would be even more helpful to show the invocation / process to get that output. For example, are you running `uv python list` or similar? And what were the invocations before that? Is the virtualenv active? This will help people reading the issue to recreate it 🙏🏻 

---

_Comment by @Odud on 2024-10-08 11:15_

Thanks James, I will create a new issue later today - done. Previous comment hidden. Please see new issue #8008

---

_Referenced in [astral-sh/uv#8008](../../astral-sh/uv/issues/8008.md) on 2024-10-08 16:40_

---

_Comment by @angelo-peronio on 2025-02-10 11:09_

Possibly a duplicate of #7892

---

_Referenced in [topgrade-rs/topgrade#1122](../../topgrade-rs/topgrade/pulls/1122.md) on 2025-04-13 08:00_

---

_Comment by @gatto on 2025-04-16 13:59_

I found this issue -
Can we update the python version with `uv` without removing and remaking the entire venv?

- edit - on macOS / Linux.

---

_Referenced in [astral-sh/uv#13954](../../astral-sh/uv/pulls/13954.md) on 2025-06-10 20:58_

---

_Closed by @jtfmumm on 2025-06-20 14:17_

---
