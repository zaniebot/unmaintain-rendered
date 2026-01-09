---
number: 4714
title: "uv incorrectly excludes pre-release Python versions when `requires-python` is greater or equal to that version"
type: issue
state: closed
author: notatallshaw
labels:
  - bug
assignees: []
created_at: 2024-07-01T23:13:14Z
updated_at: 2024-07-04T20:06:53Z
url: https://github.com/astral-sh/uv/issues/4714
synced_at: 2026-01-07T13:12:17-06:00
---

# uv incorrectly excludes pre-release Python versions when `requires-python` is greater or equal to that version

---

_Issue opened by @notatallshaw on 2024-07-01 23:13_

Tested on:

* uv 0.2.18
* Python 3.13.0b3

Create a pyproject.toml like so:

```toml
[project]
name = "foo"
version = "0.1.0"
requires-python = ">=3.13"

[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"
```

Dry install with uv fails:

```
$  uv pip install --dry-run .
  × No solution found when resolving dependencies:
  ╰─▶ Because the current Python version (3.13.0b3) does not satisfy Python>=3.13 and foo==0.1.0 depends on Python>=3.13, we can conclude that
  	foo==0.1.0 cannot be used.
  	And because only foo==0.1.0 is available and you require foo, we can conclude that the requirements are unsatisfiable.
```

Dry install with pip succeeds:

```
$ pip install --dry-run .
Processing /home/damian/opensource/support/uv/4536
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Would install foo-0.1.0
```

*IMO* pip is following the spec here and uv is not. Specifically if we take `requires-python` to be a Version as defined by PEP 440, then according to: https://packaging.python.org/en/latest/specifications/version-specifiers/#handling-of-pre-releases then the installed version of Python should already be accepted based on:

> accept already installed pre-releases for all version specifiers

Moreover, intuitively, if a developer splits up their package releases between `python < 3.11` and `python >= 3.11` they should not expect there to be a hole of Python versions.

But let me know if you know some more specific part of the spec that says my interpretation is wrong, I will file a bug against pip. 


---

