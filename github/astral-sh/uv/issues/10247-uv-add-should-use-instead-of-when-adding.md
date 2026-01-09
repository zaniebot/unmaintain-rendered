---
number: 10247
title: "uv add should use ^ instead of >= when adding dependencies to pyproject.toml"
type: issue
state: closed
author: kishaningithub
labels:
  - duplicate
assignees: []
created_at: 2024-12-31T02:53:06Z
updated_at: 2025-03-15T22:34:37Z
url: https://github.com/astral-sh/uv/issues/10247
synced_at: 2026-01-07T13:12:18-06:00
---

# uv add should use ^ instead of >= when adding dependencies to pyproject.toml

---

_Issue opened by @kishaningithub on 2024-12-31 02:53_

To add / remove dependencies i use the `uv add` and `uv remove` commands and i prefer not to edit `pyproject.toml` manually.

When i do something like `uv add requests` i see the following addition in `pyproject.toml`

```toml
dependencies = [
    "requests>=2.32.3"
]
```

The `>=` looks super scary as it semantically means my project will not break even if requests releases a major version. Yes, i know there is a `uv.lock` file to ensure reproducible builds but i prefer the following when i do `uv add requests`

```toml
dependencies = [
    "requests>=2.32.3,<3"
]
```

I believe poetry on several other semver tools use something like `^2.32.3` which is basically the equivalent of `>=2.32.3,<3`

---

_Comment by @zanieb on 2024-12-31 06:13_

Loosely a duplicate of https://github.com/astral-sh/uv/issues/6783

There's a very strong argument for _not_ including upper bounds when publishing libraries. I think in applications, an upper bound can be nice. There is further discussion linked in the referenced issue.

---

_Label `duplicate` added by @zanieb on 2024-12-31 06:13_

---

_Comment by @kishaningithub on 2024-12-31 11:50_

> There's a very strong argument for not including upper bounds when publishing libraries.

@zanieb Can you point me to this? I am unable to find it

---

_Comment by @charliermarsh on 2024-12-31 14:27_

https://iscinumpy.dev/post/bound-version-constraints/

---

_Comment by @zanieb on 2024-12-31 17:18_

And more, at https://github.com/zanieb/poetry-relax?tab=readme-ov-file#references

---

_Comment by @woutervh on 2025-01-01 15:01_

This caret-notation, made popular by poetry, is one of the most diabolical anti-patterns in the python-world.
Dependency-hell can be a pain. A fake dependency-hell is even worse.

It also conflates the notions of abstract and concrete dependencies.
- Abstract dependencies are the toplevel direct dependencies we specify in pyproject.toml
- concrete dependencies is the list all required packages with a concrete version-number need to create a reproducible build, aka lock-files.

Also a major new release does not mean "guaranteed backward incompatibility".
I've seen deployed projects break on dependencies with new major releases, minor releases, and not even rarely, patch-releases.
To check compatibililty use your own test-suite and don't rely on a vague external semantic version-number.


> The >= looks super scary as it semantically means my project will not break even if requests releases a major version.
 
That is not what it means.
It means that the code is *intended* to be working on newer versions, even if those version don't exist yet.
And if it doesn't work, it will be considered a bug that should be fixed.

But how can you test this new major release, if that packages flat-out refuses to install in your venv? 
uv's capability to override package-depencies has already been a great use for me, poetry does not even have an equivalent.


Specifying minimum-version on the other hand, means that the code likely does to work and there is no intention to fix that.

Try to read it like "intended to support"


If your package uses newer language-constructs introduced in python3.10, then use 

> python = ">3.10"

Only specify an upper bound, it you don't intend to support 3.11.

> python = ">3.10<3.11"


If your package uses python2.7, and you don't the any intention to support python 3, use

> python = ">=2.7,<3"

I never liked this caret notation, as the mathematical meaning is not intuitively clear:
> python = "^2.7"




---

_Comment by @woutervh on 2025-01-01 15:12_

I don't make the distinction between a python-library or a python-application, as long as there is a lock-file.
IMHO the distinction comes from not properly packaging applications.

