```yaml
number: 11490
title: "`uv --project=. run examples/example1.py` should add `.` to PYTHONPATH and otherwise allow `example1.py` to `import` the current project"
type: issue
state: closed
author: mdengler
labels:
  - bug
assignees: []
created_at: 2025-02-13T19:56:44Z
updated_at: 2025-02-13T21:24:28Z
url: https://github.com/astral-sh/uv/issues/11490
synced_at: 2026-01-12T16:00:38Z
```

# `uv --project=. run examples/example1.py` should add `.` to PYTHONPATH and otherwise allow `example1.py` to `import` the current project

---

_@mdengler_

### Summary

Similar to #6733, these commands should work:

```
mkdir bird-uv-issue
cd bird-uv-issue/
mkdir bird
echo -e 'def hello():\n  print("hello bird")' > bird/__init__.py
mkdir examples
echo -e "import bird\nprint(bird.hello())" > examples/example1.py
uv --project=$PWD run examples/example1.py
```

...however, they don't:


```
$ uv --version
uv 0.5.25

$ mkdir bird-uv-issue
$ cd bird-uv-issue/
$ mkdir bird
$ echo -e 'def hello():\n  print("hello bird")' > bird/__init__.py
$ mkdir examples
$ echo -e "import bird\nprint(bird.hello())" > examples/example1.py
$ tree .
.
├── bird
│   └── __init__.py
└── examples
    └── example1.py

3 directories, 2 files

$ uv --project=$PWD run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --project=. run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --directory=$PWD run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --directory=. run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --directory=examples run examples/example1.py
error: Failed to spawn: `examples/example1.py`
  Caused by: No such file or directory (os error 2)

  $ uv --directory=examples run example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'
```


The desired behavior is:
```
$ uv --project=. run examples/example1.py 
hello bird
None
```

...like:

```
$ PYTHONPATH=. uv run examples/example1.py 
hello bird
None
```

### Platform

Linux 6.12.4-100.fc40.x86_64 x86_64 GNU/Linux

### Version

uv 0.5.25

### Python version

Python 3.12.8

---

_Label `bug` added by @mdengler on 2025-02-13 19:56_

---

_Comment by @charliermarsh on 2025-02-13 20:03_

Thanks for filing. My initial reaction is that if you want your project (`bird`) to be importable like this, then it should be structured as a valid Python package? As-is, we don't install the project into the environment at all, because it doesn't have a build system declared. I suspect @zanieb will have an opinion here too.


---

_Comment by @mdengler on 2025-02-13 20:35_

> My initial reaction is that if you want your project (bird) to be importable like this, then it should be structured as a valid Python package? 

Understood -- but not exactly what you mean in practice.  This project is a valid python package, in that it has `bird/__init__.py`, and is `import`able.  Do you mean some distribution metadata in `pyproject.toml`?

---

_Comment by @mdengler on 2025-02-13 20:44_

I guess from the point of view of `examples/examples1.py`, I think "`git clone` this repo and run `uv run examples/example1.py`" is a pretty interesting use case -- it seems a really direct and simple and common way to structure projects with example scripts (see, for example, https://github.com/ageitgey/face_recognition/tree/master/examples ).

Also, it seems this is exactly what the reporter in #6733 was asking for, and though that issue is marked as closed, I don't see how it's been addressed by `--project` (I mean that literally: I assume it *has* been, but I can't articulate what commands to run that include `--project` that actually allow #6733's motivating example (in the issue report) to work).  This issue report's example is the same type of operation, though perhaps with a slightly different use case.

---

_Comment by @zanieb on 2025-02-13 20:48_

To clarify, our behavior matches `python examples/examples1.py`, right?

---

_Comment by @mdengler on 2025-02-13 20:58_

> To clarify, our behavior matches `python examples/examples1.py`, right?

Definitely.  `uv run --project=. ...` has different behavior than `python`, though, like setting up the virtual environment and so forth, so I think it's a good thing that `uv run --project=. ...`'s behavior would deviate in this respect.


---

_Comment by @zanieb on 2025-02-13 21:02_

I mean... ensuring an environment is up to date is different than modifying the standard `PYTHONPATH` behavior when running scripts.

---

_Comment by @charliermarsh on 2025-02-13 21:05_

> Do you mean some distribution metadata in pyproject.toml?

Yeah, sorry. For example, if you add this to `bird-uv-issue/pyproject.toml`:

```toml
[project]
name = "bird"
version = "0.1.0"
requires-python = ">=3.13.0"
dependencies = []

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Then `uv run examples/example1.py` works, as does `uv run example1.py` from `./examples`, etc.

---

_Comment by @mdengler on 2025-02-13 21:17_


> I mean... ensuring an environment is up to date is different than modifying the standard `PYTHONPATH` behavior when running scripts.

Right, though `PYTHONPATH` and the environment added to `sys.path` are very related.  I suppose "`poetry` does it" or "`venv` does it" doesn't hold much water :)...

Since `uv run <script>` is so useful for many things, including modifying `sys.path`, it seems like there should be a way to get the current project into the environment...which I think @charliermarsh has just clued me in to (use `hatchling` in `pyproject.toml`'s `build-system` section).



---

_Comment by @mdengler on 2025-02-13 21:18_

> > Do you mean some distribution metadata in pyproject.toml?
> 
> Yeah, sorry. For example, if you add [`hatchling`] to `bird-uv-issue/pyproject.toml`:
[...]
> Then `uv run examples/example1.py` works, as does `uv run example1.py` from `./examples`, etc.

Excellent, thanks.  This should do it; thanks!

For future reference, `build-system = "hatchling"` adds the project directory to `sys.path`:


Before:
```
$ uv run examples/example1.py                                            
/home/martin/src/bird-uv-issue/examples                                                                                                                                                                                                       
/usr/lib64/python312.zip                                                                                                                                                                                                                      
/usr/lib64/python3.12                                                                                                                                                                                                                         
/usr/lib64/python3.12/lib-dynload                                                                                                                                                                                                             
/home/martin/src/bird-uv-issue/.venv/lib64/python3.12/site-packages                                                                                                                                                                           
/home/martin/src/bird-uv-issue/.venv/lib/python3.12/site-packages                                                                                                                                                                             
Traceback (most recent call last):                                                                                                                                                                                                            
  File "/home/martin/src/bird-uv-issue/examples/example1.py", line 3, in <module>                                                                                                                                                             
    import bird                                                                                                                                                                                                                               
ModuleNotFoundError: No module named 'bird'                
```

After:
```
$ uv run examples/example1.py                                                                                                                                                                  
   Built bird @ file:///home/martin/src/bird-uv-issue                                                                                                                                                                                         
Installed 1 package in 0.70ms                                                                                                                                                                                                                 
/home/martin/src/bird-uv-issue/examples                                                                                                                                                                                                       
/usr/lib64/python312.zip                                                                                                                                                                                                                      
/usr/lib64/python3.12                                                                                                                                                                                                                         
/usr/lib64/python3.12/lib-dynload                                                                                                                                                                                                             
/home/martin/src/bird-uv-issue/.venv/lib64/python3.12/site-packages                                                                                                                                                                           
/home/martin/src/bird-uv-issue                         
/home/martin/src/bird-uv-issue/.venv/lib/python3.12/site-packages
hello bird                                                                                                             
None                                                                              
```

---

_Closed by @mdengler on 2025-02-13 21:18_

---