_Referenced in [astral-sh/uv#4536](../../astral-sh/uv/issues/4536.md) on 2024-07-01 23:14_

---

_Comment by @charliermarsh on 2024-07-01 23:21_

I don't feel strongly about this, I'm fine with effectively omitting pre-releases for Python version handling.

What does "accept already installed pre-releases for all version specifiers" mean? Is that like, if some does `pip install foo>=3.0` and you already have `foo==3.0.0a0` installed, it should be satisfy the requirement?

I maintain that per the spec a pre-release version should not satisfy `python_version < 3.11 or python_version >= 3.11` -- for example, `uv pip compile --python-version 3.11.0a0` would be correct to exclude that dependency in formal terms (I don't think we even allow pre-releases there, just an example) -- but I agree that it's not really sensible.

---

_Comment by @notatallshaw on 2024-07-01 23:29_

> What does "accept already installed pre-releases for all version specifiers" mean? Is that like, if some does `pip install foo>=3.0` and you already have `foo==3.0.0a0` installed, it should be satisfy the requirement?

Yes, that has been my interpretation anyway.

> I maintain that per the spec a pre-release version should not satisfy python_version < 3.11 or python_version >= 3.11

I don't follow your logic. If we were talking about remote packages versions instead of python_versions it would match though.

Let's say package "foo" there was only one remote package version, and it was "foo 3.11.0a0", then it would match `foo < 3.11 or foo >= 3.11`. And in the context of Python versions, during installation, there is only ever one version, so to me at least it seems intuitive it matches. 


---

_Comment by @charliermarsh on 2024-07-01 23:43_

You might be right on that last point. It's sort of hard for me to reason about in a world in which (1) we can install Python versions on-demand to meet `requires-python` and (2) we're trying to resolve for a range of Python versions at once. But I guess the thinking is: when you go to install, that's the only version available, so pre-releases should always be allowed?

Separately, if `requires-python = ">=3.13"` is interpreted as allowing pre-releases, is there no way for package authors to indicate that their package does not support those pre-release versions?


---

_Comment by @notatallshaw on 2024-07-01 23:49_

Dug through the pip codebase, I think the line that ends up allowing Python prereleases is this one (at least in the "new" resolver): https://github.com/pypa/pip/blob/24.1.1/src/pip/_internal/resolution/resolvelib/requirements.py#L207

It looks like it was implemented by https://github.com/pypa/pip/pull/8079, and a side effect of the issue that releases with only prereleases should be installed https://github.com/pypa/pip/issues/8075.

But maybe there is a deeper history to this that I'm unware of.

> You might be right on that last point. It's sort of hard for me to reason about in a world in which (1) we can install Python versions on-demand to meet requires-python and (2) we're trying to resolve for a range of Python versions at once. But I guess the thinking is: when you go to install, that's the only version available, so pre-releases should always be allowed?

That's my thinking anyway, I'm not sure anyone who wrote the spec had your scenario in mind. Is anyone else doing universal resolution other than Poetry? It might be worth sanity checking against what Poetry does.

> Separately, if requires-python = ">=3.13" is interpreted as allowing pre-releases, is there no way for package authors to indicate that their package does not support those pre-release versions?

Well, technically they could do something like:

    requires-python = ">=3.13, !=3.13.0a1, !=3.13.0a2, !=3.13.0a3, !=3.13.0a4, !=3.13.0a5, !=3.13.0a6, !=3.13.0b1, !=3.13.0b2, !=3.13.0b3"

Which I've seen similiar syntax for package versions due to the limited way you can express ranges with holes in them.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-02 01:42_

---

_Comment by @notatallshaw on 2024-07-02 01:58_

I'd be happy to make a Python packaging discussion on this to seek clarification from the broader community if you'd prefer to see if there's consensus on this. 

It's certainly not explicity spelled out in the spec, and I know sometimes the way I read the spec is not in line with the way the spec authors intended it. 

---

_Comment by @charliermarsh on 2024-07-02 02:04_

Na it's alright. We already have this semantics with `--universal` (but not when normalizing the markers -- only when determining package compatibility). I just need to find time to reason through the conversions and make it consistent everywhere.

---

_Label `bug` added by @charliermarsh on 2024-07-02 02:21_

---

_Referenced in [astral-sh/uv#4719](../../astral-sh/uv/issues/4719.md) on 2024-07-02 02:29_

---

_Comment by @charliermarsh on 2024-07-03 17:08_

Still trying to figure out how pip actually allows this given:

```python
>>> from packaging import specifiers
>>> specifiers.SpecifierSet(">=3.13").contains("3.13.0b0", prereleases=True)
False
```

---

_Comment by @notatallshaw on 2024-07-03 17:31_

Ah you're right, the `prereleases=True` there is a red herring. 

The reason pip includes prerelease versions of Python is when it extracts the version from Python it only takes the first 3 elements of the version object: https://github.com/pypa/pip/blob/24.1.1/src/pip/_internal/resolution/resolvelib/candidates.py#L540


```python
>>> sys.version_info
sys.version_info(major=3, minor=13, micro=0, releaselevel='beta', serial=3)
>>> sys.version_info[:3]
(3, 13, 0)
>>> ".".join(str(c) for c in sys.version_info[:3])
'3.13.0'
```



---

_Comment by @notatallshaw on 2024-07-03 17:39_

I appreciate that seems to invalidate most of my reasoning, I will need to run some more checks and think on this.

---

_Comment by @charliermarsh on 2024-07-03 17:40_

That actually makes life a lot easier for us though, if we just want to mimic that...

---

_Comment by @notatallshaw on 2024-07-03 18:00_

True, it does make me think though that you can't do any sort of prerelease exclusion, I will do some more careful testing and report an issue on pip side to see if maintainers have an opinion on that.

Going back to the spec I find the following: https://packaging.python.org/en/latest/specifications/pyproject-toml/#requires-python

Which links to: https://packaging.python.org/en/latest/specifications/core-metadata/#core-metadata-requires-python

It's all rather underspecified, and unfortunately uses the word *may*. Oh well.


---

_Referenced in [pypa/pip#12821](../../pypa/pip/issues/12821.md) on 2024-07-03 20:21_

---

_Referenced in [astral-sh/uv#4794](../../astral-sh/uv/pulls/4794.md) on 2024-07-03 23:50_

---

_Closed by @charliermarsh on 2024-07-04 20:06_

---
