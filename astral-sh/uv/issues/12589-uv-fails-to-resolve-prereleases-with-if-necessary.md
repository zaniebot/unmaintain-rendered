---
number: 12589
title: "uv fails to resolve prereleases with `if-necessary` flag when generating a new lock file"
type: issue
state: open
author: bryanterichardson
labels:
  - question
  - needs-mre
assignees: []
created_at: 2025-03-31T16:49:23Z
updated_at: 2025-07-15T23:23:49Z
url: https://github.com/astral-sh/uv/issues/12589
synced_at: 2026-01-10T01:25:21Z
---

# uv fails to resolve prereleases with `if-necessary` flag when generating a new lock file

---

_Issue opened by @bryanterichardson on 2025-03-31 16:49_

### Summary

I have a `pyproject.toml` that looks something like:
```
[project]
dependencies = [
    ...
    "some-dependency==0.0.1.dev3",
    ...
]

...

[tool.uv]
prerelease = "if-necessary"
```

If I run `uv lock` I run into an error:
```
uv lock --prerelease if-necessary
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of some-dependency==0.0.1.dev3 and your project depends on some-dependency==0.0.1.dev3, we can conclude that your project's
      requirements are unsatisfiable.
```

However, if I change the config from `if-necessary` to `if-necessary-or-explicit` or `allow`, uv is able to successfully resolve the dependency tree. If I then swap it back to `if-necessary`, it works... sometimes.


### Platform

Darwin 24.2.0 arm64

### Version

0.6.11

### Python version

Python 3.11.3

---

_Label `bug` added by @bryanterichardson on 2025-03-31 16:49_

---

_Comment by @notatallshaw on 2025-03-31 17:21_

I've mentioned this before, `if-necessary` is badly named: https://github.com/astral-sh/uv/issues/9395#issuecomment-2498355868


---

_Comment by @charliermarsh on 2025-04-01 12:57_

I'm a bit confused -- the error message here says that the package doesn't exist at that version? It shouldn't have anything to do with the `--prerelease` strategy.

---

_Label `bug` removed by @charliermarsh on 2025-04-01 12:57_

---

_Label `needs-mre` added by @charliermarsh on 2025-04-01 12:57_

---

_Comment by @charliermarsh on 2025-04-01 12:57_

Can you come up with a reproduction that we can use to help you?

---

_Comment by @bryanterichardson on 2025-04-01 15:25_

Sure. I’ll explain further but will find some public, prerelease to use in an example later when I’m by my computer. For context, the package does exist with that exact version.

When we use the other prerelease strategies, uv is able to completely resolve the dependencies. The behavior we were expecting was that uv would pull this specified prerelease as it is necessary (implied by the name), but later calls to `uv lock -upgrade` would prefer stable releases (within the specified bounds). The problem with the other release strategies is that the package will continue to upgrade to prereleases which is not appropriate for what we need.

After reading the thread above and more of the uv docs, it seems this might be intended (though really confusing naming). What’s even more confusing is that the `if-necessary` strategy will let me include a prerelease in the specified bounds so long as it does not actually resolve to a prerelease (eg the bounds allow for a later, stable release).

---

_Comment by @bryanterichardson on 2025-04-01 16:11_

pyproject.toml
```
[project]
dependencies = [
    "prefect>=3.0.0rc1,<3.3.0",
]
name = "test"
requires-python = ">=3.11"
version = "0.0.1"
```

Run:
```
uv lock --prerelease if-necessary-or-explicit
```

uv.lock
```
...
[[package]]
name = "prefect"
version = "3.2.16.dev1"
source = { registry = "https://pypi.org/simple" }
...
```

Run:
```
uv lock --prerelease if-necessary            
Using CPython 3.11.3 interpreter at: /Users/.../.pyenv/versions/3.11.3/bin/python3
Ignoring existing lockfile due to change in pre-release mode: `if-necessary-or-explicit` vs. `if-necessary`
Resolved 98 packages in 1.25s
Updated prefect v3.2.16.dev1 -> v3.2.15
```

Swapping to `if-necessary` yields my desired behavior as I may have wanted a prerelease at some point but when handling upgrades and security patches I'd much rather land on a stable version moving forward. However, there comes a need to depend on the latest prerelease until a stable version is available. Something like

```
[project]
dependencies = [
    "prefect==3.2.16.dev1",
]
name = "test"
requires-python = ">=3.11"
version = "0.0.1"
```

Or, more likely:
```
[project]
dependencies = [
    "prefect>=3.2.16.dev1,<4",
]
name = "test"
requires-python = ">=3.11"
version = "0.0.1"
```
Note: As of today, this example works because prefect has released stable versions after the specified prerelease

It is at the very least confusing the `if-necessary` fails to resolve a **necessary** dependency that is available.

```
uv lock --prerelease if-necessary
Using CPython 3.11.3 interpreter at: /Users/.../.pyenv/versions/3.11.3/bin/python3
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of prefect==3.2.16.dev1 and your project depends on prefect==3.2.16.dev1, we can conclude that your project's requirements are unsatisfiable.

      hint: `prefect` was requested with a pre-release marker (e.g., prefect==3.2.16.dev1), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

---

_Comment by @charliermarsh on 2025-04-01 16:13_

Oh sorry, `if-necessary` only kicks in if a package has _only_ shipped pre-releases. That's what the "necessary" means.

---

_Comment by @bryanterichardson on 2025-04-01 17:40_

Understood. TY for getting back to me as well.

To that I'd share the same sentiment as @notatallshaw that the naming is a bit confusing as the dependency is **necessary** and might be the only one able to satisfy the bounds. Maybe I am instead requesting a new feature?

---

_Label `question` added by @zanieb on 2025-04-01 19:55_

---

_Referenced in [astral-sh/uv#13819](../../astral-sh/uv/issues/13819.md) on 2025-06-03 14:37_

---

_Comment by @zanieb on 2025-07-15 22:11_

@Butanium want to open a new issue with details as described in https://github.com/astral-sh/uv/issues/9452 so we can help you?

---

_Referenced in [astral-sh/uv#14639](../../astral-sh/uv/issues/14639.md) on 2025-07-15 23:24_

---
