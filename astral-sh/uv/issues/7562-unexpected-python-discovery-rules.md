```yaml
number: 7562
title: Unexpected python discovery rules
type: issue
state: open
author: bulletmark
labels:
  - question
  - uv python
assignees: []
created_at: 2024-09-19T22:03:04Z
updated_at: 2024-09-29T14:54:29Z
url: https://github.com/astral-sh/uv/issues/7562
synced_at: 2026-01-10T04:45:10Z
```

# Unexpected python discovery rules

---

_Issue opened by @bulletmark on 2024-09-19 22:03_

I wondered why my venv was using a very old Python version and then did the following test:
```
$ uv --version
uv 0.4.13

# Create a general purpose venv and I get the latest (system) version as I would expect:
$ uv venv
Using Python 3.12.6 interpreter at: /usr/bin/python3
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
$ rm -rf .venv

# Install 3.8 for some other tests I may be doing, e.g. compatibility etc.
$ uv python install 3.8
Searching for Python versions matching: Python 3.8
Installed Python 3.8.20 in 3.46s
 + cpython-3.8.20-linux-x86_64-gnu

# Create another general purpose venv (for purpose unrelated to above) but surprisingly it defaults to using
# that old version? At least I noticed it during creation this time!
$ uv venv
Using Python 3.8.20
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
```

That is surprising to users, particularly when `uv` is choosing such an old version 3.8 for default use compared to 3.12.

---

_Comment by @zanieb on 2024-09-19 22:25_

This is surprising to me as well :) will investigate

---

_Label `uv python` added by @zanieb on 2024-09-19 22:25_

---

_Assigned to @zanieb by @zanieb on 2024-09-19 22:25_

---

_Comment by @krstp on 2024-09-20 21:26_

For reference I will link to this one: https://github.com/astral-sh/uv/issues/7604
There seems some oddity in choosing and running given python verisons 🤔 

---

_Comment by @bulletmark on 2024-09-25 01:42_

@zanieb this problem is 100% reproducible for maintainers and others isn't it?

---

_Comment by @zanieb on 2024-09-25 02:01_

Yes. Actually looking closer at this though and I'm a bit confused at what surprised me initially. We prefer managed interpreters by default so once you install one it _should_ be used over system interpreters. I think this is working as-intended.

---

_Label `question` added by @zanieb on 2024-09-25 02:01_

---

_Comment by @bulletmark on 2024-09-25 02:07_

So this bug should be closed then because it is working as designed?

But surely you can see the problem here? A user installs a very old version just for compatibility testing for a specific venv etc, then from then on it becomes his default python for all new venvs!

---

_Comment by @zanieb on 2024-09-25 02:12_

Then I'd recommend changing your `python-preference` from `managed` to `system` — which can be persisted to a configuration file. I think it's reasonable for us to use Python versions installed and managed by uv over arbitrary Python versions. I agree it's a bit of a rough edge if you mix system and managed versions, but I don't see a great alternative. I'd just `uv python install 3.9 3.10 3.11 3.12`.

---

_Comment by @zanieb on 2024-09-25 02:13_

(I still think we can improve the documentation here, at least)

---

_Comment by @bulletmark on 2024-09-25 03:07_

I'd like a `python-preference` option `managed-if-higher-version` (or `system-if-higher-version`, etc). This may also be a better (i.e. least surprising) default?

---

_Comment by @latk on 2024-09-28 23:10_

I've run into a similar `uv venv` problem, just with Pyenv-managed Pythons.

The `uv venv` command works reasonably in a fresh shell, but funky things happen if `uv` is invoked *within* this venv: `uv venv` reproducibly flip-flops between two Python version, never re-creating the venv with the Python version that's already in use by the previous venv.

My initial worry was that this may stem from undefined directory iteration order, since uv just takes the first acceptable result, and doesn't seem to sort directory entries anywhere. However, that can't be it, as uv consistently flip-flops between two choices, whereas directory entry order should remain stable unless entries are added/deleted.
 
Then I had hoped that recent changes to the relevant searching code would have resolved this:

* https://github.com/astral-sh/uv/pull/5148

But alas, the observed behaviour is unchanged.

I suspect that uv is poisoning its caches with unusable results earlier in the `$PATH`, thus rejecting a valid solution. **Perhaps the cache dereferences symlinks, but the decision whether or not to include a particular executable is based on the non-dereferenced path?**

### Details on my setup

I load the project's venv into my `$PATH`, and also have a couple of Python installations via `pyenv`:

```console
$ echo $PATH
$PROJECT/.venv/bin:$HOME/.pyenv/shims:...
```

When starting the experiment, the venv's `python` is symlinked to a `python3.9` managed by pyenv:

```console
$ ls -l .venv/bin  | grep python
lrwxrwxrwx ... python -> $HOME/.pyenv/versions/3.9.19/bin/python3.9
lrwxrwxrwx ... python3 -> python
lrwxrwxrwx ... python3.9 -> python
```

