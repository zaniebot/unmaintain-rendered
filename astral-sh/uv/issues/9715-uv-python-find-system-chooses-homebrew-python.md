```yaml
number: 9715
title: "`uv python find --system` chooses Homebrew Python instead of system one"
type: issue
state: closed
author: abitrolly
labels:
  - question
  - uv python
assignees: []
created_at: 2024-12-08T09:01:33Z
updated_at: 2025-03-07T04:37:30Z
url: https://github.com/astral-sh/uv/issues/9715
synced_at: 2026-01-12T15:59:57Z
```

# `uv python find --system` chooses Homebrew Python instead of system one

---

_@abitrolly_

Here is the Fedora 41 setup. 

```shell
$ which uv
/home/linuxbrew/.linuxbrew/bin/uv
$ uv --version
uv 0.5.7 (Homebrew 2024-12-06)
```

```shell
$ which python
/usr/bin/python
$ which python3
/home/linuxbrew/.linuxbrew/bin/python3
```

Here are Python interpreters that `uv` sees.

```shell
$ uv python list     
cpython-3.13.1+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.13.1-linux-x86_64-gnu                 <download available>
cpython-3.13.0-linux-x86_64-gnu                 /usr/bin/python3.13
cpython-3.13.0-linux-x86_64-gnu                 /usr/bin/python3 -> python3.13
cpython-3.13.0-linux-x86_64-gnu                 /usr/bin/python -> ./python3
cpython-3.13.0-linux-x86_64-gnu                 /home/linuxbrew/.linuxbrew/opt/python@3.13/bin/python3.13
cpython-3.12.8-linux-x86_64-gnu                 <download available>
cpython-3.12.7-linux-x86_64-gnu                 /home/linuxbrew/.linuxbrew/opt/python@3.12/bin/python3.12
cpython-3.11.11-linux-x86_64-gnu                <download available>
cpython-3.10.16-linux-x86_64-gnu                <download available>
cpython-3.9.21-linux-x86_64-gnu                 <download available>
cpython-3.8.20-linux-x86_64-gnu                 <download available>
cpython-3.7.9-linux-x86_64-gnu                  <download available>
pypy-3.10.14-linux-x86_64-gnu                   <download available>
pypy-3.9.19-linux-x86_64-gnu                    <download available>
pypy-3.8.16-linux-x86_64-gnu                    <download available>
pypy-3.7.13-linux-x86_64-gnu                    <download available>
```

