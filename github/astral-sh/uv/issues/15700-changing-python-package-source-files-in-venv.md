---
number: 15700
title: Changing python package source files in venv changes cached uv files too
type: issue
state: closed
author: S-Erik
labels:
  - question
assignees: []
created_at: 2025-09-05T10:43:01Z
updated_at: 2025-09-06T02:39:32Z
url: https://github.com/astral-sh/uv/issues/15700
synced_at: 2026-01-07T13:12:19-06:00
---

# Changing python package source files in venv changes cached uv files too

---

_Issue opened by @S-Erik on 2025-09-05 10:43_

### Summary

### Commands I ran:
Clear cache to start from a clean slate:
```bash
uv cache clean
```

Create new python virtual environment (the used python version should not be important here, tested with 3.11 and 3.12):
```bash
python3.12 -m venv .venv && source .venv/bin/activate
```

Use uv to pip install a package. Here we use my-mini-package since it has no dependencies and just one function.
```bash
uv pip install my-mini-package==0.1.0
```

Find and go to cache directory of uv (location printed when we executed `uv cache clean`). On linux:
```bash
cd ~/.cache/uv/
```

Find cache folder containing my_mini_package:
```bash
find . -type d -name "my_mini_package"
```

This outputs something like:
```bash
./archive-v0/G3V8NCb0ZBzfavnefTMRX/my_mini_package
```

Go to that folder:
```bash
cd ./archive-v0/G3V8NCb0ZBzfavnefTMRX/my_mini_package
```

show content of single_function.py:
```
cat single_function.py
```

This outputs:
```python
def greet(name: str) -> str:
    """Returns a greeting message."""
    return f"Hello, {name}!"
```

Now let's go to the code of my_mini_package in the virtual environment created earlier:
```bash
cd .venv/lib/python3.12/site-packages/my_mini_package
```

Let's edit the file `single_function.py` and change line
```python
return f"Hello, {name}!"
```
to
```python
return f"Hello, {name}! (changed)"
```
and save the file.

If we now take a look at the cached version of my_mini_package (`~/.cache/uv/archive-v0/G3V8NCb0ZBzfavnefTMRX/my_mini_package/single_function.py`), we see that this change was also applied to the cached version of `single_function.py`.

Now, if we would create a new virtual environment (could be anywhere on the same machine with the same user) and install my_mini_package with uv in the same version (`uv pip install my-mini-package==0.1.0`), we use the **changed** cached version of my_mini_package with the changed line in `single_function.py`. This can result in wierd scenarios where we expect the original behaviour of the package but actually use a changed version.
I think that changing a source file of an installed package in a virtual environment can sometimes be useful for testing purposes where you want to check how your code reacts when you change some code in an installed package. But that this results in the case that this change gets installed when you install the same package in a totally new enviornment, is very unexpected and not at all intuitive for me.

Clearing the cache with `uv cache clean` or installing packages with `uv pip install --no-cache <package-name>` removes the cached changes or does not use the cache for installation, respectively.

For me, this would mean that I need to clear the cache everytime (also possible by setting environment variable UV_NO_CACHE) or installing unchached packages everytime, because maybe I have changed a package somewhere and I cannot remember.

Is this intended behaviour? How are the files of the installed packages in the venv-folder linked to the cached files in the uv cache folder? I couldn't find any symbolic links on the related folders.


### Versions
`uv --version`:
```bash
uv 0.8.15
```

Python version: tested with 3.11 and 3.12.

`cat /etc/os-release`:
```bash
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```


### Platform

Ubuntu 22.04.5 amd64

### Version

uv 0.8.15

### Python version

Python 3.11 and Python 3.12

---

_Label `bug` added by @S-Erik on 2025-09-05 10:43_

---

_Comment by @konstin on 2025-09-05 11:59_

It's not intended, it's rather an unfortunate side-effect of hard-linking. We don't want to duplicate files for each venvs, that's both slow and takes a lot of disk space. The ideal solution for that are copy-on-write file systems: We tell the operating system to create something that looks like a copy, but use the same on disk, until either file is edited: Only then do we actually copy the file and change one copy. This is supported on MacOS with `--copy-mode clone` which reflinks through `clonefileat`. On Windows and Linux, there is no or no consistent support for this, so we use hardlinking on these platforms instead.

You can switch to copying files instead by using `--link-mode copy`, with the drawbacks to slow installation and larger on-disk size. An alternative workflow is `git clone`ing the dependency and installing it with `uv pip install -e`, to get an editable install of the dependency that can be freely edited.

---

_Label `bug` removed by @konstin on 2025-09-05 11:59_

---

_Label `question` added by @konstin on 2025-09-05 11:59_

---

_Comment by @S-Erik on 2025-09-05 13:52_

I understand the cause now. Thanks for your swift answer.
But I am not sure if I understand your suggestions. I found that `uv run --link-mode copy` is a valid command but needs a command or script to be invoked with. Are you suggesting using `uv run --link-mode copy pip install ...`?
Currently, I think that setting the `UV_LINK_MODE` environment variable to `copy` might be easier, wouldn't it?

---

_Comment by @konstin on 2025-09-05 15:14_

> Currently, I think that setting the `UV_LINK_MODE` environment variable to `copy` might be easier, wouldn't it?

Yes, that works.

For comprehensiveness, if you want to run it in a manual command, you can do `uv sync --link-mode copy`

---

_Comment by @charliermarsh on 2025-09-06 02:39_

(Using `--link-mode=copy` has downsides, though. It's slower, and it requires more disk space, since you're now storing a separate copy of each file every time you install a package. Personally, I'd recommend sticking to the default link mode, and then using `uv cache clean my_mini_package` or similar as needed.)

---

_Closed by @charliermarsh on 2025-09-06 02:39_

---
