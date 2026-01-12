```yaml
number: 1795
title: Should preserve symlink to specified python interpreter
type: issue
state: closed
author: bulletmark
labels:
  - bug
  - virtualenv
assignees: []
created_at: 2024-02-21T05:03:00Z
updated_at: 2024-12-11T22:12:14Z
url: https://github.com/astral-sh/uv/issues/1795
synced_at: 2026-01-12T15:58:32Z
```

# Should preserve symlink to specified python interpreter

---

_@bulletmark_

This describes an issue where `uv venv` operates in an incompatible and less-optimal way compared to `python -m venv`.

```sh
$ uv --version
uv 0.1.6
```

I use [`pyenv`](https://github.com/pyenv/pyenv) to install multiple python versions etc. I don't use the pyenv shims and just specify the pyenv python executable when building my venv's. So I have  symlinks to the latest minor versions, i.e:
```sh
$ cd ~/.pyenv/versions
$ ls -l
lrwxrwxrwx - mark mark 13 Jan 12:47 3.7 -> 3.7.17/
drwxr-xr-x - mark mark 13 Jun  2023 3.7.17/
lrwxrwxrwx - mark mark 13 Jan 12:47 3.8 -> 3.8.18/
drwxr-xr-x - mark mark 29 Aug  2023 3.8.18/
lrwxrwxrwx - mark mark 13 Jan 12:47 3.9 -> 3.9.18/
drwxr-xr-x - mark mark 29 Aug  2023 3.9.18/
lrwxrwxrwx - mark mark 13 Jan 12:47 3.10 -> 3.10.13/
drwxr-xr-x - mark mark 29 Aug  2023 3.10.13/
lrwxrwxrwx - mark mark 18 Feb 13:04 3.11 -> 3.11.8/
drwxr-xr-x - mark mark 18 Feb 13:04 3.11.8/
lrwxrwxrwx - mark mark 21 Feb 11:32 3.12 -> 3.12.2/
drwxr-xr-x - mark mark 21 Feb 11:18 3.12.1/
drwxr-xr-x - mark mark 18 Feb 12:53 3.12.2/
```

Now witness the difference in the created symlinks to the interpreter when making a venv using these links:

```sh
$ ~/.pyenv/versions/3.12/bin/python -m venv venv
$ ls -l venv/bin/python
lrwxrwxrwx - mark mark 21 Feb 12:36 venv/bin/python -> /home/mark/.pyenv/versions/3.12/bin/python*

$ uv venv -p ~/.pyenv/versions/3.12/bin/python uvenv
Using Python 3.12.2 interpreter at /home/mark/.pyenv/versions/3.12.2/bin/python3.12
Creating virtualenv at: uvenv
Activate with: source uvenv/bin/activate
$ ls -l uvenv/bin/python
lrwxrwxrwx - mark mark 21 Feb 12:42 uvenv/bin/python -> /home/mark/.pyenv/versions/3.12.2/bin/python3.12
```

I.e. `python -m venv` creates a `3.12` link as I specified but `uv venv` dereferences that `3.12` path and creates a final link to `3.12.2`.

This is undesirable because it means when `3.12` gets it's next compatible maintenance release, e.g. `3.12.3` and the pyenv link is updated, then the `python -m venv` venv will automatically use that `3.12.3` update but the `uv venv` uvenv will continue to use the older release and requires the venv to be rebuilt against the new version.

So is there any reason `uv venv` needs to resolve that symlink?  It seems better to do what `python -m venv` does and just use it directly.

---

_Comment by @charliermarsh on 2024-02-21 05:10_

This was changed in https://github.com/astral-sh/uv/pull/966 to fix something else. Maybe @konstin knows if we actually need to resolve symlinks there, or if we just needed to convert to an absolute path or something.

---

_Comment by @konstin on 2024-02-21 11:34_

We need to resolve symlinks for the caching, but i think we can use the resolved version for caching and the original version for the venv. I think we should special case venv-from-venv cases, otherwise the new venv will break when deleting the venv it was created from.

---

_Label `bug` added by @zanieb on 2024-02-21 17:28_

---

_Label `virtualenv` added by @zanieb on 2024-02-21 17:28_

---

_Comment by @charliermarsh on 2024-03-07 21:44_

This is related to a few other issues, but note that virtualenv (on `main`) also resolves symlinks like this.

---

_Comment by @charliermarsh on 2024-03-07 21:45_

For the future: similar to https://github.com/astral-sh/uv/issues/1640. Need to decide if we want to preserve symlinks in these various cases.

---

_Comment by @gschaffner on 2024-03-10 06:55_

> ```shell
> $ ~/.pyenv/versions/3.12/bin/python -m venv venv
> $ ls -l venv/bin/python
> lrwxrwxrwx - mark mark 21 Feb 12:36 venv/bin/python -> /home/mark/.pyenv/versions/3.12/bin/python*
>
> $ uv venv -p ~/.pyenv/versions/3.12/bin/python uvenv
> Using Python 3.12.2 interpreter at /home/mark/.pyenv/versions/3.12.2/bin/python3.12
> Creating virtualenv at: uvenv
> Activate with: source uvenv/bin/activate
> $ ls -l uvenv/bin/python
> lrwxrwxrwx - mark mark 21 Feb 12:42 uvenv/bin/python -> /home/mark/.pyenv/versions/3.12.2/bin/python3.12
> ```

#2327 is related—when creating the same `uv venv` but without passing `--python`, uv's report states that `uv venv` didn't resolve `versions/3.12` to `versions/3.12.2`:

```console
+ type python
python is /home/ganden/.pyenv/shims/python
+ python -c 'import sys; print(sys.executable)'
/home/ganden/.pyenv/versions/3.12/bin/python
+ readlink -f /home/ganden/.pyenv/versions/3.12/bin/python
/home/ganden/.pyenv/versions/3.12.2/bin/python3.12
+ : '^^^ the above resolution is due to the symlinks'
+ readlink /home/ganden/.pyenv/versions/3.12 /home/ganden/.pyenv/versions/3.12.2/bin/python
3.12.2
python3.12
+ :
+ uv venv
Using Python 3.12.2 interpreter at: /home/ganden/.pyenv/versions/3.12/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
+ : '^^^ uv reports using versions/3.12'
+ readlink .venv/bin/python
/home/ganden/.pyenv/versions/3.12.2/bin/python3.12
+ : '^^^ but actually uses versions/3.12.2'
```

---

_Comment by @charliermarsh on 2024-03-11 19:44_

I'm not totally sure what we're supposed to do when creating a virtualenv from within a virtualenv.

---

_Comment by @charliermarsh on 2024-03-11 19:45_

`python -m venv` seems to use the existing virtualenv as the base:

```
❯ cat .nested-venv/pyvenv.cfg
home = /Users/crmarsh/workspace/uv/.venv/bin
include-system-site-packages = false
version = 3.8.18
```

However, `virtualenv` does not seem to do this.

---

_Comment by @CharString on 2024-05-13 11:03_

I this is how it's supposed to work, then what's the proposed workflow to upgrade, say, 3.9.18 to 3.9.19 and repair all my venvs that I created to use 3.9? freeze, rm, venv, sync?

---

_Comment by @charliermarsh on 2024-05-13 14:15_

Yeah, that's technically the correct thing to do, because you _could_ have packages in your environment that are compatible with the previous but not the updated patch version; or packages whose dependencies differ by patch version. Both of those things are allowed in Python / per the spec though are very rare in practice when talking about patch versions.

---

_Comment by @charliermarsh on 2024-05-13 14:15_

(We may change our behavior here eventually to preserve the symlink; undecided right now.)

---

_Comment by @CharString on 2024-05-13 17:55_

Hmmm, right. Then maybe the correct thing would be the addition of some `venv rebuild` command, the way `pipx` has the `reinstall` and `reinstall-all`.

---

_Comment by @ncoghlan on 2024-06-25 12:19_

As far as creating virtualenvs from within virtualenvs goes, the `python -m venv` behaviour is questionable, since it won't work in the general case (if you want to layer virtual environments together in a way that actually works, you currently need to add a `sitecustomize.py` file to the upper layers to get everything resolving correctly at runtime, simply referencing a venv layout as your "base Python" environment may not work reliably because some of the files can end up in the wrong relative locations, especially when Python has been compiled as a shared library with a wrapper executable). (I haven't gone looking to see if there's an open bug report about this though)

Outside the specific scenario of being passed a virtual environment symlink, using a provided symlink as given rather than resolving it feels like it would be more correct, though.

---

_Comment by @charliermarsh on 2024-06-25 12:22_

Interesting. Perhaps what we want then, is: resolve the symlink if it's a virtualenv, preserve it if not?

---

_Comment by @itamarst on 2024-10-15 13:35_

> I this is how it's supposed to work, then what's the proposed workflow to upgrade, say, 3.9.18 to 3.9.19 and repair all my venvs that I created to use 3.9? freeze, rm, venv, sync?

Note that this is a security concern: users will almost never want a specific patch release, they will want "the latest 3.12" or whatever so they can get security fixes when they update. This is the typical behavior with e.g. Linux distro-managed virtualenvs; I don't go and upgrade every single venv when the distro does a security updates.

You can also approach this from a user intention perspective in how the venv was originally created:

```
$ uv venv --python=3.12
$ uv venv --python=3.12.6
```

The first one fairly clearly does _not_ care about patch releases, and arguably should be updated to the latest available one. The second _does_ case about the specific patch release and should not be upgraded.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-21 20:16_

---

_Comment by @charliermarsh on 2024-10-22 00:20_

PR here: https://github.com/astral-sh/uv/pull/8433

---

_Comment by @MRigal on 2024-11-01 13:59_

Partially solved with the release of uv 0.4.29 which includes #8481 or waiting to be ungated (Preview) to close it?

---

_Comment by @charliermarsh on 2024-11-01 14:01_

It'll be in v0.5.

---

_Closed by @zanieb on 2024-11-07 20:29_

---

_Comment by @bulletmark on 2024-12-05 22:46_

I raised this issue but it was not resolved for me:
```
$ uv --version
uv 0.5.6 (b70c4f30e 2024-12-03)

$ ls -l /home/mark/.local/share/pystand/3.12
lrwxrwxrwx 1 mark mark 6 Dec  6 07:51 /home/mark/.local/share/pystand/3.12 -> 3.12.8

$ ~/.local/share/pystand/3.12/bin/python -m venv py-venv
$ ls -l py-venv/bin/python
lrwxrwxrwx    - mark mark  6 Dec 08:23 python -> /home/mark/.local/share/pystand/3.12/bin/python*

$ uv venv -p ~/.local/share/pystand/3.12/bin/python uv-venv
Using CPython 3.12.8 interpreter at: .local/share/pystand/3.12/bin/python
Creating virtual environment at: uv-venv
Activate with: source uv-venv/bin/activate
$ ls -l uv-venv/bin/python
lrwxrwxrwx    - mark mark  6 Dec 08:24 python -> /home/mark/.local/share/pystand/3.12.8/bin/python3.12*
```

---

_Comment by @charliermarsh on 2024-12-05 23:41_

Do you have the same experience with `python -m venv` on that Python?

---

_Comment by @bulletmark on 2024-12-06 00:50_

Sorry, not sure what you mean. I've made two venvs just above. Isn't the first one what you are asking for?

---

_Comment by @charliermarsh on 2024-12-06 00:53_

Sorry, I missed that! Where does this Python come from? Did you install it with uv?

---

_Comment by @bulletmark on 2024-12-06 01:02_

I installed that myself from python-build-standalone (not using `uv`). I would have thought the build of python was irrelevant but I just tried the same sequence using 3.12.7 from `pyenv` and in that case `uv` did set `uv-venv/bin/python -> /home/mark/.pyenv/versions/3.12/bin/python`. Why would that work there but not with the PBS build?

---

_Comment by @charliermarsh on 2024-12-06 01:27_

Yeah, it's about this issue: https://github.com/indygreg/python-build-standalone/issues/380. Basically, if you create an arbitrary symlink to a PBS interpreter, invoking it can crash (note that there's no `uv` here):

