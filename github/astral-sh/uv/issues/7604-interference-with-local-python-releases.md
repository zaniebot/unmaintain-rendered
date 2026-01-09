---
number: 7604
title: Interference with local python releases
type: issue
state: closed
author: krstp
labels:
  - question
assignees: []
created_at: 2024-09-20T21:10:00Z
updated_at: 2025-11-07T07:24:42Z
url: https://github.com/astral-sh/uv/issues/7604
synced_at: 2026-01-07T13:12:17-06:00
---

# Interference with local python releases

---

_Issue opened by @krstp on 2024-09-20 21:10_

First, thank you for a great tool which is `uv`. I am really enjoying it.

However, I am running into issue. Conflicts with existing python installations. It turns out:
- I have system wide python `3.12.6` version, probably installed with `brew`
- I have other python releases (multiple versions) installed with `pyenv`

Now a gist:
- I do have `3.12.4` installed with `uv`, no problem here; everything works as desired.
- when specifying in `.python-version` version `3.12.6` regardless of what command I invoke with `uv` I get the following error:
```
pyenv: version `3.12.6' is not installed (set by /Users/<user>/<dev_path>/sample_git_project/.python-version)
```

I was trying to go through `~/.config/uv` and `~/.local/` but I am not finding any `json` or other conf file that would allow me to re/point to the right python release `~/.local/share/uv/python/cpython-3.x...` 

Any tips would be appreciated.

---

_Referenced in [astral-sh/uv#7562](../../astral-sh/uv/issues/7562.md) on 2024-09-20 21:26_

---

_Comment by @zanieb on 2024-09-20 21:27_

Hm that looks like a pyenv error message not a uv error message. Can you share more of the error with the `-v` flag? If there's not more I presume this is some sort of pyenv shell hook?

---

_Label `question` added by @zanieb on 2024-09-20 21:28_

---

_Comment by @krstp on 2024-09-20 21:34_

This is also not clear to me. After running `uv venv` the `.venv` is created and looking inside it I see `uv` creates `pyvenv.cfg`, but despite even changing it to the install of my liking say `3.12.6` path instead of `3.12.4` it will still revert to `.4` 🤔 

And indeed, weirdly `uv` picks up `pyenv` version not the `uv` installed version and I am having trouble finding where could this be defined in `uv` 🤔 

`uv -v venv` does not yield any additional input.

The solution here would be to force `uv` to use its own install.... and sorry, but I will not be uninstalling `pyenv`, this is what I used to rely on and still large part of my solution run on it, before I battle-test the `uv` ;)

---

_Comment by @krstp on 2024-09-20 21:37_

Rather my main question is... where does the `uv` config live, where does it pick up given python version?

When I type `uv python list` it will show me what version are installed where, but even if `3.12.6` is the one from `uv` it will still somehow pick up previously installed version with `brew`; BTW: `pyenv` does not have `3.12.6` in its repository (or I haven't updated mine), so this is also weird that it reference `pyenv` 🤔 

Ref from `uv python list`:
```
cpython-3.13.0rc2-macos-aarch64-none    <download available>
cpython-3.12.6-macos-aarch64-none       /Users/<user>/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3 -> python3.12
[...]
```

---

_Comment by @krstp on 2024-09-20 21:46_

Some more insight (notice instantiation of 3.12.6 but cfg set with 3.12.4):
```
~/<path>/sample_git_project> uv python --python-preference managed install 3.12.6
Searching for Python versions matching: Python 3.12.6
Found existing installation for Python 3.12.6: cpython-3.12.6-macos-aarch64-none

~/<path>/sample_git_project > uv venv 3.12.6 
Using Python 3.12.4
Creating virtual environment at: 3.12.6
Activate with: source 3.12.6/bin/activate

