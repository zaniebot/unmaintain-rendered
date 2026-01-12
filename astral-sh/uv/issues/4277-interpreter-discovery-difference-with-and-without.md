```yaml
number: 4277
title: "Interpreter discovery difference with and without `--python` flag"
type: issue
state: closed
author: matterhorn103
labels:
  - question
assignees: []
created_at: 2024-06-12T15:54:50Z
updated_at: 2024-07-18T15:14:39Z
url: https://github.com/astral-sh/uv/issues/4277
synced_at: 2026-01-12T15:58:49Z
```

# Interpreter discovery difference with and without `--python` flag

---

_@matterhorn103_

Sorry for the spree of issue opening and comment writing, I'm just raising things as I find them. I'm very excited about what you're doing with uv :)

uv version: 0.2.11
OS: Linux

A Python interpreter that is found successfully for a given version string passed with the `--python` flag is not found when the same version is mandated by `requires-python` in a `pyproject.toml`.

It is easiest to demonstrate what I mean, I think:
```bash
~/p/foo> ls
foo.py  pyproject.toml
~/p/foo> cat foo.py
print("Hello World!")

~/p/foo> cat pyproject.toml
[project]
name = "foo"
version = "0.0.1"
requires-python = ">= 3.12"

~/p/foo> python3 -V
Python 3.11.9
~/p/foo> python3.12 -V
Python 3.12.3
~/p/foo> which python3
/usr/bin/python3
~/p/foo> which python3.12
/usr/bin/python3.12
~/p/foo> uv run foo.py
warning: `uv run` is experimental and may change without warning.
error: No interpreter found for Python >=3.12 in search path
~/p/foo [2]> uv run -p 3.12 foo.py
warning: `uv run` is experimental and may change without warning.
Using Python 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtualenv at: .venv
Resolved 1 package in 1ms
   Built foo @ file:///home/matt/python/foo
Downloaded 1 package in 651ms
Installed 1 package in 0.33ms
 + foo==0.0.1 (from file:///home/matt/python/foo)
Hello World!
```

---

_Comment by @zanieb on 2024-06-12 15:56_

Interesting. Will investigate..

---

_Comment by @zanieb on 2024-06-12 15:58_

Can you provide verbose logs? `RUST_LOG=uv=trace uv run -v ...`

---

_Comment by @zanieb on 2024-06-12 16:06_

I get something like

```
❯ RUST_LOG=uv=trace uv run -v -- python -V
warning: `uv run` is experimental and may change without warning.
DEBUG Syncing project environment.
DEBUG Found project root: `/Users/zb/workspace/foo`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.12 in search path
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /Users/zb/bin
TRACE Checking `PATH` directory for interpreters: /Users/zb/.local/bin
TRACE Checking `PATH` directory for interpreters: /Users/zb/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /opt/homebrew/bin
TRACE Found possible Python executable: /opt/homebrew/bin/python3
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /opt/homebrew/bin/python3
DEBUG Found CPython 3.12.3 at `/opt/homebrew/bin/python3` (search path)
Using Python 3.12.3 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtualenv at: .venv
...
DEBUG Finished building: foo @ file:///Users/zb/workspace/foo
Downloaded 1 package in 419ms
Installed 1 package in 0.75ms
 + foo==0.0.1 (from file:///Users/zb/workspace/foo)
DEBUG Running `python -V`
Python 3.12.3
```

and the same if I include the `--python` flag.

---

_Comment by @matterhorn103 on 2024-06-12 16:34_

Sure. Without the flag:

```
~/p/foo> RUST_LOG=uv=trace uv run -v -- python -V
warning: `uv run` is experimental and may change without warning.
DEBUG Syncing project environment.
DEBUG Found project root: `/home/matt/python/foo`
DEBUG No workspace root found, using project root
DEBUG Searching for Python >=3.12 in search path
TRACE Searching PATH for executables: python3, python
TRACE Checking `PATH` directory for interpreters: /home/matt/miniforge3/condabin
TRACE Checking `PATH` directory for interpreters: /home/matt/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /home/matt/.local/bin
TRACE Checking `PATH` directory for interpreters: /home/matt/bin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3
TRACE Cached interpreter info for Python 3.11.9, skipping probing: /usr/bin/python3
DEBUG Found CPython 3.11.9 at `/usr/bin/python3` (search path)
TRACE Checking `PATH` directory for interpreters: /bin
TRACE Found possible Python executable: /bin/python3
TRACE Cached interpreter info for Python 3.11.9, skipping probing: /bin/python3
DEBUG Found CPython 3.11.9 at `/bin/python3` (search path)
error: No interpreter found for Python >=3.12 in search path
```

