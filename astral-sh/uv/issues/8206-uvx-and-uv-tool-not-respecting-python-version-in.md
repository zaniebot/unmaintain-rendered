```yaml
number: 8206
title: uvx and uv tool not respecting python version in packaged app
type: issue
state: open
author: my1e5
labels:
  - needs-design
assignees: []
created_at: 2024-10-15T09:48:42Z
updated_at: 2025-12-02T10:01:12Z
url: https://github.com/astral-sh/uv/issues/8206
synced_at: 2026-01-12T15:59:22Z
```

# uvx and uv tool not respecting python version in packaged app

---

_@my1e5_

Now that Python 3.13 has been released I'm seeing this behaviour and wondered if I was doing something wrong. 

Say I want to create a [packaged application](https://docs.astral.sh/uv/concepts/projects/#packaged-applications) that only runs on Python 3.12.

```py
uv init --app --package example-packaged-app --python 3.12
```
This creates a `.python-version` pinned at `3.12`. I also go into the `pyproject.toml` file and change `requires-python` to
```diff
- requires-python = ">=3.12"
+ requires-python = "~=3.12"
```
When I run the app within the working directory it uses a suitable version of 3.12
```
$ uv run example-packaged-app
Using CPython 3.12.4 interpreter at: /opt/homebrew/opt/python@3.12/bin/python3.12
Creating virtual environment at: .venv
   Built example-packaged-app @ file:///Users/User/example-packaged-app
Installed 1 package in 2ms
Hello from example-packaged-app!
```
I will also add a PyPi project which currently does not have 3.13 support.
```
uv add dearpygui
Resolved 2 packages in 2ms
   Built example-packaged-app @ file:///Users/User/example-packaged-app
Prepared 1 package in 379ms
Uninstalled 1 package in 0.87ms
Installed 2 packages in 4ms
 + dearpygui==1.11.1
 ~ example-packaged-app==0.1.0 (from file:///Users/User/example-packaged-app)
```
But my problem is when I try to run the app with uvx (or if I try to install the app as a uv tool)
```
uvx -v --from file:///Users/User/example-packaged-app example-packaged-app
DEBUG uv 0.4.21
DEBUG Searching for default Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/rd/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.13.0-macos-aarch64-none`
DEBUG Found `cpython-3.13.0-macos-aarch64-none` at `/Users/rd/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for /Users/User/example-packaged-app in `pyproject.toml` (example-packaged-app)
DEBUG Acquired lock for `/Users/rd/.local/share/uv/tools`
DEBUG Released lock at `/Users/rd/.local/share/uv/tools/.lock`
DEBUG Caching via interpreter: `/Users/rd/.local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: example-packaged-app @ file:///Users/User/example-packaged-app
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: example-packaged-app*
DEBUG Searching for a compatible version of example-packaged-app @ file:///Users/User/example-packaged-app (*)
DEBUG Adding transitive dependency for example-packaged-app==0.1.0: dearpygui>=1.11.1
DEBUG Found fresh response for: https://pypi.org/simple/dearpygui/
DEBUG Searching for a compatible version of dearpygui (>=1.11.1)
DEBUG Searching for a compatible version of dearpygui (>1.11.1)
DEBUG No compatible version found for: dearpygui
DEBUG Searching for a compatible version of example-packaged-app @ file:///Users/User/example-packaged-app (<0.1.0 | >0.1.0)
DEBUG No compatible version found for: example-packaged-app
  × No solution found when resolving tool dependencies:
  ╰─▶ Because only dearpygui<=1.11.1 is available and dearpygui==1.11.1 has no wheels with a matching Python ABI tag, we can conclude that dearpygui>=1.11.1 cannot be used.
      And because example-packaged-app==0.1.0 depends on dearpygui>=1.11.1, we can conclude that example-packaged-app==0.1.0 cannot be used.
      And because only example-packaged-app==0.1.0 is available and you require example-packaged-app, we can conclude that your requirements are unsatisfiable.

      hint: Pre-releases are available for dearpygui in the requested range (e.g., 2.0.0b1), but pre-releases weren't enabled (try: `--prerelease=allow`)
```
How can I get uvx to respect the requirement that Python 3.12 must be used? My use case is I have this packaged app hosted on a local private git server and I want my colleagues to be able to download/install it as a tool. 

My assumption was that uv would look at the `.python-version` file first and if that wasn't present then it would look at the `requires-python` field? But it seems to be going straight to 3.13.0 without checking? 


---

_Comment by @my1e5 on 2024-10-15 10:19_

Ok, I see now that I can pass `--python 3.12` into the `uvx` command. And this does solve my issue. But it doesn't seem fully satisfactory. 

If you are installing a packaged app from a local folder or git repo, you'd want it to just work. For example,
```
$ uv tool install file:///Users/User/example-packaged-app
error: Because only dearpygui<=1.11.1 is available and dearpygui==1.11.1 has no wheels with a matching Python ABI tag, we can conclude that dearpygui>=1.11.1 cannot be used.
And because example-packaged-app==0.1.0 depends on dearpygui>=1.11.1, we can conclude that example-packaged-app==0.1.0 cannot be used.
And because only example-packaged-app==0.1.0 is available and you require example-packaged-app, we can conclude that your requirements are unsatisfiable.
```
To an uninformed end user this error can be confusing. How do you know you need to specify `--python 3.12`?

---

_Comment by @my1e5 on 2024-10-16 10:23_

A simpler MRE. Create a packaged app with `.python-version` pinned to 3.11 (and 3.11 is downloaded already on the system). But uvx is using 3.13.0.

```
$ uv python list
cpython-3.13.0-windows-x86_64-none     C:\Users\User\AppData\Roaming\uv\data\python\cpython-3.13.0-windows-x86_64-none\python.exe
cpython-3.12.7-windows-x86_64-none     <download available>
cpython-3.12.4-windows-x86_64-none     C:\Users\User\AppData\Roaming\uv\data\python\cpython-3.12.4-windows-x86_64-none\python.exe
cpython-3.11.10-windows-x86_64-none    <download available>
cpython-3.11.4-windows-x86_64-none     C:\Users\User\AppData\Local\Programs\Python\Python311\python.exe
cpython-3.10.15-windows-x86_64-none    <download available>
cpython-3.10.0-windows-x86_64-none     C:\Users\User\AppData\Local\Programs\Python\Python310\python.exe
cpython-3.9.20-windows-x86_64-none     <download available>
cpython-3.9.7-windows-x86_64-none      C:\Users\User\AppData\Local\Programs\Python\Python39\python.exe
cpython-3.8.20-windows-x86_64-none     <download available>
cpython-3.8.12-windows-x86_64-none     C:\Users\User\AppData\Local\Programs\Python\Python38\python.exe
cpython-3.7.9-windows-x86_64-none      <download available>
cpython-3.7.5-windows-x86_64-none      C:\Program Files\Python37\python.exe
pypy-3.10.14-windows-x86_64-none       <download available>
pypy-3.9.19-windows-x86_64-none        <download available>
pypy-3.8.16-windows-x86_64-none        <download available>
pypy-3.7.13-windows-x86_64-none        <download available>

$ uv init --app --package foo --python 3.11
Initialized project `foo` at `C:\Users\User\Projects\Test\foo`

$ uvx -v --from foo/ foo
DEBUG uv 0.4.22 (34be3af84 2024-10-15)
DEBUG Searching for default Python interpreter in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `C:\Users\User\AppData\Roaming\uv\data\python`
DEBUG Found managed installation `cpython-3.13.0-windows-x86_64-none`
DEBUG Found `cpython-3.13.0-windows-x86_64-none` at `C:\Users\User\AppData\Roaming\uv\data\python\cpython-3.13.0-windows-x86_64-none\python.exe` (managed installations)
DEBUG Using request timeout of 30s
DEBUG Found PEP 621 metadata for C:\Users\User\Projects\Test\foo in `pyproject.toml` (foo)
DEBUG Acquired lock for `C:\Users\User\AppData\Roaming\uv\data\tools`
DEBUG Released lock at `C:\Users\User\AppData\Roaming\uv\data\tools\.lock`
DEBUG Caching via interpreter: `C:\Users\User\AppData\Roaming\uv\data\python\cpython-3.13.0-windows-x86_64-none\python.exe`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: foo @ file:///C:/Users/User/Projects/Test/foo
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: foo*
DEBUG Searching for a compatible version of foo @ file:///C:/Users/User/Projects/Test/foo (*)
DEBUG Tried 1 versions: foo 1
DEBUG Split specific environment resolution took 0.001s
Resolved 1 package in 4ms
DEBUG Ignoring empty directory
DEBUG Using request timeout of 30s
DEBUG Identified uncached distribution: foo @ file:///C:/Users/User/Projects/Test/foo
DEBUG Acquired lock for `C:\Users\User\AppData\Local\uv\cache\sdists-v4\path\e9b4b6a9e9eb107a`
DEBUG Building: foo @ file:///C:/Users/User/Projects/Test/foo
DEBUG No workspace root found, using project root
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.13.0
DEBUG Solving with target Python version: >=3.13.0
DEBUG Adding direct dependency: hatchling*
DEBUG Found fresh response for: https://pypi.org/simple/hatchling/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Found fresh response for: https://pypi.org/simple/pluggy/
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Found fresh response for: https://pypi.org/simple/trove-classifiers/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/4b/5c/65ae209acac6038f9576893e983b1abcdd6268cab7f5ab124103f4dde701/trove_classifiers-2024.10.13-py3-none-any.whl.metadata
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.10.13 [compatible] (trove_classifiers-2024.10.13-py3-none-any.whl)
DEBUG Tried 5 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1
DEBUG Split specific environment resolution took 0.007s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.13 in C:\Users\User\AppData\Local\uv\cache\builds-v0\.tmpVLpN99
DEBUG Requirement already cached: hatchling==1.25.0
DEBUG Requirement already cached: packaging==24.1
DEBUG Requirement already cached: pathspec==0.12.1
DEBUG Requirement already cached: pluggy==1.5.0
DEBUG Requirement already cached: trove-classifiers==2024.10.13
DEBUG Installing build requirements: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.10.13
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG No workspace root found, using project root
DEBUG Calling `hatchling.build.build_wheel("C:\\Users\\User\\AppData\\Local\\uv\\cache\\sdists-v4\\path\\e9b4b6a9e9eb107a\\d-QOBkfdZKPxrelgX-oX8\\.tmplT1L5H", {}, None)`
DEBUG Finished building: foo @ file:///C:/Users/User/Projects/Test/foo
DEBUG Released lock at `C:\Users\User\AppData\Local\uv\cache\sdists-v4\path\e9b4b6a9e9eb107a\.lock`
Prepared 1 package in 890ms
Installed 1 package in 13ms
 + foo==0.1.0 (from file:///C:/Users/User/Projects/Test/foo)
DEBUG Running `foo`
DEBUG Looking at `.dist-info` at: C:\Users\User\AppData\Local\uv\cache\archive-v0\f-oVDTBZALXPvVuNI7m51\Lib\site-packages\foo-0.1.0.dist-info
Hello from foo!
DEBUG Command exited with code: 0
```

---

_Comment by @charliermarsh on 2024-10-16 12:46_

I think right now you would need to have the `.python-version` file in the user's directory, rather than just in the project directory. https://github.com/astral-sh/uv/pull/7827 might unlock doing something better here. \cc @zanieb 

---

_Label `needs-design` added by @charliermarsh on 2024-10-16 12:46_

---

_Comment by @DavidCEllis on 2024-10-16 14:43_

> This creates a `.python-version` pinned at `3.12`. I also go into the `pyproject.toml` file and change `requires-python` to
> 
> ```diff
> - requires-python = ">=3.12"
> + requires-python = "~=3.12"
> ```

@my1e5 

I don't know if respecting `pyproject.toml` is intended, but `~=3.12` here means `>=3.12, ==3.*` and not `==3.12.*` so this wouldn't stop Python 3.13 from being used.

https://packaging.python.org/en/latest/specifications/version-specifiers/#compatible-release

```python
>>> from packaging.specifiers import SpecifierSet
>>> spec = SpecifierSet("~=3.12")
>>> "3.13" in spec
True
>>> spec2 = SpecifierSet("~=3.12.0")
>>> "3.13" in spec2
False
>>> "3.12.7" in spec2
True
```

---

_Comment by @my1e5 on 2024-10-16 15:04_

Thanks @DavidCEllis - good to know, I didn't realise this. 

It seems uvx isn't respecting the `requires-python` field at the moment anyway (https://github.com/astral-sh/uv/issues/6381#issuecomment-2303072011). But that might change with PR https://github.com/astral-sh/uv/pull/7827?

---

_Comment by @my1e5 on 2025-02-19 10:58_

Seems to still be an issue in uv `0.6.1`. New MRE in https://github.com/my1e5/uv-tool-python-version

---

_Comment by @bbeaudreault on 2025-03-13 15:20_

Any update here? It sort of ruins the promise of "makes it easy to run a package on anyone's computer"

---

_Comment by @my1e5 on 2025-03-13 16:21_

@bbeaudreault - see

* https://github.com/astral-sh/uv/issues/11624

This is the most relevant tracking issue now 

---

_Comment by @ForsakenRei on 2025-04-10 14:07_

I noticed this recently after switching to uv but not sure if I'm understanding the `uvx` interface correctly. When I have a `.python-version` in the working dir, `uv` does respect the python version I pinned, however `uvx` is not.
For example if I run `uv python pin 3.11.11` then run `uv run python` I will get into a python 3.11.11 console, but if I run `uvx python` I will get into a different python console, for my case it is 3.12.10 but I have never pinned 3.12.10 globally with `--global` flag. Is this by design since `uvx` is `uv tool run` which run tools in an isolated environment?

---

_Comment by @helmut-hoffer-von-ankershoffen on 2025-12-02 09:47_

It's leads to a very bad customer experience that `uvx <tool>` ignores the upper bound defined in `requires-python` of the tool's `pyproject.toml,` in light of 3.14 having been released and some dependencies not having been built for that.

What happens is that customers call `uvx <tool>` on their notebook with python 3.14.0 system installed. Some dependency of tool is not yet built for 3.14. On MacOS X compilation fails because the customer does not have xcode license agreement accepted; on Windows no tool chain present to build.

The only mitigation seems to be to have the user do a `uv python install 3.13` first as uvx seems to prefer managed installations by default over the system installed 3.14.

What's holding back to respect the upper bound defined in requires-python? @zanieb 


---

_Comment by @my1e5 on 2025-12-02 09:58_

> The only mitigation seems to be to have the user do a `uv python install 3.13` first as uvx seems to prefer managed installations by default over the system installed 3.14.

You can also do `uvx -p 3.13 <tool>`

---

_Comment by @helmut-hoffer-von-ankershoffen on 2025-12-02 10:01_

@my1e5 Sure, but that's something the user would have to do on every tool call, and would have to change when 3.14 is supported. So not great experience.

---
