```yaml
number: 6426
title: "`uv venv`'s runtime import patching hooks are unexpected"
type: issue
state: open
author: edmorley
labels:
  - virtualenv
assignees: []
created_at: 2024-08-22T12:07:40Z
updated_at: 2025-02-13T22:51:03Z
url: https://github.com/astral-sh/uv/issues/6426
synced_at: 2026-01-12T15:59:04Z
```

# `uv venv`'s runtime import patching hooks are unexpected

---

_@edmorley_

Hi

When a virtual environment is created using `uv venv`, it adds run-time import hooks in the created `site-packages`, which patch `distutils.dist` and/or `setuptools.dist`:

```
$ uv --version
uv 0.3.1 (Homebrew 2024-08-21)

$ uv venv
Using Python 3.12.5 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

$ ls -1 .venv/lib/python3.12/site-packages/
_virtualenv.pth
_virtualenv.py
```

See:
https://github.com/astral-sh/uv/blob/0.3.1/crates/uv-virtualenv/src/virtualenv.rs#L401-L403
https://github.com/astral-sh/uv/blob/0.3.1/crates/uv-virtualenv/src/_virtualenv.py

It turns out these hooks are copied from the `virtualenv` project:
https://github.com/pypa/virtualenv/blob/20.26.3/src/virtualenv/create/via_global_ref/_virtualenv.py

