---
number: 1341
title: "\"× Neither `python` nor `python3` are in `PATH`. Is Python installed?\" even though Python is installed and in PATH"
type: issue
state: closed
author: gorkaerana
labels: []
assignees: []
created_at: 2024-02-15T21:03:15Z
updated_at: 2024-02-15T22:29:50Z
url: https://github.com/astral-sh/uv/issues/1341
synced_at: 2026-01-07T13:12:16-06:00
---

# "× Neither `python` nor `python3` are in `PATH`. Is Python installed?" even though Python is installed and in PATH

---

_Issue opened by @gorkaerana on 2024-02-15 21:03_

Hi,

First and foremost, thank you very much for your work in `ruff` and for what a priori seems to be another mindbogglingly good Python tool! Keep it up 💪️

Following the `curl` installation method (`curl -LsSf https://astral.sh/uv/install.sh | sh`) indicated in the [release blog](https://astral.sh/blog/uv) results in the error specified in the title, even though Python is installed and available in `PATH`. Please find below a screenshot of the terminal and the terminal output itself for easier copy-pasting. I'm on a Ubuntu 22.04.3 LTS machine.

![image](https://github.com/astral-sh/uv/assets/23142248/d59499d0-1a1c-45a3-afa7-eea085cf0d31)

```
lenovo-home -> /home/gorka @ 
$ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.1.0 x86_64-unknown-linux-gnu
installing to /home/gorka/.cargo/bin
  uv
everything's installed!

To add $HOME/.cargo/bin to your PATH, either restart your shell or run:

    source $HOME/.cargo/env
lenovo-home -> /home/gorka @ 
$ source $HOME/.cargo/env
lenovo-home -> /home/gorka @ 
$ uv venv
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?
lenovo-home -> /home/gorka @ 
$ echo $PATH
/home/gorka/.cargo/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/snap/bin
lenovo-home -> /home/gorka @ 
$ which python3
/usr/bin/python3
```

Thank you in advance,
Gorka.

---

_Comment by @zanieb on 2024-02-15 21:03_

Thank you! Can you share the output of `uv venv -v`?

---

_Comment by @gorkaerana on 2024-02-15 21:04_

```
 uv_interpreter::python_query::find_default_python platform=Platform { os: Manylinux { major: 2, minor: 35 }, arch: X86_64 }, cache=Cache { root: "/home/gorka/.cache/uv", refresh: None, _temp_dir_drop: None }
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?
```

---

_Comment by @gorkaerana on 2024-02-15 21:13_

Forgot to mention that `pip install uv` worked, thought it might be useful.

---

_Comment by @zanieb on 2024-02-15 21:14_

Oh thanks that's good to know!

---

_Referenced in [astral-sh/uv#1346](../../astral-sh/uv/issues/1346.md) on 2024-02-15 21:14_

---

_Comment by @zanieb on 2024-02-15 21:19_

I believe this is because `python3` is on your path but not `python` and we have a bug #1351 — if you create a `python` alias for `python3` does it work?

---

_Comment by @gorkaerana on 2024-02-15 21:26_

Adding `alias python=python3` to my `.bashrc` changes nothing, see terminal output pasted below. Let me remove `uv` and reinstall it to confirm with the `python` alias.

```
lenovo-home -> /home/gorka @ 
$ which python
lenovo-home -> /home/gorka @ 
$ tail -1 .bashrc
alias python=python3
lenovo-home -> /home/gorka @ 
$ python
Python 3.10.12 (main, Nov 20 2023, 15:14:05) [GCC 11.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> exit()
lenovo-home -> /home/gorka @ 
$ uv venv
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?
lenovo-home -> /home/gorka @ 
$ uv venv -v
 uv_interpreter::python_query::find_default_python platform=Platform { os: Manylinux { major: 2, minor: 35 }, arch: X86_64 }, cache=Cache { root: "/home/gorka/.cache/uv", refresh: None, _temp_dir_drop: None }
  × Neither `python` nor `python3` are in `PATH`. Is Python installed?```

---

_Comment by @gorkaerana on 2024-02-15 21:27_

Fyi, this is also an issue when installing `uv` via `curl` in a WSL running Ubuntu 20.04.5 TLS.

---

_Comment by @zanieb on 2024-02-15 21:30_

Closed by https://github.com/astral-sh/uv/pull/1351

I think alias is at the shell level, you'd need to have a link from python3 -> python in your path.

---

_Comment by @gorkaerana on 2024-02-15 21:30_

Removing and re-installing `uv` with `alias python=python3` on my `.bashrc` does not fix the issue.

---

_Comment by @gorkaerana on 2024-02-15 21:42_

Solved! Thank you for the swift help 😊️

```
lenovo-home -> /home/gorka @ 
$ which python
/usr/bin/python
lenovo-home -> /home/gorka @ 
$ ll /usr/bin/python
lrwxrwxrwx 1 root root 16 Feb 15 22:38 /usr/bin/python -> /usr/bin/python3*
lenovo-home -> /home/gorka @ 
$ uv venv
Using Python 3.10.12 interpreter at /usr/bin/python3.10
Creating virtualenv at: .venv
```

---

_Comment by @MichaReiser on 2024-02-15 21:43_

This should be fixed by https://github.com/astral-sh/uv/pull/1351 A new release is going out right now...

---

_Closed by @MichaReiser on 2024-02-15 21:43_

---

_Comment by @gorkaerana on 2024-02-15 21:44_

@MichaReiser when will the release drop? I'll gladly test this very issue from scratch again.

---

_Comment by @MichaReiser on 2024-02-15 21:46_

@gorkaerana in the next 10-20minutes? It's building https://github.com/astral-sh/uv/actions/runs/7922621154

---

_Comment by @gorkaerana on 2024-02-15 22:28_

Can confirm it works now. Thank you for the help! :)

---

_Comment by @MichaReiser on 2024-02-15 22:29_

Thanks for the feedback and have fun with uv

---
