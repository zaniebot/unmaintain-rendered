```yaml
number: 13961
title: "Can't install 2 different ruff versions with different Python interpreters"
type: issue
state: closed
author: giampaolo
labels:
  - question
assignees: []
created_at: 2024-10-28T09:52:29Z
updated_at: 2024-10-28T16:27:13Z
url: https://github.com/astral-sh/ruff/issues/13961
synced_at: 2026-01-12T15:54:53Z
```

# Can't install 2 different ruff versions with different Python interpreters

---

_@giampaolo_

Hello. :-) 
At work we're stuck at Python 3.8 to a specific ruff version, but for my everyday development I'm free to use 3.10 with the most up to date version of ruff. It turns out that whatever version of ruff is installed last, it overwrites any previous installation, including if the Python interpreter is different:

```
$ python3.8 -m pip install ruff==0.4.7
Successfully installed ruff-0.4.7
$ python3.8 -m ruff -V
ruff 0.4.7

$ python3.10 -m pip install ruff
Successfully installed ruff-0.7.1
$ python3.10 -m ruff -V
ruff 0.7.1

$ python3.8 -m ruff -V
ruff 0.7.1
```

Also note the following. Different versions are, in fact, installed:

```
$ python3.8 -m pip list | grep ruff
ruff                      0.4.7
$ python3.10 -m pip list | grep ruff
ruff                          0.7.1
```

The problem is the `python3 -m ruff` invocation (I never invoke `ruff` command directly). `python3 -m ruff` executes logic in https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py, so I believe the problem should be there, somewhere. According to what I see in `__main__.py`, the `ruff` executable seems to be determined via $PATH, which appears to be the problem here. Instead I would expect `__main__.py` to do something like:

```python
import ruff
ruff.main()
```

---

_Renamed from "Can't install 2 different ruff versions" to "Can't install 2 different ruff versions with different Python interpreters" by @giampaolo on 2024-10-28 10:25_

---

_Comment by @giampaolo on 2024-10-28 10:32_