This was surprising to me, since:
- I had expected `uv venv` to behave more like `python -m venv` (which doesn't add these hooks) than the `virtualenv` project, given (a) the `venv` subcommand name, (b) the `virtualenv` project to me is more commonly associated with older Python versions (prior to the `venv` module existing) or niche use-cases for seeding or API usage.
- There's no mention of these hooks on https://docs.astral.sh/uv/reference/cli/#uv-venv or https://docs.astral.sh/uv/pip/environments/#creating-a-virtual-environment
- Native support for virtual environments has existed since Python 3.3 (see [PEP 405](https://peps.python.org/pep-0405/)), so at first glance it seems unlikely that any issues wouldn't have already been fixed upstream by now, making these patches redundant? Sadly there's no link to an upstream distutils issues in [_virtualenv.py](https://github.com/astral-sh/uv/blob/0.3.1/crates/uv-virtualenv/src/_virtualenv.py) with history or steps to reproduce.
- Distuils has been removed in Python 3.12, and whilst a copy of it lives on in setuptools:
  - this isn't really mentioned in code comments in [_virtualenv.py](https://github.com/astral-sh/uv/blob/0.3.1/crates/uv-virtualenv/src/_virtualenv.py), requiring people to have that knowledge,
  - for Python 3.12 setuptools is no longer installed by default by `ensurepip` or `venv`, and so it feels odd to be installing pre-emptive hooks for it.
- I generally have a negative association with packages that inject custom import hooks into site-packages, having seen performance and compatibility issues with them over the years (eg some of setuptools' hooks).

Some thoughts:
1. Do the issues that these hooks aim to fix still exist? If they really were still a widespread problem, then surely the stdlib's `venv` feature would include these hooks too?
2. Could the fixes be upstreamed to distutils instead? (If only for setuptool's copy of distutils, where it would be easier to have changes be accepted, and would at least mean the hooks could be dropped for Python 3.12+)
3. Should `uv venv` provide an option to opt-out of the hooks?
4. Should `uv venv` skip installing these hooks on Python 3.12+, given that setuptools is more likely to not be installed there (due to  the upstream `ensurepip` and `venv` changes)?
5. More generally, should `uv venv` be looking to align itself with the behaviour of the stdlib's venv module, or the virtualenv project? Which would be least surprising for end-users? (Personally, I would expect to be able to substitute a `python -m venv .venv && uv ...` with `uv venv && uv ...` and for there to be no change in functionality. I haven't used the separate `virtualenv` project to directly create an environment for years.)


---

_Comment by @charliermarsh on 2024-08-22 12:34_

@konstin -- Do you know if we can remove these?

---

_Label `virtualenv` added by @charliermarsh on 2024-08-22 12:34_

---

_Comment by @konstin on 2024-08-22 16:48_

I frankly don't know, i initially copied this file from https://github.com/gaborbernat/virtualenv/blob/main/src/virtualenv/create/via_global_ref/_virtualenv.py to create identical venvs to pypa/virtualenv (an initial goal in gourgeist), but i can't tell if they have any relevance today still.

---

_Comment by @edmorley on 2024-08-22 21:03_

I found some upstream history for the hooks in:
- https://github.com/pypa/virtualenv/issues/1632
- https://github.com/pypa/virtualenv/issues/1663
- https://github.com/pypa/virtualenv/pull/1688

Of note I found this comment:
https://github.com/pypa/virtualenv/issues/1663#issuecomment-596679468

...which said:

> So, MacOS Catalina 10.15.3 came with a stock /usr/bin/python3 (version 3.7.3) - It also came with what looks like a "light weight" virtual env:
> <gives example of `python -m venv` usage
> ...
> This virtualenv doesn't have this issue.

...which suggests at least one of the bugs being worked around by these hooks only occurred with `virtualenv`'s virtual environments, and not the `venv` module's.


---

_Comment by @edmorley on 2024-08-22 21:25_

Hmm so the `distutils.cfg` venv issue was supposedly fixed upstream in distutils in Python 3.3:

- https://github.com/python/cpython/issues/61932
- https://github.com/python/cpython/commit/521ed521317ffd385afaabdd889879e7cb7461df

And that fix is still there in Python 3.11:
https://github.com/python/cpython/blob/795f2597a4be988e2bb19b69ff9958e981cb894e/Lib/distutils/dist.py#L384-L392

Plus in setuptools' adopted copy of distutils:
https://github.com/pypa/setuptools/blob/c11767170a6cc1097b3ba5046d82dd2a9d25290c/setuptools/_distutils/dist.py#L369-L387

The fix involves identify a venv using `sys.prefix != sys.base_prefix` - which will be the case for PEP-405 compliant venvs.

My first thought was that perhaps the environments created by `virtualenv` weren't PEP-405 compliant back then, but digging around it seems they switched to creating `pyvenv.cfg` files in the big rewrite for `virtualenv` 20 :
https://github.com/pypa/virtualenv/commit/d47ab61dc274b84c616f81ff740d1ee369a18871

And https://github.com/pypa/virtualenv/issues/1663 reports using virtualenv 20.

That said, there was a slight deviation from the PEP-405 spec until it was fixed in virtualenv v20.16.7 (Nov 2022):
https://github.com/pypa/virtualenv/pull/2441

---

_Comment by @garthk on 2024-09-03 01:59_

~~I'd like to delete the patch, as it breaks Python 3.6.~~
I'd like to delete the one line of the patch that breaks Python 3.6:

```python
from __future__ import annotations
```

I appreciate the Python team no longer support Python 3.6, but RedHat-style Linux distributions will support Python 3.6 until [releasever 8 expires in 2029](https://access.redhat.com/support/policy/updates/errata#RHEL8_Planning_Guide). I'm happy for the community to move on, in general, but find it frustrating when key tools break due to a lack of care or a surplus of spite rather than necessity.

Update: it occurs to me Python 3.6 projects might be benefiting from some of the other lines in the VIRTUALENV_PATCH. I'd like to keep those lines until either 2029 or the discovery or provision of some means to patch the environment ourselves. I regret we can't [just simply](https://justsimply.dev/) create the file between the environment's creation and use, as they're both happening during `uv pip install .` as invoked by Hatch.

---

_Comment by @edmorley on 2025-02-13 22:50_

I'd like to use `uv venv` rather than `python -m venv`, but this issue blocks doing so, since we don't want these custom hooks installed.

Would you be open to a PR removing these hooks, so uv's venvs match the stdlib's?

---