~/<path>/sample_git_project> cat 3.12.6/pyvenv.cfg                                                                                                                                                                                                                                                                                                                                                                                                                                                               
       │ File: 3.12.6/pyvenv.cfg
   1   │ home = /Users/<user>/.local/share/uv/python/cpython-3.12.4-macos-aarch64-none/bin
   2   │ implementation = CPython
   3   │ uv = 0.4.13
   4   │ version_info = 3.12.4
   5   │ include-system-site-packages = false

~/<path>/sample_git_project> source 3.12.6/bin/activate

~/<path>/sample_git_project> python -version
Python 3.12.4
```
😩 
But...
```
~/<path>/sample_git_project> ~/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12 --version

Python 3.12.6
```

So all there seems is to change some underlying cfg pointer, but changing the `.venv/pyvenv.cfg` value does not work...  as soon as I run `uv venv` it somehow gets overwritten with `3.12.4` version :/

---

_Comment by @zanieb on 2024-09-20 21:55_

Sounds like you want to create a `uv.toml` for your user (https://docs.astral.sh/uv/configuration/files/)

Then change the `python-preference` to `only-managed` (https://docs.astral.sh/uv/reference/settings/#python-preference)

---

_Comment by @zanieb on 2024-09-20 21:55_

`uv venv 3.12.6` creates a directory called `3.12.6`. You want `uv venv --python 3.12.6` 

---

_Comment by @krstp on 2024-09-20 22:08_

So...
- created `~/.config/uv/uv.toml` so it will be used globally, the content of the file is:
```
python-preference = "managed"
```
- I also copied the same file into project directory

Then I run `python --version` as well as `uv venv --python 3.12.6` and in each case I get:
```
pyenv: version `3.12.6' is not installed (set by /Users/<path>/sample_git_project/.python-version)
```


---

_Comment by @krstp on 2024-09-20 22:24_

Update:

I have manually disabled `pyenv` from the whole env: `PATH=$(echo $PATH | tr ':' '\n' | grep -v pyenv | tr '\n' ':' | sed 's/:$//')`
then `uv venv --verbose`
This way I get feedback on 3.12.6 avaibility:
```
Using Python 3.12.6
Creating virtual environment at: .venv
DEBUG Removing existing directory
Activate with: source .venv/bin/activate
```
but `source .venv/bin/activate` doesn't do a thing, such as:
```
> python --version                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
zsh: command not found: python
```
on the other hand:
```
> .venv/bin/python --version                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
Python 3.12.6
```
checking `python` installation path more:
```
> which python
python not found

> which python3
/opt/homebrew/bin/python3  (btw: this is brew install with 3.12.5)
```
but
```
> uv python find                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
/Users/<path>/sample_git_project/.venv/bin/python3
```
So in summary, now `.venv` carries right python version, but global aliases to `python` and `python3` will not recognize it 🤔 

---

_Comment by @krstp on 2024-09-20 22:41_

OK, never mind. I opened a fresh terminal and reloaded `.zshrc` ... whatever I have setup up upon `deactivation` it still seems to stay into somehow initialized under the hood pyenv (this is my global setup), but at least now everything works. I will close it for how. If this comes back I will reopen.

---

_Closed by @krstp on 2024-09-20 22:41_

---

_Comment by @krstp on 2024-09-29 01:17_

One detail should be added here for reference, the folder I was tested it in was nested in the other folder that already had `uv init`; this means it was double nested `.venv` without activation and I believe this was party the reason why it was referencing external (one level up) `.venv`.

I hope this helps to pin point the exact `uv` behavior.

Otherwise https://github.com/astral-sh/uv/issues/7562 catches the behavior quite well.

---

_Referenced in [NanmiCoder/MediaCrawler#673](../../NanmiCoder/MediaCrawler/issues/673.md) on 2025-07-24 03:52_

---

_Comment by @SwatGetmann on 2025-11-07 07:24_

I don't know if that helps, but I had the same issue on Ubuntu, right after `uv init`. What helped me is removing `.python-version` file (with a single line `3.13` in it) and then run `uv sync` - it worked and initialized venv with CPython 3.13.3 (which is the latest I have downloaded).

---