Indeed, [find_ruff_bin](https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py#L6C5-L6C20) returns `/home/giampaolo/.local/bin/ruff` with both python 3.8 and 3.10. So apparently when `ruff` is installed via pip, the executable location is the same, and hence it gets overwritten, regardless of the python version being used. This means that if `ruff` is **uninstalled** for Python 3.8, it will also stop working for Python 3.10, because the executable path is the same and gets deleted:

```
$ python3.8 -m pip uninstall ruff
Found existing installation: ruff 0.4.7
Uninstalling ruff-0.4.7:
  Would remove:
    /home/giampaolo/.local/bin/ruff     # <--------------- SEE HERE
    /home/giampaolo/.local/lib/python3.8/site-packages/ruff-0.4.7.dist-info/*
    /home/giampaolo/.local/lib/python3.8/site-packages/ruff/*
Proceed (Y/n)? y
  Successfully uninstalled ruff-0.4.7
  
$ python3.10 -m ruff
Traceback (most recent call last):
  File "/usr/lib/python3.10/runpy.py", line 196, in _run_module_as_main
    return _run_code(code, main_globals, None,
  File "/usr/lib/python3.10/runpy.py", line 86, in _run_code
    exec(code, run_globals)
  File "/home/giampaolo/.local/lib/python3.10/site-packages/ruff/__main__.py", line 61, in <module>
    ruff = os.fsdecode(find_ruff_bin())
  File "/home/giampaolo/.local/lib/python3.10/site-packages/ruff/__main__.py", line 57, in find_ruff_bin
    raise FileNotFoundError(scripts_path)
FileNotFoundError: /usr/local/bin/ruff
```

It's probably worth mentioning that the way `pip` solved this is by installing multiple copies of the exe by appending the python version as prefix:

```
$ ls -l /home/giampaolo/.local/bin/ | grep pip
.rwxrwxr-x   221 giampaolo 28 ott 10:26  pip
.rwxrwxr-x   223 giampaolo  8 feb 12:26  pip2
.rwxrwxr-x   223 giampaolo  8 feb 12:26  pip2.7
.rwxrwxr-x   221 giampaolo 28 ott 10:26  pip3
.rwxrwxr-x   223 giampaolo  6 ago 10:42  pip3.8
.rwxrwxr-x   221 giampaolo 28 ott 10:26  pip3.10
.rwxrwxr-x   224 giampaolo  6 apr 22:43  pip3.12
.rwxrwxr-x   224 giampaolo 21 ott 17:23  pip3.13
```

Perhaps `ruff` could do the same?

---

_Label `question` added by @MichaReiser on 2024-10-28 12:02_

---

_Comment by @MichaReiser on 2024-10-28 12:06_

Thanks for the detailed write-up. 

This seems more of a pip issue than Ruff where it uses the same location for binaries coming from different package versions. 

> Perhaps ruff could do the same?

Ruff isn't python version-dependent. There's no ruff for python 3.8. The constraint that Ruff 4.7 should be used is one of your own. 

I suggest you to do one of the following:

* Install ruff locally in a venv instead of globally in your home directory. That way projects using Python 3.8 can pin Ruff to an older version. 
* Use `pipx` or `uvx` to run a specific ruff version. For example with `uv` you can do `uvx ruff@0.5` to run version 0.5 and use `uvx ruff` to use the latest ruff version ([docs](https://docs.astral.sh/uv/guides/tools/))


---

_Comment by @giampaolo on 2024-10-28 14:31_

> Ruff isn't python version-dependent.

Thanks, this clarifies things a bit. This looks kind of a grey area though, because, de-facto, each python interpreter does have its own ruff installed:

```
$ python3.8 -m pip list | grep ruff
ruff                      0.4.7
$ python3.10 -m pip list | grep ruff
ruff                          0.7.1
```

E.g. I can successfully invoke the 2 different ruff versions if I:

* create a copy of `ruff` as in `cp /home/giampaolo/.local/bin/ruff /home/giampaolo/.local/bin/ruff3.8` **after** I install ruff for 3.8 (first installation) 
* after installing `ruff` for python 3.10, I manually edit `/home/giampaolo/.local/lib/python3.8/site-packages/ruff/__main__.py` so that it points to `/home/giampaolo/.local/bin/ruff3.8` instead of `/home/giampaolo/.local/bin/ruff`

To further expand: `python3 -m` option is there precisely for use cases like mine, that is: invoke a module which defines a `__main__.py` file by using a specific python version. Or to put it another way: do not rely on invoking scripts from $PATH. E.g. I can successfully invoke `python3 -m black` in such a fashion, and get the expected behavior. 

`ruff` works differently in this regard because its `__main__.py` script looks into $PATH for an executable called "ruff" to execute as a subprocess (see [source](https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py#L60-L68)), and this is somewhat  unusual. But then again, perhaps this is because ruff is written in rust, and therefore it cannot be invoked directly from python as in `import ruff; ruff.main()`. 

With that said, I fully realize that a better way to proceed would be to simply use a venv. This is a bad habit I never managed to adjust over the years, so blame on me. :-) 

---

_Comment by @MichaReiser on 2024-10-28 14:39_

We did change this behavior recently. I'm interested to hear @gaborbernat thoughts who worked on it. This might also be related to https://github.com/astral-sh/ruff/pull/13881

> With that said, I fully realize that a better way to proceed would be to simply use a venv. This is a bad habit I never managed to adjust over the years, so blame on me. :-)

That's why I use `uvx` for tools :) No dealing with venv's

---

_Comment by @charliermarsh on 2024-10-28 14:44_

> Indeed, [find_ruff_bin](https://github.com/astral-sh/ruff/blob/main/python/ruff/__main__.py#L6C5-L6C20) returns /home/giampaolo/.local/bin/ruff with both python 3.8 and 3.10. So apparently when ruff is installed via pip, the executable location is the same, and hence it gets overwritten, regardless of the python version being used.

This is a function of using the system Python, and whatever choices your system Python seems to have made. They share a `bin` directory? In that case you're going to suffer from more issues than just this

---

_Comment by @charliermarsh on 2024-10-28 14:46_

I don't think there's really anything to solve here -- it doesn't make sense for us to introduce the complexity of having versioned executables, especially given that they're identical. I'd suggest using `uvx` or virtual environments, or installing with our installers rather than pip.


---

_Comment by @giampaolo on 2024-10-28 15:03_

> This is a function of using the system Python, and whatever choices your system Python seems to have made. They share a bin directory? In that case you're going to suffer from more issues than just this.

Completely agree. My main point here though is that `python3.X -m ruff` works in an unexpected way. 
E.g. I don't have this problem with `black`, `flake8` and others, because their `__main__.py` does not rely on invoking an external script defined in $PATH as a subprocess. The promise of the `-m` option should be to execute a python package as a script by using the same python interpreter invoked from the command line.

---

_Comment by @gaborbernat on 2024-10-28 15:12_

> At work we're stuck at Python 3.8 to a specific ruff version, but for my everyday development I'm free to use 3.10 with the most up to date version of ruff.

`pipx` has a `--suffix` argument, that allows you to install different versions/interpreters; that way you could do

```
uv tool install ruff
ruff --version

uv tool install ruff<=0.4 -s 0.4 
ruff0.4 --version

uv tool install -p 3.8 ruff -s '-3.8'
ruff-3.8 --version
```

This is quite handy when dealing with different versions of tools, could be worth `uv` having similar too.

---

_Comment by @giampaolo on 2024-10-28 16:26_

I just realized that `/home/giampaolo/.local/bin/ruff` is a _binary_ executable, and that the ruff installation only consists of `__main__.py`:

```
~$ ls -la /home/giampaolo/.local/lib/python3.10/site-packages/ruff
Permissions Size User      Date Modified Name
drwxrwxr-x     - giampaolo 28 ott 12:43  __pycache__
.rw-rw-r--     0 giampaolo 28 ott 12:43  __init__.py
.rw-rw-r--  2,3k giampaolo 28 ott 12:43  __main__.py
```

As such, I now see why `__main__.py` can't do anything else other than simply executing the `ruff` binary file. 
This also made me realize that invoking ruff via `python3 -m ruff` is considerably slower:

```
~$ time python3 -m ruff -V
ruff 0.7.1
real	0m0,020s
user	0m0,015s
sys	0m0,005s

~$ time ruff -V
ruff 0.7.1
real	0m0,003s
user	0m0,003s
sys	0m0,000s
```

I therefore understand how addressing my request is probably not possible, so please feel free to close this issue, and thanks for all the quick responses I've received from all.

---

_Comment by @charliermarsh on 2024-10-28 16:27_

Thanks for following up -- that's generally what I was trying to convey but it's a nuanced issue.

---

_Closed by @charliermarsh on 2024-10-28 16:27_

---