When passing the flag a lot more happens:

```
~/p/foo [2]> RUST_LOG=uv=trace uv run -p 3.12 -v -- python -V
warning: `uv run` is experimental and may change without warning.
DEBUG Syncing project environment.
DEBUG Found project root: `/home/matt/python/foo`
DEBUG No workspace root found, using project root
DEBUG Searching for Python 3.12 in search path
TRACE Searching PATH for executables: python3.12, python3, python
TRACE Checking `PATH` directory for interpreters: /home/matt/miniforge3/condabin
TRACE Checking `PATH` directory for interpreters: /home/matt/.cargo/bin
TRACE Checking `PATH` directory for interpreters: /home/matt/.local/bin
TRACE Checking `PATH` directory for interpreters: /home/matt/bin
TRACE Checking `PATH` directory for interpreters: /usr/local/bin
TRACE Checking `PATH` directory for interpreters: /usr/bin
TRACE Found possible Python executable: /usr/bin/python3.12
TRACE Cached interpreter info for Python 3.12.3, skipping probing: /usr/bin/python3.12
DEBUG Found CPython 3.12.3 at `/usr/bin/python3.12` (search path)
Using Python 3.12.3 interpreter at: /usr/bin/python3.12
Creating virtualenv at: .venv
DEBUG Using request timeout of 30s
TRACE Performing lookahead for foo @ file:///home/matt/python/foo
TRACE Checking lock for `/home/matt/.cache/uv/built-wheels-v3/editable/5472323fa88996ff`
DEBUG Acquired lock for `/home/matt/.cache/uv/built-wheels-v3/editable/5472323fa88996ff`
DEBUG Preparing metadata for: foo @ file:///home/matt/python/foo
DEBUG No static `PKG-INFO` available for: foo @ file:///home/matt/python/foo (MissingPkgInfo)
DEBUG Found static `pyproject.toml` for: foo @ file:///home/matt/python/foo
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.12.3
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///home/matt/python/foo (*)
DEBUG Tried 1 versions: foo 1
Resolved 1 package in 1ms
DEBUG Using request timeout of 30s
DEBUG Identified uncached requirement: foo @ file:///home/matt/python/foo
TRACE Checking lock for `/home/matt/.cache/uv/built-wheels-v3/editable/5472323fa88996ff`
DEBUG Acquired lock for `/home/matt/.cache/uv/built-wheels-v3/editable/5472323fa88996ff`
DEBUG Building: foo @ file:///home/matt/python/foo
INFO Ignoring empty directory
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: setuptools>=40.8.0
TRACE Fetching metadata for setuptools from https://pypi.org/simple/setuptools/
TRACE cached request https://pypi.org/simple/setuptools/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/setuptools/
TRACE Received package metadata for: setuptools
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 558 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "70.0.0" version
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 558 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "70.0.0" version
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/de/88/70c5767a0e43eb4451c2200f07d042a4bcd7639276003a9c54a68cfcc1f8/setuptools-70.0.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/de/88/70c5767a0e43eb4451c2200f07d042a4bcd7639276003a9c54a68cfcc1f8/setuptools-70.0.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: setuptools==70.0.0
DEBUG Tried 1 versions: setuptools 1
DEBUG Installing in setuptools==70.0.0 in /home/matt/.cache/uv/environments-v0/.tmpAUpQQi
DEBUG Requirement already cached: setuptools==70.0.0
DEBUG Installing build requirement: setuptools==70.0.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_editable()`
DEBUG Installing extra requirements for build backend
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Adding direct dependency: wheel*
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 558 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "70.0.0" version
DEBUG Selecting: setuptools==70.0.0 (setuptools-70.0.0-py3-none-any.whl)
TRACE Fetching metadata for wheel from https://pypi.org/simple/wheel/
TRACE selecting candidate for package setuptools with range Range { segments: [(Included("40.8.0"), Unbounded)] } with 558 remote versions
TRACE found candidate for package PackageName("setuptools") with range Range { segments: [(Included("40.8.0"), Unbounded)] } after 1 steps: "70.0.0" version
TRACE cached request https://pypi.org/simple/wheel/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://pypi.org/simple/wheel/ has a cached response that does not allow staleness
TRACE request https://pypi.org/simple/wheel/ does not have a fresh cache because its age is 2642 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.org/simple/wheel/
DEBUG Sending revalidation request for: https://pypi.org/simple/wheel/
TRACE Handling request for https://pypi.org/simple/wheel/
TRACE Request for https://pypi.org/simple/wheel/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/wheel/
TRACE Attempting unauthenticated request for https://pypi.org/simple/wheel/
TRACE not modified because old and new etag values ([34, 69, 99, 78, 88, 101, 54, 48, 107, 117, 120, 81, 66, 99, 113, 76, 98, 104, 77, 106, 54, 56, 65, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/wheel/
TRACE Received package metadata for: wheel
TRACE selecting candidate for package wheel with range Range { segments: [(Unbounded, Unbounded)] } with 75 remote versions
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
DEBUG Searching for a compatible version of wheel (*)
TRACE selecting candidate for package wheel with range Range { segments: [(Unbounded, Unbounded)] } with 75 remote versions
TRACE found candidate for package PackageName("wheel") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "0.43.0" version
DEBUG Selecting: wheel==0.43.0 (wheel-0.43.0-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7d/cd/d7460c9a869b16c3dd4e1e403cce337df165368c71d6af229a74699622ce/wheel-0.43.0-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: wheel==0.43.0
DEBUG Tried 2 versions: setuptools 1, wheel 1
DEBUG Installing in setuptools==70.0.0, wheel==0.43.0 in /home/matt/.cache/uv/environments-v0/.tmpAUpQQi
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("setuptools"), version: "70.0.0", path: "/home/matt/.cache/uv/environments-v0/.tmpAUpQQi/lib/python3.12/site-packages/setuptools-70.0.0.dist-info" }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "70.0.0" }]), index: None }
DEBUG Requirement already installed: setuptools==70.0.0
DEBUG Requirement already cached: wheel==0.43.0
DEBUG Installing build requirement: wheel==0.43.0
DEBUG Calling `setuptools.build_meta:__legacy__.build_editable("/home/matt/.cache/uv/built-wheels-v3/editable/5472323fa88996ff/9ZoalE7xvYkrzT09MLQff/.tmpM7iXTS", {}, None)`
DEBUG Finished building: foo @ file:///home/matt/python/foo
Downloaded 1 package in 648ms
Installed 1 package in 0.24ms
 + foo==0.0.1 (from file:///home/matt/python/foo)
DEBUG Running `python -V`
Python 3.12.3
```

---

_Comment by @zanieb on 2024-06-12 16:54_

Ah so the difference here is that unless you _request_ Python 3.12 we won't look for an interpreter with the `3.12` suffix. The key difference is

```
TRACE Searching PATH for executables: python3.12, python3, python
```

vs

```
TRACE Searching PATH for executables: python3, python
```

If you were to pass the flag `--python '>=3.12'` you'd get the same result. We _could_ consider searching for executables with suffixes for Python range requests (i.e. >=3.12), it just gets kind of complicated since that range is technically unbounded.

---

_Comment by @zanieb on 2024-06-12 18:28_

I was thinking about adding this but am actually unsure where we'd stop. Like should we be checking for `python3.33` on the PATH? Probably not.

---

_Label `question` added by @zanieb on 2024-06-13 01:48_

---

_Comment by @zanieb on 2024-06-27 10:53_

@konstin do you have any ideas here?

---

_Assigned to @konstin by @konstin on 2024-06-27 10:59_

---

_Comment by @matterhorn103 on 2024-06-27 15:04_

I think at minimum the lowest possible matching version should be checked for, as that can be pretty much guaranteed to be a released version. I.e. `--python '>=3.12'`should check `python3.12, python3, python` and `--python '>3.9'` should check `python3.10, python3, python` (I assume using > is possible there but I don't actually know).

When it comes to scanning across many possible version numbers, shouldn't it be possible to come up with a reasonable upper bound based on the current date? The Python release schedule is fairly predictable after all, so in the event of no upper bound being given for the Python version, can't an upper bound be set to `YY - 11 + x`, where `x` is a buffer, something like 3, for safety?

---

_Comment by @konstin on 2024-07-01 09:37_

We need to split `PATH` at the path separator, list all directories and filter them for `python3\.([\d]+)` where the minor number is compatible.

---

_Unassigned @konstin by @konstin on 2024-07-01 09:37_

---

_Comment by @zanieb on 2024-07-01 21:17_

There's `which_re` though a regular expression sounds kind of heavy here it would be a good start.

---

_Comment by @zanieb on 2024-07-01 21:20_

I'm going to close this in favor of https://github.com/astral-sh/uv/issues/4709 which specifies a way we could improve this behavior.

At the moment though, this is working as designed.

---

_Closed by @zanieb on 2024-07-01 21:20_

---

_Comment by @zanieb on 2024-07-18 15:14_

@matterhorn103 this is resolved in #5148

---