```
❯ ln -s /Users/crmarsh/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python pbs3.12
❯ ./pbs3.12 -m venv --without-pip pbs3.12-venv-link
❯ ./pbs3.12-venv-link/bin/python
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Python path configuration:
  PYTHONHOME = (not set)
  PYTHONPATH = (not set)
  program name = './pbs3.12-venv-link/bin/python'
  isolated = 0
  environment = 1
  user site = 1
  safe_path = 0
  import site = 1
  is in build tree = 0
  stdlib dir = '/install/lib/python3.12'
  sys._base_executable = '/Users/crmarsh/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12'
  sys.base_prefix = '/install'
  sys.base_exec_prefix = '/install'
  sys.platlibdir = 'lib'
  sys.executable = '/Users/crmarsh/workspace/uv/foo/pbs3.12-venv-link/bin/python'
  sys.prefix = '/install'
  sys.exec_prefix = '/install'
  sys.path = [
    '/install/lib/python312.zip',
    '/install/lib/python3.12',
    '/install/lib/python3.12/lib-dynload',
  ]
Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x00000001fdf34c00 (most recent call first):
  <no Python frame>
```

(What you're doing seems to work, I think because you're symlinking the entire `bin` directory or something? I'm not totally sure.)

So right now in uv we conservatively resolve symlinks for all PBS-based environments. We can probably stop doing that once the upstream issue is fixed.

---

_Comment by @bulletmark on 2024-12-06 01:32_

Thanks. Would perhaps be good if you reopened this bug as a reminder here though :)

---

_Comment by @charliermarsh on 2024-12-10 20:42_

@bulletmark -- I think your use-case _should_ be resolved by https://github.com/astral-sh/uv/pull/9723.

---

_Comment by @bulletmark on 2024-12-10 23:22_

@charliermarsh thanks, I will test that and report here when it appears in a release.

---

_Comment by @bulletmark on 2024-12-11 22:09_

I tested today's new release 0.5.7 -> 0.5.8 and 0.5.8 does fix this bug. Thanks very much for fixing this.

This fixes a significant deficit. Now when a user updates their local build of python 3.x.y -> 3.x.(y+1) for a maintenance update (e.g. using `pyenv`) then this will automatically be reflected in derived `uv` venvs like they are in standard python venvs, assuming of course they were created from a 3.x link. Before this fix, the user had to run around and manually update all his/her derived `uv` built venvs by hand because they otherwise still point to the old version.


---

_Comment by @charliermarsh on 2024-12-11 22:12_

Awesome, thanks for re-testing @bulletmark.

---
