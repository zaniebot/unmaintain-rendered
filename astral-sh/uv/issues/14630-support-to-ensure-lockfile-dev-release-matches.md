```yaml
number: 14630
title: "support to ensure lockfile dev release matches pyproject.toml's"
type: issue
state: closed
author: thehesiod
labels:
  - enhancement
  - needs-mre
assignees: []
created_at: 2025-07-15T17:25:38Z
updated_at: 2025-07-16T16:14:16Z
url: https://github.com/astral-sh/uv/issues/14630
synced_at: 2026-01-12T16:01:53Z
```

# support to ensure lockfile dev release matches pyproject.toml's

---

_@thehesiod_

**Summary**

When developing with `uv`, we often begin by depending on a **dev release** of a library (e.g. `~=1.0.0.dev0`). Once the changes are approved and that dev version is promoted to a stable release (e.g. `1.0.0`), we update the constraint in `pyproject.toml` to `~=1.0` and run `uv lock`.

However, the lockfile continues to contain the previously locked dev version **unless we explicitly pass `-P`**, even though it no longer matches the new constraint.

#### Expected Behavior

`uv lock` should detect that the current dev version no longer satisfies the new constraint and automatically resolve to the latest matching stable version (`1.0.0`), without requiring `-P foo`.

---

#### Possible Improvements

- Automatically **replace prerelease/dev versions** in the lockfile when they no longer satisfy the updated constraint.
- Emit a **warning** when a prerelease version is locked but no prerelease is permitted by `pyproject.toml`.
- Support a **pre-release hook** that checks for this situation and can enforce clean production locks.

---

#### Why It Matters

This workflow — developing against a dev version, then locking to a stable release — is common in CI/CD pipelines and collaborative team workflows.

Without automatic resolution or feedback, it's easy to accidentally retain prerelease dependencies in the lockfile even after bumping the constraint to a stable track.

---


### Example


#### Example

1. Initial state:

```toml
# pyproject.toml
dependencies = ["foo ~=1.0.0.dev0"]
```

```bash
uv lock
```

Lockfile result:

```
foo==1.0.0.dev0
```

2. Later, we update to:

```toml
# pyproject.toml
dependencies = ["foo ~=1.0"]
```

But after running:

```bash
uv lock
```

The lockfile still incorrectly contains:

```
foo==1.0.0.dev0
```

even though the latest valid version is `1.0.0`.

---

_Label `enhancement` added by @thehesiod on 2025-07-15 17:25_

---

_Comment by @charliermarsh on 2025-07-15 19:10_

I can't reproduce this in my initial testing. Can you provide a complete minimal reproduction that we can use to help you?

---

_Label `needs-mre` added by @charliermarsh on 2025-07-15 19:10_

---

_Comment by @thehesiod on 2025-07-15 22:30_

