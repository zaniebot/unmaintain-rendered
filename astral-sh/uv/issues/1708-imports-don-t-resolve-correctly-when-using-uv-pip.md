```yaml
number: 1708
title: "Imports don't resolve correctly when using `uv pip install -e`"
type: issue
state: closed
author: philipp-eisen
labels:
  - question
  - installer
  - external
assignees: []
created_at: 2024-02-19T16:58:54Z
updated_at: 2024-06-19T09:44:25Z
url: https://github.com/astral-sh/uv/issues/1708
synced_at: 2026-01-12T15:58:31Z
```

# Imports don't resolve correctly when using `uv pip install -e`

---

_@philipp-eisen_

## How to reproduce
Say you have the following file structure:

```
├── random_package
│   ├── random_package
│   │   └── __init__.py
│   └── setup.py
```


```
uv venv
uv pip install -e random_package
python -c "import random_package; print(random_package.__path__)" # ❌ prints a namespace path to ./random_package
mkdir some_other_dir
cd some_other_dir
python -c "import random_package; print(random_package.__path__)" # ✅ prints correct path to ./random_package/random_package
```


![Screenshot](https://github.com/astral-sh/uv/assets/8607233/1eef17a6-ef1c-4478-b970-0d765f2833bc)


I created a repo with the files to reproduce here: https://github.com/philipp-eisen/uv-editable-install-issue

---

_Label `bug` added by @zanieb on 2024-02-19 17:00_

---

_Comment by @zanieb on 2024-02-19 17:00_

Thanks for the report!

---

_Label `installer` added by @zanieb on 2024-02-19 17:01_

---

_Comment by @charliermarsh on 2024-02-20 04:52_

I see the same behavior with `pip` -- have you compared to `pip install -e`?

```shell
uv-editable-install-issue on  main [?]
❯ virtualenv .venv
created virtual environment CPython3.12.1.final.0-64 in 86ms
  creator CPython3macOsBrew(dest=/Users/crmarsh/workspace/puffin/uv-editable-install-issue/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, via=copy, app_data_dir=/Users/crmarsh/Library/Application Support/virtualenv)
    added seed packages: pip==23.3.2
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
uv-editable-install-issue on  main [?]
❯ v
uv-editable-install-issue on  main [?] via 🐍 v3.12.1 (.venv)
❯ pip install -e random_package
Obtaining file:///Users/crmarsh/workspace/puffin/uv-editable-install-issue/random_package
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Building wheels for collected packages: random_package
  Building editable for random_package (pyproject.toml) ... done
  Created wheel for random_package: filename=random_package-1.0-0.editable-py3-none-any.whl size=2548 sha256=c5099053323ae8f975a2e36259bbb1c7b0b830f52f0b23420d78ab359146187e
  Stored in directory: /private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/pip-ephem-wheel-cache-6dchil35/wheels/c2/79/66/ef5932c9c578f59adf4a1aa4a6130056e0158ee769622e464e
Successfully built random_package
Installing collected packages: random_package
Successfully installed random_package-1.0

[notice] A new release of pip is available: 23.3.2 -> 24.0
[notice] To update, run: pip install --upgrade pip
uv-editable-install-issue on  main [?] via 🐍 v3.12.1 (.venv)
❯ python
Python 3.12.1 (main, Dec  7 2023, 20:45:44) [Clang 15.0.0 (clang-1500.1.0.2.5)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import random_package
>>> print(random_package.__path__)
_NamespacePath(['/Users/crmarsh/workspace/puffin/uv-editable-install-issue/random_package'])
```

---

_Label `needs-mre` added by @charliermarsh on 2024-02-20 04:52_

---

_Comment by @philipp-eisen on 2024-02-20 12:51_

@charliermarsh 
I dug a bit deeper, and have some more data, but don't really know why it behaves that way:



1. `venv` on my Macbook -> "wrong" behavior -- same as what you posted, though I have installed and used uv on my macbook
2. new conda environment on my macbook --> "correct" behavior 
3. `venv` on Google Cloud Shell (haven't used it before so -- "clean" install of whatever is in that vm image) --> "correct" behavior


My guess is that it has something to do with that venv just symlinks to the python executable you create the environment with, whereas conda downloads a new executable. But there is still some `uv` related (I think) missing piece that makes the imports wrong.



For reference, I did the following
```
git clone https://github.com/philipp-eisen/uv-editable-install-issue.git
cd uv-editable-install-issue
python -m venv .venv
source .venv/bin/activate
python -c "import random_package; print(random_package.__path__)"
```










---

_Comment by @philipp-eisen on 2024-03-25 13:33_

This is still an issue btw. I.e. doesn't happen with pip in a clean environment. 

---

_Comment by @Jorricks on 2024-06-17 19:38_

I am noticing the same thing with editable installations on MacOS

I have a directory setup as such;
```
- lib_1
- lib_1/lib_1
- lib_1/lib_1/__init__.py
- lib_1/lib_1/some_folder
- lib_1/lib_1/some_folder/__init__.py
- lib_1/lib_1/some_folder/module.py
- lib_1/setup.py
- lib_2/
- lib_2/lib_2
- lib_2/lib_2/__init__.py
- lib_2/lib_2/module.py
- lib_2/setup.py
```

When running the pip install with editable I get;
```
>>> import lib_1; print(lib_1.__path__)
['/Users/abc/my_repo/lib_1/lib_1']
```
However, when I installed it through `uv` I get;
```
>>> import lib_1; print(lib_1.__path__)
_NamespacePath(['/Users/abc/my_repo/lib_1'])
```

Also tried to fix it with changing the argument to add `#subdirectory=lib_1`, but also to no avail
```
-e file:///Users/abc/my_repo/lib_1#subdirectory=lib_1
```

This also becomes an issue for monorepo's with MyPy, because now it can not resolve the package correctly and you get errors as this;
```
lib_2.import_lib_1.py:1: error: Cannot find implementation or library stub for module named "lib_1.some_folder.module"  [import-not-found]
```
Of course you can fix this with including all of the subdirectories into the PATH manually but that's me not fixing the root issue, but instead moving around it, which is always worse then fixing the root 🤷‍♂️ 

Note; I made some edits as my investigation went on :)

---

_Comment by @carljm on 2024-06-17 20:28_

@Jorricks Can you provide a full reproduction example where you get different behavior from `uv` vs `pip`? It's important to have the exact commands run, because factors like e.g. the cwd and the exact way that Python is invoked when trying the import are all very important here.

@philipp-eisen If you have repro instructions showing a difference in behavior between `uv` and `pip`, that would be really useful as well. I'm a bit confused by your most recent updates, because you mention that `uv` and `pip` behave differently, but the list of repro commands that you showed don't include installing anything with either `uv` or `pip`.

At the moment, without a reproducible example, it's really not clear whether the differences in behavior described above are actually due to `uv` vs `pip` or to some other factor. So far I haven't been able to reproduce a difference in behavior.

Some information that may be useful, for reference:

It is normal and expected behavior in Python that an empty string is the first entry in `sys.path` (only if Python is executed like `python` and not like `python script_file.py`), and when resolving imports the empty string is always resolved as the current working directory. This means that in general imports will be preferentially resolved relative to the current working directory, which could cause the behavior described above as "wrong" (i.e. resolving the import to the outer directory instead of the inner one.)

If the current working directory is not the outer directory containing the outer `lib1` or `random_package` directory (either because Python is run from somewhere else, or because something in-process calls `os.chdir()` and changes the current working directory), that would make the "wrong" behavior go away.

If safe-path mode (`-P` or `PYTHONSAFEPATH` env var) or isolated mode (`-I`) are active, this empty string is not added as the first entry in `sys.path`, and then the "wrong" behavior would also go away.

If Python is run like `python path/to/some/file.py`, then the directory containing `file.py` is set as the first entry on `sys.path` instead of the empty string; this could also make the "wrong" behavior go away.

---

_Comment by @Jorricks on 2024-06-17 20:55_

> @Jorricks Can you provide a full reproduction example where you get different behavior from `uv` vs `pip`? It's important to have the exact commands run, because factors like e.g. the cwd and the exact way that Python is invoked when trying the import are all very important here.
> 
> @philipp-eisen If you have repro instructions showing a difference in behavior between `uv` and `pip`, that would be really useful as well. I'm a bit confused by your most recent updates, because you mention that `uv` and `pip` behave differently, but the list of repro commands that you showed don't include installing anything with either `uv` or `pip`.
> 
> At the moment, without a reproducible example, it's really not clear whether the differences in behavior described above are actually due to `uv` vs `pip` or to some other factor. So far I haven't been able to reproduce a difference in behavior.
> 
> Some information that may be useful, for reference:
> 
> It is normal and expected behavior in Python that an empty string is the first entry in `sys.path` (only if Python is executed like `python` and not like `python script_file.py`), and when resolving imports the empty string is always resolved as the current working directory. This means that in general imports will be preferentially resolved relative to the current working directory, which could cause the behavior described above as "wrong" (i.e. resolving the import to the outer directory instead of the inner one.)
> 
> If the current working directory is not the outer directory containing the outer `lib1` or `random_package` directory (either because Python is run from somewhere else, or because something in-process calls `os.chdir()` and changes the current working directory), that would make the "wrong" behavior go away.
> 
> If safe-path mode (`-P` or `PYTHONSAFEPATH` env var) or isolated mode (`-I`) are active, this empty string is not added as the first entry in `sys.path`, and then the "wrong" behavior would also go away.
> 
> If Python is run like `python path/to/some/file.py`, then the directory containing `file.py` is set as the first entry on `sys.path` instead of the empty string; this could also make the "wrong" behavior go away.

Thank you for this information. I was not aware of the empty path resolving to the current working directory.
Still, it does not really explain why editable pip installs do resolve to the correct folder and editable uv installs don't.
I will see if I can provide a minimal working example tomorrow. Thanks :)