For me an application is
 - a python-package
 - config-file(s)
 - entry-point to execute/start

In scripts with inline metadata, distributed without a lock-file, the upper bounds make sense.

---

_Comment by @yakMM on 2025-01-01 16:05_

I agree the default lower bound constraint is enough for library.

I also think there is value in adding an option like `--save-exact` to bind the latest version (useful for applications and scripts)

---

_Referenced in [astral-sh/uv#7810](../../astral-sh/uv/issues/7810.md) on 2025-01-03 17:55_

---

_Closed by @zanieb on 2025-01-06 20:34_

---

_Referenced in [astral-sh/uv#6783](../../astral-sh/uv/issues/6783.md) on 2025-01-07 19:51_

---

_Comment by @Ark-kun on 2025-03-03 20:48_

I've seen many many projects around me break for all users when some backwards incompatible major version of a library is released. Pydantic released v2 and thousands of major projects and libraries broke.

When your library only specifies lower bound, it will eventually break for all your users. Their CI/CDs, Dockerfiles etc will be broken.

When people do `pip install some-package==1.2.3` or `uv add some-package==1.2.3` they expect that the package will work, not be a broken mess. Not having upper bounds guarantees the result will be a mess.

When package specifies only lower bound `dependency>=1.0.0`, the package is claiming it is compatible with any future version which is a lie. Package versions with dependency upper bounds can live their whole life and retire without breaking the users. There is a big chance. Packages without upper bound WILL eventually break all their users. There is zero chance.


>It means that the code is intended to be working on newer versions, even if those version don't exist yet.

No. It first and foremost means that the current version is not supported to be working on far away future versions.
It's like product warranty. Lower bound is a "this version will be compatible forever" claim which is most certainly a lie. Upper bound is analogous to "This product version is supported for 1 year" which has much higher probability of not breaking.

---

_Comment by @zanieb on 2025-03-03 20:57_

This has been repeatedly discussed, so I won't go into depth here. While I agree with your perspective, the reality is more complicated.

When `>=` is used, those downstream users can add a constraint to, e.g., Pydantic, when a new version is released to resolve their problem. When `<` is used, there is _no recourse_ for downstream users. The package versions are considered incompatible regardless of runtime compatibility. That's it. The user can't do anything about it (unless you're using uv 🥳 which allows overwriting that incorrect package metadata).

Unfortunately, that's the state of things.

This is why lockfiles and other methods of reproducibility (like `exclude-newer`) are so important for avoiding the "eventually break" case you mention.

---

_Comment by @MatteoCampinoti94 on 2025-03-14 12:14_

> When >= is used, those downstream users can add a constraint

That works if you are distributing a library and the downstream users are programmers who know how to look up and fix those kind of errors, but if you are distributing an application you want it to work with as little user intervention as possible and having only a lower bound _guarantees_ that the application will break eventually. Having an option to pin the version to major (`>=x.y.z,<x+1`), minor (`>=x.y.z,<x.y+1`), or patch (`==x.y.z`) would be extremely useful.

```bash
uv add <package> --pin {major|minor|patch}
```

---

_Comment by @woutervh on 2025-03-15 14:32_

@MatteoCampinoti94  

> if you are distributing an application you want it to work with as little user intervention as possible and having only a lower bound guarantees that the application will break eventually. 


This will solve your use-case?  

```
uv sync --resolution lowest-direct
```

in pyproject.toml:
```
[tool.uv]
resolution = "lowest-direct"
```


---

_Comment by @MatteoCampinoti94 on 2025-03-15 22:34_

@woutervh That does not do anything for distribution, uv's resolution setting only affects the lock file which is _not_ part of the distributed wheel. To make sure those versions are pinned you need to either add the version spec when using `uv add` or modify the pyproject file after adding the dependencies.

The first method is just annoying because you have to know the version beforehand, so you'd have to check the package on pypi then run uv add. The second is not so bad but it is still annoying.

There is no reason for `uv add` not to have a simple way to pin the version to the current major number or minor, because that is good practice when distributing applications.

---
