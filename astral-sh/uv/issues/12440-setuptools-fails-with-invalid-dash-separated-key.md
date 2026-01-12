```yaml
number: 12440
title: "`setuptools` fails with \"Invalid dash-separated key\" or \"Invalid uppercase key\""
type: issue
state: closed
author: zanieb
labels:
  - external
assignees: []
created_at: 2025-03-24T16:48:43Z
updated_at: 2025-03-25T01:50:22Z
url: https://github.com/astral-sh/uv/issues/12440
synced_at: 2026-01-12T16:01:02Z
```

# `setuptools` fails with "Invalid dash-separated key" or "Invalid uppercase key"

---

_@zanieb_

`setuptools` [v78](https://setuptools.pypa.io/en/stable/history.html#v78-0-0) no longer accepts options containing uppercase or dash characters in `setup.cfg`. This means if your package uses this feature, or if any of your dependencies need to be built from source (and are using the invalid syntax) then an error will be raised.

This can be worked around in the uv CLI by excluding new versions:

```
$ uv sync --exclude-newer 2025-03-24
```

This fix can be persisted in the `pyproject.toml` as:

```toml
[tool.uv]
exclude-newer = "2025-03-24T00:00:00Z"
```

Note this will prevent upgrading other packages.

If you are using `uv pip`, you can set a build constraint instead:

```
echo "setuptools<78" | uv pip install -b - <package>
```

This fix can be persisted in the `pyproject.toml` as:

```toml
[tool.uv]
build-constraint-dependencies = ["setuptools<78"]
```

There is an unrelated uv bug that does not propagate `build-constraint-dependencies` in `uv sync` (https://github.com/astral-sh/uv/issues/12441). We will be releasing a fix for that soon.

I've asked for the change to be reverted upstream at https://github.com/pypa/setuptools/pull/4870#issuecomment-2748731521

We're also adding a dedicated warning for this error case in https://github.com/astral-sh/uv/pull/12438

See also

- https://github.com/astral-sh/uv/issues/12434
- #12437 


---

_Label `external` added by @zanieb on 2025-03-24 17:03_

---

_Comment by @systemsoverload on 2025-03-24 17:04_

Is there a reasonable alternative to `uv sync` that will allow for installing `[dependency-groups]` ? Looks like `uv venv && uv pip install .` is a rough approximation to unblock the base deps but I cant figure out how to get the dev groups installed.

---

_Comment by @zanieb on 2025-03-24 17:06_

`uv pip install --group <name>` should work for that.

---

_Comment by @systemsoverload on 2025-03-24 17:08_

```
✗ uv pip install --group dev
error: unexpected argument '--group' found

  tip: to pass '--group' as a value, use '-- --group'

Usage: uv pip install [OPTIONS] <PACKAGE|--requirements <REQUIREMENTS>|--editable <EDITABLE>>

For more information, try '--help'.
```


Unfortunately it seems `uv pip` is unaware of groups?

---

_Comment by @toby-j on 2025-03-24 17:11_

```
[tool.uv]
exclude-newer = "2025-03-24T00:00:00Z"
```

Worked for me although no joy with `build-constraint-dependencies`. 

Thanks for the support @zanieb 

---

_Comment by @systemsoverload on 2025-03-24 17:15_

Unfortunately `exclude-newer` will refuse to install packages that have no upload date specified, which of course, happens to include popular public packages 🤦 

```
warning: pytest_django-4.10.0.tar.gz is missing an upload date, but user provided: 2025-03-24T20:00:00Z
warning: responses-0.17.0.tar.gz is missing an upload date, but user provided: 2025-03-24T20:00:00Z
```

---

_Comment by @rayanramoul on 2025-03-24 17:16_

> Unfortunately `exclude-newer` will refuse to install packages that have no upload date specified, which of course, happens to include popular public packages 🤦
> 
> ```
> warning: pytest_django-4.10.0.tar.gz is missing an upload date, but user provided: 2025-03-24T20:00:00Z
> warning: responses-0.17.0.tar.gz is missing an upload date, but user provided: 2025-03-24T20:00:00Z
> ```

Yes, getting the same one for torch==2.3.0
```
warning: torch-2.3.0+cu118-cp310-cp310-linux_x86_64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp310-cp310-win_amd64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp311-cp311-linux_x86_64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp311-cp311-win_amd64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp312-cp312-linux_x86_64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp312-cp312-win_amd64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp38-cp38-linux_x86_64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp38-cp38-win_amd64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp39-cp39-linux_x86_64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
warning: torch-2.3.0+cu118-cp39-cp39-win_amd64.whl is missing an upload date, but user provided: 2025-03-24T00:00:00Z
  x No solution found when resolving dependencies:
  `-> Because there is no version of torch==2.3.0 ....
```

---

_Comment by @notatallshaw on 2025-03-24 17:19_

> Unfortunately `exclude-newer` will refuse to install packages that have no upload date specified, which of course, happens to include popular public packages 🤦

Are you getting those of PyPI or some other index? In general PyPI should have the upload date of every package and if not we should report a bug to them.

However it is known that torch, and many other indexes, don't yet support upload date.

---

_Comment by @systemsoverload on 2025-03-24 17:22_

Good point. We are using Artifactory as an internal mirror, and given that they only implemented the `/simple` API, Im guessing upload date is going to be missing for every package we've cached there. 

---

_Comment by @systemsoverload on 2025-03-24 17:26_

As noted in  #12441, this is a reasonable approximation that will follow `build-constraint-dependencies` and `dependency-groups`. Just creating an alias `uv-sync` for now that does this:

``` 
uv export --all-groups > requirements.txt
uv venv
uv pip install -r requirements.txt
```

---

_Comment by @charliermarsh on 2025-03-24 18:56_

`uv pip --group` shipped in v0.6.7: https://github.com/astral-sh/uv/releases/tag/0.6.7

---

_Comment by @anrooo on 2025-03-24 19:03_

Also hitting the limitation with `exclude-newer`. It would be great to have a flag to ignore this constraint for packages that don't publish version data. 

---

_Comment by @inoa-jboliveira on 2025-03-24 20:09_

Hi everyone, 20 minutes ago setuptools added a new package that postpones this change:

https://setuptools.pypa.io/en/stable/history.html


Anyway, it would be better if uv used the same setuptools in the env when installed

---

_Comment by @notatallshaw on 2025-03-24 20:14_

> Anyway, it would be better if uv used the same setuptools in the env when installed

It has been a Python packaging standard to seperate out build dependencies from runtime dependencies for years now, generally it has solved a lot more problems than it has created, all modern packaging tools I am aware of support it. 

It was defined in https://peps.python.org/pep-0517/ and discussion about problems it has and solutions to those problems should be discussed over at https://discuss.python.org/c/packaging/14

---

_Comment by @zanieb on 2025-03-24 20:14_

@inoa-jboliveira we won't use the same setuptools as installed in the environment as that breaks PEP 517 build isolation.

---

_Comment by @zanieb on 2025-03-24 20:14_

See https://github.com/pypa/setuptools/pull/4911 for the setuptools revert.

---

_Closed by @zanieb on 2025-03-25 01:50_

---