---

_Comment by @Jorricks on 2024-06-17 21:15_

Okay I think I figured out why the result is different for `pip` then for `uv`.
In Python we add `EditableFinders` (as they call it in `uv`) to the `sys.meta_path`.
```
$ python3
Python 3.8.10 (v3.8.10:3d8993a744, May  3 2021, 09:09:08) 
[Clang 12.0.5 (clang-1205.0.22.9)] on darwin
>>> import sys
>>> sys.meta_path
[<_distutils_hack.DistutilsMetaFinder object at 0x10aa73400>, <class '_frozen_importlib.BuiltinImporter'>, <class '_frozen_importlib.FrozenImporter'>, <class '_frozen_importlib_external.PathFinder'>,  <class '__editable___lib_1_1_0_dev0_finder._EditableFinder'>, <class '__editable___lib_2_1_0_dev0_finder._EditableFinder'>]
>>> import lib_1; print(lib_1.__path__)
_NamespacePath(['/Users/abc/my_repo/lib_1'])
```

Now the problem resides in that the `PathFinder` comes before the EditableFinder. That causes it to first find the path locally and therefore return that. However, when we change the code in the `EditableFinder.install` method;
```
    if not any(finder == _EditableFinder for finder in sys.meta_path):
        # before
        # sys.meta_path.append(_EditableFinder)
        # after
        sys.meta_path.insert(len(sys.meta_path) - 2,_EditableFinder)
```

