---
number: 13951
title: "`uv lock` empties lockfile when upgrading python"
type: issue
state: closed
author: nrlulz
labels:
  - bug
assignees: []
created_at: 2025-06-10T15:58:07Z
updated_at: 2025-06-26T21:51:05Z
url: https://github.com/astral-sh/uv/issues/13951
synced_at: 2026-01-10T01:25:40Z
---

# `uv lock` empties lockfile when upgrading python

---

_Issue opened by @nrlulz on 2025-06-10 15:58_

### Summary

I noticed this while migrating a project to uv and testing that the pinned version of python can be updated with renovate. If I have project with a dependency which adds some `resolution-markers` to uv.lock, update the pinned version of python, and run `uv lock --upgrade-package python`, all of the `package`s in uv.lock are removed.

See [minimal reproduction repo](https://github.com/nrlulz/renovate-bug-repro)

Steps to reproduce (in above repo):
1. Increment the `requires-python = "==3.11.12"` constraint in pyproject.toml to `"==3.11.13"`
2. Run `uv lock --upgrade-package python` (this is the command run by renovate)
3. Lockfile is emptied

I'm not sure whether this is:
1. A bug in uv?
2. Renovate doing something unsupported (i.e. should I be reporting this to renovate instead)?
3. An issue with the build configuration of this particular opencv package?
4. Me doing it wrong by pinning an exact version of python instead of a range?

Some other notes:
* uv version is 0.7.12
* Reproduces on macos 15.5 (arm64) environment, github actions' "ubuntu-latest" environment, and whatever environment the hosted renovate bot uses
* It only seems to happen if the project has a dependency which causes uv.lock to contain `resolution-markers`
* Bumping requires-python and running `uv lock` without the `--upgrade-package` argument produces the correct results
* Using an inexact constraint like `requires-python = ">=3.11.12,<3.12"` and bumping it to `">3.11.13,<3.12"` also behaves correctly
* It doesn't seem related to the particular version of python. e.g., upgrading from 3.13.3 to 3.13.4 exhibits the same behavior.

Verbose output:

```
$ cat uv.lock
version = 1
revision = 2
requires-python = "==3.11.12"
resolution-markers = [
    "sys_platform == 'darwin'",
    "platform_machine == 'aarch64' and sys_platform == 'linux'",
    "(platform_machine != 'aarch64' and sys_platform == 'linux') or (sys_platform != 'darwin' and sys_platform != 'linux')",
]

[[package]]
name = "numpy"
...
```

```
$ uv run python --version
Python 3.11.12
```

```
$ uv lock --upgrade-package python --verbose
DEBUG uv 0.7.12 (Homebrew 2025-06-06)
DEBUG Found workspace root: `/Users/nparker/Projects/renovate-bug-repro`
DEBUG Adding root workspace member: `/Users/nparker/Projects/renovate-bug-repro`
DEBUG No Python version file found in workspace: /Users/nparker/Projects/renovate-bug-repro
DEBUG Using Python request `==3.11.13` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version does not satisfy the request: `Python ==3.11.13`
DEBUG Searching for Python ==3.11.13 in managed installations or search path
DEBUG Searching for managed installations at `/Users/nparker/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.13.4-macos-aarch64-none`
DEBUG Found managed installation `cpython-3.11.13-macos-aarch64-none`
DEBUG Found `cpython-3.11.13-macos-aarch64-none` at `/Users/nparker/.local/share/uv/python/cpython-3.11.13-macos-aarch64-none/bin/python3.11` (managed installations)
Using CPython 3.11.13
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to `--upgrade-package`
DEBUG Found static `pyproject.toml` for: renovate-bug-repro @ file:///Users/nparker/Projects/renovate-bug-repro
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.11.13
DEBUG Solving with target Python version: ==3.11.13
DEBUG Narrowed `requires-python` bound to: <=3.11.12, >=3.11.13
DEBUG Narrowed `requires-python` bound to: <=3.11.12, >=3.11.13
DEBUG Narrowed `requires-python` bound to: <=3.11.12, >=3.11.13
DEBUG Solving split (python_full_version == '3.11.12' and sys_platform == 'darwin') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: LessThanEqual, version: "3.11.12" }, VersionSpecifier { operator: GreaterThanEqual, version: "3.11.13" }]), range: RequiresPythonRange(LowerBound(Included("3.11.13")), UpperBound(Included("3.11.12"))) })
DEBUG Tried 0 versions: 
DEBUG split `python_full_version == '3.11.12' and sys_platform == 'darwin'` resolution took 0.001s
DEBUG Solving split (python_full_version == '3.11.12' and platform_machine == 'aarch64' and sys_platform == 'linux') (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: LessThanEqual, version: "3.11.12" }, VersionSpecifier { operator: GreaterThanEqual, version: "3.11.13" }]), range: RequiresPythonRange(LowerBound(Included("3.11.13")), UpperBound(Included("3.11.12"))) })
DEBUG Tried 0 versions: 
DEBUG split `python_full_version == '3.11.12' and platform_machine == 'aarch64' and sys_platform == 'linux'` resolution took 0.000s
DEBUG Solving split ((python_full_version == '3.11.12' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.11.12' and sys_platform != 'darwin' and sys_platform != 'linux')) (requires-python: RequiresPython { specifiers: VersionSpecifiers([VersionSpecifier { operator: LessThanEqual, version: "3.11.12" }, VersionSpecifier { operator: GreaterThanEqual, version: "3.11.13" }]), range: RequiresPythonRange(LowerBound(Included("3.11.13")), UpperBound(Included("3.11.12"))) })
DEBUG Tried 0 versions: 
DEBUG split `(python_full_version == '3.11.12' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.11.12' and sys_platform != 'darwin' and sys_platform != 'linux')` resolution took 0.000s
INFO Solved your requirements for 3 environments
DEBUG Distinct solution for split (python_full_version == '3.11.12' and sys_platform == 'darwin') with 0 packages
DEBUG Distinct solution for split (python_full_version == '3.11.12' and platform_machine == 'aarch64' and sys_platform == 'linux') with 0 packages
DEBUG Distinct solution for split ((python_full_version == '3.11.12' and platform_machine != 'aarch64' and sys_platform == 'linux') or (python_full_version == '3.11.12' and sys_platform != 'darwin' and sys_platform != 'linux')) with 0 packages
Resolved in 9ms
Removed numpy v2.3.0
Removed opencv-python-headless v4.11.0.86
Removed renovate-bug-repro v0.1.0
```

```
$ cat uv.lock
version = 1
revision = 2
requires-python = "==3.11.13"
resolution-markers = [
    "python_version < '0'",
]
```

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.7.12 (Homebrew 2025-06-06)

### Python version

Python 3.11.12

---

_Label `bug` added by @nrlulz on 2025-06-10 15:58_

---

_Comment by @zanieb on 2025-06-10 16:01_

Thanks for the report and MRE!

---

_Comment by @zanieb on 2025-06-10 16:14_

Maybe the problem is that we're reading the existing exact Python versions markers from the lockfile?

---

_Comment by @nrlulz on 2025-06-10 16:34_

Not sure. The output from the lock command suggests that it's `` Ignoring existing lockfile due to `--upgrade-package` ``, but it must be, because it doesn't happen if I delete the lockfile and run the same command. Somehow it's getting into an impossible situation trying to solve the constraint `<=3.11.12, >=3.11.13`.

---

_Comment by @zanieb on 2025-06-10 16:41_

We will still use the existing lockfile contents as preferences during resolution. That log is a little misleading, that's just about staleness checks.

---

_Assigned to @BurntSushi by @konstin on 2025-06-11 12:08_

---

_Comment by @BurntSushi on 2025-06-13 19:02_

So I think I see the problem here. In our validation, we are [returning `Preferable`](https://github.com/astral-sh/uv/blob/49b450109b825ec8fdec7600c2a8f763496f70b7/crates/uv/src/commands/project/lock.rs#L986), which in turn says that we should try to re-use the fork markers in the lock file. But the fork markers have a `python_version` pinned to `3.11.12`. So when we go to re-generate the lock file using the new `3.11.13` version for `requires-python`, the process of _simplifying_ the fork markers with respect to `3.11.13` causes them to all become impossible: because `==3.11.12` is disjoint with `==3.11.13`.

I _think_ this _should_ be handled by checking this exact fact with the marker coverage:

https://github.com/astral-sh/uv/blob/49b450109b825ec8fdec7600c2a8f763496f70b7/crates/uv/src/commands/project/lock.rs#L1022-L1033

Specifically, this would return `Versions`, which means we should try to respect locked versions but _not_ the fork markers. Which is what I think we want here.

The problem is that at validation time, `requires-python` corresponds to what I believe is in the lock file. However, when we go to re-generate the lock file, `requires-python` is the new value from `pyproject.toml`. So validation is in effect making a decision based on old information. I think.

I'm not 100% on the fix here. I think perhaps validation should incorporate the _new_ `requires-python` value? If so, then it's just a matter of doing that and moving the check above the `--upgrade-package` checks. Indeed, if `Versions` is returned here, then this problem goes away.

---

_Referenced in [astral-sh/uv#14076](../../astral-sh/uv/pulls/14076.md) on 2025-06-16 14:50_

---

_Closed by @BurntSushi on 2025-06-17 15:50_

---

_Comment by @nrlulz on 2025-06-26 21:51_

@BurntSushi just wanted to say thanks for the quick fix on this! I can confirm the issue is resolved in 0.7.15.

---