My pyenv installation contains 3.8, 3.9, 3.11, and 3.12.2, but per `pyenv global` the `system` installation should have priority (which is a Python 3.12.3).

```console
$ ls ~/.pyenv/shims/python* | grep -v -
$HOME/.pyenv/shims/python
$HOME/.pyenv/shims/python3
$HOME/.pyenv/shims/python3.11
$HOME/.pyenv/shims/python3.12
$HOME/.pyenv/shims/python3.8
$HOME/.pyenv/shims/python3.9
```

I tested with two `uv` versions, `0.4.17` and the current main branch at <https://github.com/astral-sh/uv/commit/9312a08009e1f5f77687ab9d1b0a09f09e2e029d>. They produced equivalent behaviour.

```console
$ uv@2024-09-28 --version
uv 0.4.17 (9312a0800 2024-09-28)

$ uv --version
uv 0.4.17
```

When running `uv venv` in our `python3.9`-venv, it will correctly refuse using the `.venv/bin/python*` executables. But when it comes to the `~/.pyenv/shims/python*` candidates, uv immediately picks `python3.12`, thus changing the venv's Python version.

<details><summary>logs for 3.9 → 3.12 flip</summary>

```console
$ uv@2024-09-28 --verbose venv
DEBUG uv 0.4.17
DEBUG Found project root: `$PROJECT`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.8 in managed installations or system path
DEBUG Searching for managed installations at `$HOME/.local/share/uv/python`
DEBUG Found `cpython-3.9.19-linux-x86_64-gnu` at `$PROJECT/.venv/bin/python` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python`: system interpreter required
DEBUG Found `cpython-3.9.19-linux-x86_64-gnu` at `$PROJECT/.venv/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.9.19-linux-x86_64-gnu` at `$PROJECT/.venv/bin/python3.9` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3.9`: system interpreter required
DEBUG Found `cpython-3.9.19-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python`: system interpreter required
DEBUG Found `cpython-3.9.19-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python3` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python3.12` (search path)
Using CPython 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtual environment at: .venv
DEBUG Removing existing directory
Activate with: source .venv/bin/activate
```

</details>

If we run `uv venv` again, the venv's `python3.12` is rejected, whereas the pyenv's `3.9` is picked again:

<details><summary>logs for 3.12 → 3.9 flop</summary>

```console
$ uv@2024-09-28 --verbose venv
DEBUG uv 0.4.17
DEBUG Found project root: `$PROJECT`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.8 in managed installations or system path
DEBUG Searching for managed installations at `$HOME/.local/share/uv/python`
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$PROJECT/.venv/bin/python` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python`: system interpreter required
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$PROJECT/.venv/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$PROJECT/.venv/bin/python3.12` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3.12`: system interpreter required
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python`: system interpreter required
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python3` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3`: system interpreter required
DEBUG Found `cpython-3.12.3-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python3.12` (search path)
DEBUG Ignoring Python interpreter at `$PROJECT/.venv/bin/python3.12`: system interpreter required
DEBUG Found `cpython-3.9.19-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python3.9` (search path)
Using CPython 3.9.19 interpreter at: $HOME/.pyenv/versions/3.9.19/bin/python3.9
Creating virtual environment at: .venv
DEBUG Removing existing directory
Activate with: source .venv/bin/activate
```

</details>

Of course things work perfectly if I force a particular Python version. It seems that `uv venv --python 3.12` consistently resolves to my system Python, regardless of the initial state of the `.venv` directory.

If I exit the venv, then `uv venv` also consistently picks one Python version, namely the 3.12 from pyenv.

<details><summary>logs when re-creating the venv from the outside</summary>

```console
$ uv@2024-09-28 --verbose venv
DEBUG uv 0.4.17
DEBUG Found project root: `$PROJECT`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.8 in managed installations or system path
DEBUG Searching for managed installations at `$HOME/.local/share/uv/python`
DEBUG Found `cpython-3.12.2-linux-x86_64-gnu` at `$HOME/.pyenv/shims/python` (search path)
Using CPython 3.12.2 interpreter at: $HOME/.pyenv/versions/3.12.2/bin/python
Creating virtual environment at: .venv
DEBUG Removing existing directory
Activate with: source .venv/bin/activate
```

</details>

---

_Comment by @krstp on 2024-09-29 01:18_

You described it really well. This is mostly what I had experience in https://github.com/astral-sh/uv/issues/7604. One detail that played role in my case I had another `uv init` one level above, hence it was picking up the env from level up, needless to say, the whole `uv` did show some oddities that match to what you described above. Thanks for documenting this, which also makes me verify my sanity :) 

PS. Also working here on the `pyenv`

---

_Comment by @zanieb on 2024-09-29 14:54_

Thanks for all the details. I'll need to dig into this more during the week, but pyenv is generally quite problematic for our caching since their shim can return any interpreter.

---