[uv-bug.zip](https://github.com/user-attachments/files/21244223/uv-bug.zip)

here you go, included is script to run local pypiserver with u/p: admin:admin.

Steps:
1. build + publish `module` (0.0.1.dev0) (uv publish -v --index local)
2. go to module-user and lock (locks to 0.0.1.dev0)
3. go to module and bump to 0.0.1, build + publish
4. go back to module-user, change required version to ~=0.1, re-lock

Result:
module-user uv.lock stays on 0.0.1.dev0 even though 0.0.1 is available and pyproject.toml doesn't list dev version

Expected:
should warn/re-lock to latest production release

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-15 23:53_

---

_Comment by @charliermarsh on 2025-07-16 00:07_

Thanks, I'll give it a try!

---

_Comment by @charliermarsh on 2025-07-16 00:48_

Hmm, I ran this, but I see the expected upgrade:
```
DEBUG uv 0.7.21+2 (9871bbdc7 2025-07-14)
DEBUG Project is contained in non-workspace project: `/Users/crmarsh/workspace/uv`
DEBUG Found workspace root: `/Users/crmarsh/workspace/uv/uv-bug/module-user`
DEBUG Adding root workspace member: `/Users/crmarsh/workspace/uv/uv-bug/module-user`
DEBUG No Python version file found in workspace: /Users/crmarsh/workspace/uv/uv-bug/module-user
DEBUG Using Python request `>=3.8` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python >=3.8 in managed installations or search path
DEBUG Searching for managed installations at `/Users/crmarsh/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.14.0b3-macos-aarch64-none`
DEBUG Found `cpython-3.14.0b3-macos-aarch64-none` at `/Users/crmarsh/.local/share/uv/python/cpython-3.14.0b3-macos-aarch64-none/bin/python3.14` (managed installations)
DEBUG Skipping pre-release cpython-3.14.0b3-macos-aarch64-none
DEBUG Found managed installation `cpython-3.13.2-macos-aarch64-none`
DEBUG Found `cpython-3.13.2-macos-aarch64-none` at `/Users/crmarsh/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13` (managed installations)
Using CPython 3.13.2
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: hello-world-user @ file:///Users/crmarsh/workspace/uv/uv-bug/module-user
DEBUG Project is contained in non-workspace project: `/Users/crmarsh/workspace/uv`
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `hello-world-user==0.0.1`
  Requested: {Requirement { name: PackageName("hello-world"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: TildeEqual, version: "0.0.1" }]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("hello-world"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: TildeEqual, version: "0.0.1.dev0" }]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.13.2
DEBUG Solving with target Python version: >=3.8
DEBUG Adding direct dependency: hello-world-user*
DEBUG Searching for a compatible version of hello-world-user @ file:///Users/crmarsh/workspace/uv/uv-bug/module-user (*)
DEBUG Adding direct dependency: hello-world>=0.0.1, <0.1.dev0
DEBUG Found stale response for: http://localhost:8080/simple/hello-world/
DEBUG Sending revalidation request for: http://localhost:8080/simple/hello-world/
DEBUG Found modified response for: http://localhost:8080/simple/hello-world/
DEBUG Searching for a compatible version of hello-world (>=0.0.1, <0.1.dev0)
DEBUG Selecting: hello-world==0.0.1 [compatible] (hello_world-0.0.1-py3-none-any.whl)
DEBUG No cache entry for: http://localhost:8080/api/package/hello-world/hello_world-0.0.1-py3-none-any.whl
WARN Range requests not supported for hello_world-0.0.1-py3-none-any.whl; streaming wheel
DEBUG No cache entry for: http://localhost:8080/api/package/hello-world/hello_world-0.0.1-py3-none-any.whl
DEBUG Tried 2 versions: hello-world 1, hello-world-user 1
DEBUG all marker environments resolution took 0.024s
Resolved 2 packages in 32ms
Updated hello-world v0.0.1.dev0 -> v0.0.1
```

The only thing to note is I ran `cargo lock -v` (so I ran from `main`). Are you on a recent uv version?

---

_Comment by @charliermarsh on 2025-07-16 00:49_

The only thing I can think of is that we could be caching the result from your index server (if it sends `Cache-Control` headers). Do you see the same thing if you pass `--no-cache` or `--refresh`?

---

_Comment by @thehesiod on 2025-07-16 03:37_

let me get the verbose output for you 

---

_Comment by @thehesiod on 2025-07-16 03:45_

so first run pypicloud:
```bash
~/uv-bug $ ./run-pypi-cloud.sh 
```

and verifying no uv env vars set:
```bash
~/uv-bug/module $ ( set -o posix ; set ) | grep '^UV_'
~/uv-bug/module $ 
```

now publish dev version of module:
```bash
~/uv-bug/module $ uv publish --index local
Publishing 2 files http://localhost:8080/
Enter username ('__token__' if using a token): admin
Enter password: 
Uploading hello_world-0.0.1.dev0-py3-none-any.whl (1.1KiB)
Uploading hello_world-0.0.1.dev0.tar.gz (912.0B)
```

now lock module-user to dev version:
```bash
~/uv-bug/module-user $ uv lock 
Using CPython 3.11.11
Resolved 2 packages in 39ms
~/uv-bug/module-user $ cat uv.lock 
version = 1
revision = 2
requires-python = ">=3.8"

[[package]]
name = "hello-world"
version = "0.0.1.dev0"
source = { registry = "http://localhost:8080/simple" }
sdist = { url = "http://localhost:8080/api/package/hello-world/hello_world-0.0.1.dev0.tar.gz", hash = "sha256:e0461d2be2773b59711e02ecc40387d943d7fdd5558f9bc02a0ea5d238deb44a" }
wheels = [
    { url = "http://localhost:8080/api/package/hello-world/hello_world-0.0.1.dev0-py3-none-any.whl", hash = "sha256:ec5ed452cf94ad94528be03a01b7295d5efe6742d5ea605eb86a96d85234e606" },
]

[[package]]
name = "hello-world-user"
version = "0.0.1"
source = { editable = "." }
dependencies = [
    { name = "hello-world" },
]

[package.metadata]
requires-dist = [{ name = "hello-world", specifier = "~=0.0.1.dev0" }]
```

now bump and publish non-dev version of module:
```bash
$ uv publish --index local
Publishing 2 files http://localhost:8080/
Enter username ('__token__' if using a token): admin
Enter password: 
Uploading hello_world-0.0.1-py3-none-any.whl (1.0KiB)
Uploading hello_world-0.0.1.tar.gz (904.0B)
```

now re-lock to non-dev version
```bash
~/uv-bug/module-user $ cat pyproject.toml 
[project]
name = "hello-world-user"
version = "0.0.1"
description = "module that uses hello-world"
readme = "README.md"
requires-python = ">=3.8"
dependencies = [
    "hello-world~=0.0.1.dev0"
]

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[[tool.uv.index]]
name = "local"
url = "http://localhost:8080/simple"
default = true

~/uv-bug/module-user $ uv lock -v
DEBUG uv 0.7.15 (Homebrew 2025-06-25)
DEBUG Found workspace root: `/Users/alexmohr/uv-bug/module-user`
DEBUG Adding root workspace member: `/Users/alexmohr/uv-bug/module-user`
DEBUG No Python version file found in workspace: /Users/alexmohr/uv-bug/module-user
DEBUG Using Python request `>=3.8` from `requires-python` metadata
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python >=3.8 in managed installations or search path
DEBUG Searching for managed installations at `/Users/alexmohr/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.11-macos-aarch64-none`
DEBUG Found `cpython-3.11.11-macos-aarch64-none` at `/Users/alexmohr/.local/share/uv/python/cpython-3.11.11-macos-aarch64-none/bin/python3.11` (managed installations)
Using CPython 3.11.11
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: hello-world-user @ file:///Users/alexmohr/uv-bug/module-user
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 2 packages in 4ms
```

definitely an issue, and I don't think related to my environment

---

_Comment by @thehesiod on 2025-07-16 03:47_

appreciate for looking into this so quickly btw!  This is a very common workflow for us so I'd like a way to protect our main branch from accidental dev version check-ins.

---

_Comment by @thehesiod on 2025-07-16 03:47_

and btw verified uv.lock remains unchanged with .dev version in there

---

_Comment by @charliermarsh on 2025-07-16 13:03_

In the example above, you didn't modify the bounds in `pyproject.toml`. After "now re-lock to non-dev version", you still have `"hello-world~=0.0.1.dev0"`. By default, we avoid modifying locked versions -- i.e., we prefer versions that are already present in the lockfile. You would need to specify `--upgrade-package hello-world` to force the version to change, _or_ update the bound to `"hello-world~=0.0.1"`.

---

_Comment by @thehesiod on 2025-07-16 15:54_

I did, note in the pyproject.toml I cat'd above, the version is now set to `0.0.1`:
```
[project]
name = "hello-world-user"
version = "0.0.1"
```

this is now bound to a non-dev version, the version in uv.lock no longer matches the requirements in the pyproject.toml.  A dev version never matches a top level non-dev version requirement.

IOW given the pyproject.toml with the non-dev version, there is no situation where locking it without the uv.lock present would ever pick a dev version.  This is a pure breach in requirements.

---

_Comment by @charliermarsh on 2025-07-16 16:00_

I'm really confused. I must be misunderstanding. I'm referring to this:

<img width="700" height="786" alt="Image" src="https://github.com/user-attachments/assets/8f73db18-5f45-4c3f-8f35-5a9e9b1e127f" />

You still have `"hello-world~=0.0.1.dev0"` in the requirements, so choosing `0.0.1.dev0` is still a valid choice. (It's not the version you would get if you removed `uv.lock`, but it _is_ a valid version given the specifier, and we prefer retaining the existing versions.)

---

_Comment by @thehesiod on 2025-07-16 16:04_

oh, sorry 🤦🏼 let me re-do the scenario lol. I know it will occur just updated the wrong version

---

_Comment by @thehesiod on 2025-07-16 16:13_

wow it worked, ok, so it must be more complicated than I thought or perhaps based on secondary requirements?  I'll dig some more

---

_Comment by @thehesiod on 2025-07-16 16:14_

I'll close this for now since I can't reproduce what I was previously seeing in our project.  Thanks for taking the time to look into this!  sorry for wasting your time :(

---

_Closed by @thehesiod on 2025-07-16 16:14_

---