We get the expected result;
```
$ python3
Python 3.8.10 (v3.8.10:3d8993a744, May  3 2021, 09:09:08) 
[Clang 12.0.5 (clang-1205.0.22.9)] on darwin
>>> import sys
>>> sys.meta_path
[<_distutils_hack.DistutilsMetaFinder object at 0x10aa73400>, <class '_frozen_importlib.BuiltinImporter'>, <class '_frozen_importlib.FrozenImporter'>, <class '__editable___lib_1_1_0_dev0_finder._EditableFinder'>, <class '__editable___lib_2_1_0_dev0_finder._EditableFinder'>, <class '_frozen_importlib_external.PathFinder'>,]
>>> import lib_1; print(lib_1.__path__)
['/Users/abc/my_repo/lib_1/lib_1']
```


After some googling where the editable installations were coming from, I found out that most likely it comes from setuptools and it seems to be a known issues;
https://github.com/pypa/setuptools/issues/3518

---

_Comment by @Jorricks on 2024-06-17 21:32_

Alright, my search has come to an ending.
Turns out this is indeed related to setuptools#3518.

When I ran `uv pip sync --config-setting editable_mode=compat requirements.txt` it worked fine :)!

I suppose my issue would have also shown up with a newer pip version

Thanks for the help :)!

---

_Label `bug` removed by @zanieb on 2024-06-17 21:38_

---

_Label `needs-mre` removed by @zanieb on 2024-06-17 21:38_

---

_Label `question` added by @zanieb on 2024-06-17 21:38_

---

_Label `upstream` added by @zanieb on 2024-06-17 21:39_

---

_Closed by @zanieb on 2024-06-19 00:35_

---

_Comment by @philipp-eisen on 2024-06-19 09:44_

Thanks @Jorricks and sorry for being a bit too late to provide any useful info here.

---
