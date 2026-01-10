---
number: 9514
title: "`uv sync` and `uv run --no-project` end up in different version with same requirements"
type: issue
state: closed
author: ThomasHezard
labels:
  - question
assignees: []
created_at: 2024-11-28T19:27:43Z
updated_at: 2024-12-04T11:43:12Z
url: https://github.com/astral-sh/uv/issues/9514
synced_at: 2026-01-10T01:24:42Z
---

# `uv sync` and `uv run --no-project` end up in different version with same requirements

---

_Issue opened by @ThomasHezard on 2024-11-28 19:27_

Hello

I had an issue with `torch` dependency. I am pretty sure the problem comes from `torch` and I opened an issue on that matter: https://github.com/pytorch/pytorch/issues/141781

However, while investigating my issue I encountered a `uv` behaviour I do not fully understand.

The issue can be reproduced with the following very simple `pyproject.toml`: 
```
[project]
name = "uv-torch-test"
version = "0.1.0"
requires-python = ">=3.8"
dependencies = [
    "torch",
]
```

If you `uv sync` with Python 3.8 (either using `.python-version` or `--python 3.8` option), it fails with the following error
```
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```
I think this is expected given the issue described here: https://github.com/pytorch/pytorch/issues/141781.

However, the following command succeeds and indicates that uv ends up installing `torch` 2.4.1 in this case
```
uv run --no-project --python 3.8 --with "torch" python -c "import torch; print(torch.__version__)"
```

In conclusion:

- One feature request:
  - Having an option to print the package versions in `uv run --no-project` the same way they are printed in `uv sync` would have helped me a lot. The only solution to print them today is to use the `--verbose` option which is extremely verbose. I use a lot the "`uv run --no-project`" command in my unit testing CI jobs, and adding the `--verbose` would make the test results and logs difficult to read, whereas I just need the package versions.

- Two questions:
  - Why does `uv run --no-project --python 3.8 --with "torch"` dependency resolution ends up choosing `2.4.1` and not `2.5.1` when `2.5.1` is identified as compatible with Python 3.8 by `uv sync`?
  - Would #8686 help avoid this behaviour? (really waiting for this one, it would help on several issues I encountered 😉 )

Thank you 🙏

---

_Comment by @ThomasHezard on 2024-11-29 11:53_

Hello

Quick update after some additional testing.

`pip install torch` in a blank Python 3.8 environment installs 2.4.1, which indeed is the real last Python 3.8 compatible `torch` release, despite what the metadata of the version says. This leads me to two conclusions:
  - `pip` does not respect its own interface, in the sense that it does not fail if a package metadata is not properly written (in that `torch` 2.5.0/2.5.1 case, the package does not provide the wheels or sdist for all supposedly compatible Python versions), which bugs me a lot as a developer (as well as the fact that this has been pushed on PyPI without any problem) 😱 
  - I am not sure about that, but I think what `pip` does under the hood is that it looks at the available wheels or sdist rather than looking at the `requires_python` specifications (I looked a bit at the `pip` sources but I did not find where this is done yet).

On the `uv` side:
- `uv sync` seems to resolve the dependencies by looking at the packages metadata, which totally makes sense to me, and the error I got definitely comes from a problem in the PyTorch releases and is therefore fully legit I think it is a good think `uv` reports it as an error.
- I still do not understand why `uv run --no-project --python 3.8 --with "torch"` does not fail. Does it use `pip` resolver under the hood?

In general, I wanted to say that `uv sync` resolver seems really excellent to me as its rigorous "fail-fast" behaviour helped me spot errors in packages metadata (mine or others) really easily, and write perfectly reliable `pyproject.toml` and lock files.

---

_Comment by @charliermarsh on 2024-11-29 16:44_

So, I think `uv run --no-project --python 3.8 --with "torch"` will look for the first version of `torch` that has a wheel for Python 3.8, whereas `uv sync` will look for the first version of `torch` that's compatible with your project, and attempt to use that for all supported Python versions. In this case, it picks 2.5.1, since it has wheels for _some_ supported Python versions. It's related to #5182.

---

_Comment by @charliermarsh on 2024-11-29 16:45_

The difference is that `uv sync` is doing a "universal" solve, whereas `uv run --with` only has to solve for your _current_ platform / environment, since it's not creating a universal lockfile.

---

_Comment by @charliermarsh on 2024-11-29 16:45_

I think https://github.com/astral-sh/uv/pull/8686 would indeed help though it's definitely a tricky case.

---

_Closed by @charliermarsh on 2024-11-29 16:45_

---

_Comment by @charliermarsh on 2024-11-29 16:45_

(Happy to answer any further questions.)

---

_Label `question` added by @charliermarsh on 2024-11-29 16:45_

---

_Comment by @ThomasHezard on 2024-11-29 18:04_

> The difference is that `uv sync` is doing a "universal" solve, whereas `uv run --with` only has to solve for your _current_ platform / environment, since it's not creating a universal lockfile.

One more question to be sure I understand well :)