I expect `ux python find --system` to get `/usr/bin/python` first (which points to `/usr/bin/python3.13`, but instead it chooses Homebrew one.

```shell
$ uv python find --system -vvv
    0.000121s DEBUG uv uv 0.5.7 (Homebrew 2024-12-06)
    0.002906s DEBUG uv_python::discovery Searching for default Python interpreter in managed installations or search path
    0.002969s DEBUG uv_python::discovery Searching for managed installations at `/home/anatoli/.local/share/uv/python`
    0.008152s DEBUG uv_python::discovery Found `cpython-3.13.0-linux-x86_64-gnu` at `/home/linuxbrew/.linuxbrew/bin/python3` (search path)
/home/linuxbrew/.linuxbrew/opt/python@3.13/bin/python3.13
```

EDIT: This is important, because virtual environment with `/usr/bin/python` doesn't have problem with `tkinter` (https://github.com/astral-sh/uv/issues/7036) and Homebrew one needs extra steps.

---

_Comment by @FishAlchemist on 2024-12-08 10:14_

https://docs.astral.sh/uv/concepts/python-versions/#discovery-of-python-versions
> A Python interpreter on the PATH as python, python3, or python3.x on macOS and Linux, or python.exe on Windows.


Adjusting the order in PATH might help your desired Python version be found first.
While it's not the ideal solution, it should be able to fix your problem temporarily.

---

_Comment by @zanieb on 2024-12-08 16:04_

Please see https://docs.astral.sh/uv/concepts/python-versions/#managed-and-system-python-installations

> uv does not distinguish between Python versions installed by the operating system vs those installed and managed by other tools. For example, if a Python installation is managed with pyenv, it would still be considered a system Python version in uv.

We have some sort of unfortunate naming here that we inherited from `pip install --system`. I have some plans to re-frame the option, but uv does not have the capability to distinguish between a Python installation native to your system and one that you installed separately (at this time).

---

_Comment by @zanieb on 2024-12-08 16:05_

I'd recommend being explicit about the interpreter you need in this case, e.g., ` --python /usr/bin/python3 python`

---

_Label `question` added by @samypr100 on 2024-12-09 00:33_

---

_Label `uv python` added by @samypr100 on 2024-12-09 00:33_

---

_Comment by @abitrolly on 2024-12-09 02:05_

> uv does not have the capability to distinguish between a Python installation native to your system and one that you installed separately (at this time)

On Linux system it is just a matter of sorting, putting `/bin` and `/usr/bin` first. In this particular case it helps if lookup for `python` was made first, and if it is not found, then a search for `python3` would be done.

Even now the `uv python list` order is right, so selecting first available from it would do the trick. I tried to see if I can patch https://github.com/astral-sh/uv/blob/main/crates/uv-python/src/discovery.rs but nope, Rust is not my language.

---

_Comment by @zanieb on 2024-12-09 15:10_

> On Linux system it is just a matter of sorting, putting /bin and /usr/bin first. 

We respect the order in `PATH` — which I believe is correct.

> In this particular case it helps if lookup for python was made first, and if it is not found, then a search for python3 would be done.

Perhaps, but I think that would also be surprising? Some discussion on this in https://github.com/astral-sh/uv/issues/6444

---

_Comment by @zanieb on 2024-12-09 15:11_

> Even now the uv python list order is right, so selecting first available from it would do the trick. 

The `uv python list` order isn't sorted by discovery order, it's sorted by implementation, version, then platform ~alphabetically.

---

_Comment by @pradyunsg on 2024-12-10 13:33_

> We have some sort of unfortunate naming here that we inherited from `pip install --system`.

Wait, that's not true. This is a uv-specific option:

```
❯ pip install --help | grep -- "--system" || echo "Nothing to see here."
Nothing to see here.
❯ uv pip install --help | grep -- "--system" || echo "Nothing to see here."
      --system                               Install packages into the system Python environment [env: UV_SYSTEM_PYTHON=]
```


---

_Comment by @charliermarsh on 2024-12-10 13:35_

(I think we came up with `--system` on our own.)

---

_Comment by @zanieb on 2024-12-10 14:04_

Oh, sorry — misremembered there. Thanks for the correction @pradyunsg.

The unfortunate naming is my mistake, I guess :)

---

_Comment by @charliermarsh on 2024-12-10 14:04_

I think it might be mine :)

---

_Comment by @zanieb on 2024-12-10 14:28_

It makes sense until you pick at the definition of a system interpreter.

Anyway.. this behavior is intentional. We should track better naming for the options elsewhere, but `--system` is here to stay for compatibility purposes.

---

_Closed by @zanieb on 2024-12-10 14:28_

---

_Comment by @notatallshaw on 2024-12-10 16:12_

FYI, I *suspect* the name "system" was inspired by the actual pip flag "--break-system-packages".

---

_Comment by @abitrolly on 2024-12-10 17:31_

This root cause of this issue is actually a duplicate of https://github.com/astral-sh/uv/issues/6444

---

_Comment by @ashleyconnor on 2025-03-07 04:37_

Setting envvar `UV_PYTHON_PREFERENCE=only-managed` will make uv ignore system, homebrew and other unmanaged python installations in favour of the ones uv manages. This was the desired behaviour for my case.

Docs: https://docs.astral.sh/uv/configuration/environment/#uv_python_preference

---