My understanding was that, whether you try to resolve the `torch` for `Python>=3.8` (universal) or `Python==3.8`(current environment), the obvious resolution seems to be
- 2.5.1 if you look at the metadata (the `requires_python` field),
- 2.4.1 if you look at the available wheels,

because `torch` released the 2.5.0 and 2.5.1 versions with a `requires_python` at 3.8 while only providing wheels for Python 3.9 to 3.13 (see https://github.com/pytorch/pytorch/issues/141781).

In a sense my question is: Do `uv sync` and `uv run --no-project` look at the `requires_python` or the available wheels to resolve dependencies?

If both do look at the available wheels, I do not understand why the universal resolution (with `uv sync` or `uv lock`) chooses 2.5.1 (without forking for Python 3.8) and not 2.4.1 because 2.4.1 if the most recent version compatible with Python >= 3.8, not 2.5.1. 

Is there something I misunderstood there in this "universal" resolution concept?   


---

_Comment by @ThomasHezard on 2024-11-29 18:07_

I'm not fluent enough in `rust` to look at the `uv` code, but I tried to interpret the `uv` verbose logs.    
I'm not sure what to interpret from these 😅 

--

Here is (part of) the output of `uv lock --python 3.8 --verbose` with the `pyproject.toml` of my initial message:
```
DEBUG uv 0.5.5 (Homebrew 2024-11-27)
[...]
DEBUG Using Python request `3.8` from explicit request
[...]
Using CPython 3.8.20
[...]
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8
[...]
DEBUG Adding transitive dependency for uv-torch-test==0.1.0: torch*
DEBUG Found fresh response for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (*)
DEBUG Selecting: torch==2.5.1 [compatible] (torch-2.5.1-cp310-cp310-manylinux1_x86_64.whl)
```

--

Here is (part of) the output of `uv run --no-project --verbose --python 3.8 --with "torch" python`:
```
DEBUG uv 0.5.5 (Homebrew 2024-11-27)
[...]
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.8 in virtual environments, managed installations, or search path
[...]
DEBUG Using Python 3.8.20 interpreter at: [...]/.local/share/uv/python/cpython-3.8.20-macos-aarch64-none/bin/python3.8
[...]
DEBUG Solving with installed Python version: 3.8.20
DEBUG Solving with target Python version: >=3.8.20
DEBUG Adding direct dependency: torch*
DEBUG Found fresh response for: https://pypi.org/simple/torch/
DEBUG Searching for a compatible version of torch (*)
DEBUG Searching for a compatible version of torch (<2.5.1 | >2.5.1)
DEBUG Searching for a compatible version of torch (<2.5.0 | >2.5.0, <2.5.1 | >2.5.1)
DEBUG Selecting: torch==2.4.1 [compatible] (torch-2.4.1-cp38-none-macosx_11_0_arm64.whl)
```

--



---

_Comment by @ThomasHezard on 2024-11-29 18:21_

One more experiment.

I compiled the version from https://github.com/astral-sh/uv/pull/8686 and ran
```
uv lock --python 3.8 --multi-version lastest
``` 
on the `pyproject.toml` from my initial message.    
=> `uv` still resolves `torch` 2.5.1 for all Python versions (3.8 to 3.13)
=> `uv` still fails to `sync` with Python 3.8 as it tries to install `torch`2.5.1 (same error as my first message).    

So unfortunately, https://github.com/astral-sh/uv/pull/8686 won't help in that case and the mystery of why my two commands do not behave the same way is still bothering me a bit 🙂 

I can provide logs if that can help.

---

_Comment by @charliermarsh on 2024-11-29 18:57_

We look at the `requires-python`, not the available wheels. We look at the wheels to determine whether there's at least one wheel in the space of supported Python versions. The assumption is that other versions will build from source. PyTorch doesn't ship source, so you end up with a version that may not support (e.g.) Python 3.8. Hence the open issue that I linked earlier.

---

_Comment by @ThomasHezard on 2024-11-29 19:13_

Thank you, absolutely all clear, now I understand why the two commands end up with different versions 👌    

This totally makes sense and is quite clever to get a fast resolution while taking advantage of PyPI API.

I must say that the thing that bugs me the most here is that nothing happened in both `torch` release process and `PyPI` backend when faced to what seems to me two ill-formed releases, in the sense that the `requires-python` is not respected by the either wheels or sdists 😮
But maybe there is something I'm missing here.

It is quite funny how `uv` helped me spot several eronous dependency specs in packages really quickly (see https://github.com/astral-sh/uv/issues/9211#issuecomment-2484266926 for another example).   
For me that is a huge advantage of `uv`, this is the first time I find such an efficiently rigorous (which I love) environment manager in the Python world 👏     
It helps me test my packages on different environment really efficiently and make absolutely reliable releases 🤩 

---

_Comment by @ThomasHezard on 2024-12-04 11:43_

@charliermarsh    

FYI: the problem on PyTorch side is being taken care of: https://github.com/pytorch/pytorch/issues/141781.  

No sure when, but this (and https://github.com/astral-sh/uv/issues/5182 if I understood well) should be fixed in the near future, without having to specify complex version constraints in order to fix in `uv` what is in fact a bug on PyTorch side.


---
