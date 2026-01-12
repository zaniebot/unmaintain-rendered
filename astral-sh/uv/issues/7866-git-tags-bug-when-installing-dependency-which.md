```yaml
number: 7866
title: Git tags bug when installing dependency which uses dynamic versioning
type: issue
state: open
author: my1e5
labels:
  - wish
assignees: []
created_at: 2024-10-02T10:06:37Z
updated_at: 2024-10-17T23:56:08Z
url: https://github.com/astral-sh/uv/issues/7866
synced_at: 2026-01-12T15:59:17Z
```

# Git tags bug when installing dependency which uses dynamic versioning

---

_@my1e5_

I'm sorry I don't have a full MRE of this at the moment, but I wanted to report it anyway for the record.

I'm adding a dependency from a Git URL which uses dynamic versioning - so the version number is determined at build time using the Git tags. It uses the [`versioningit`](https://pypi.org/project/versioningit/) plugin for Hatch.

Consider I have a tag on my remote called `v0.3.1` which is associated with commit hash `5cf4ddf`. On my Windows PC, this is working fine:
```
$ uv add git+ssh://git@mygitserver/myproject --tag v0.3.1
 Updated ssh://git@mygitserver/myproject (5cf4ddf)
 Updated ssh://git@mygitserver/myproject (5cf4ddf)
Resolved 16 packages in 2.41s
Installed 13 packages in 1.87s
.
.
 + myproject==0.3.1 (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
```
But then I encountered this on my macOS machine (I don't know if the OS is relevant)
```
$ uv add git+ssh://git@mygitserver/myproject --tag v0.3.1
 Updated ssh://git@mygitserver/myproject (5cf4ddf)
 Updated ssh://git@mygitserver/myproject (5cf4ddf)
Resolved 16 packages in 2.41s
Installed 13 packages in 1.87s
.
.
 + myproject==0.3.0.post6+g5cf4ddf (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
```
Note how it has installed the same commit but is building it as version `0.3.0.post6+g5cf4ddf` - because during the build stage it is somehow unable to see the `v0.3.1` tag? Even though it installed `5cf4ddf` associated with the tag.

I managed to work out it was a uv bug - because I tried using pip on the same macOS machine and it was able to correctly build and install the project using the correct version number.
```
$ uv add pip
$ uv run pip install git+ssh://git@mygitserver/myproject@v0.3.1
.
.
Successfully installed myproject-0.3.1
```
I wondered if it could be a cache problem so I tried using `--reinstall` (which I understood to imply `--refresh` and would therefore refresh cached data?)
```
$ uv add git+ssh://git@mygitserver/myproject --tag v0.3.1 --reinstall
.
.
 + myproject==0.3.0.post6+g5cf4ddf (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
```
But that led to same thing.

It was only after I ran `uv cache clean` and tried again that I got it to work.
```
$ uv cache clean
Clearing cache at: /Users/me/.cache/uv
Removed 46639 files (795.7MiB)

$ uv add git+ssh://git@mygitserver/myproject --tag v0.3.1 --reinstall
.
.
 + myproject==0.3.1 (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
```
I haven't tried to recreate this - it's a little tricky to do so - but I wanted to report it regardless in case this gave any hints at a possible bug. My impression is uv was in some way not properly updating or refreshing the git tags? So it took cleaning the entire cache to get the git history to properly update?

Also sorry I'm not able to go back and generate verbose logs.

uv version `0.4.17`

---

_Comment by @my1e5 on 2024-10-03 10:12_

I've managed to reproduce it. 

Start by adding a dependency from a Git URL which uses dynamic versioning. For example, here I am adding whatever the latest commit is from the `main` branch (which is currently untagged):
```
$ uv add git+ssh://git@mygitserver/myproject
 Updated ssh://git@mygitserver/myproject (5cf4ddf)
 Updated ssh://git@mygitserver/myproject (5cf4ddf)
Resolved 16 packages in 2.41s
Installed 13 packages in 1.87s
.
.
 + myproject==0.3.0.post6+g5cf4ddf (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
```
So uv has correctly fetched commit `5cf4ddf` and the dynamic versioning has correctly labelled the build as `0.3.0.post6+g5cf4ddf`. This means 'six commits post `v0.3.0`' (which is the latest tag in the git history) - there is currently no `v0.3.1` tag present in the remote. All good so far.

Next, create a new release tag on the remote called `v0.3.1` which is associated with commit `5cf4ddf`. 

Now, it seems that despite trying to use `--reinstall` you cannot get uv to build `myproject` using this new version tag. It's like uv remembers that it has a cached version of `myproject @ 5cf4ddf` stored and so will forever build it as `0.3.0.post6+g5cf4ddf` which is the version name it first created for it.

```
$ uv add -v git+ssh://git@mygitserver/myproject --tag v0.3.1 --reinstall
```

<details>
<summary>Expand verbose logs</summary>
<pre>
DEBUG uv 0.4.18
DEBUG Found project root: `/Users/.........../test-project`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/.........../test-project/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: ssh://git@mygitserver/myproject
DEBUG Acquired lock for `ssh://mygitserver/myproject`
DEBUG Updating Git source `ssh://git@mygitserver/myproject`
DEBUG Performing a Git fetch for: ssh://git@mygitserver/myproject
DEBUG Released lock at `/Users/me/.cache/uv/git-v0/locks/9f1202babc7ca148`
DEBUG Acquired lock for `/Users/me/.cache/uv/sdists-v4/git/0f5be1768a6ffc48/114ab0c03a727caf`
DEBUG No static `pyproject.toml` available for: git+ssh://git@mygitserver/myproject (PyprojectToml(DynamicField("version")))
DEBUG No static `PKG-INFO` available for: git+ssh://git@mygitserver/myproject (MissingPkgInfo)
DEBUG No static `egg-info` available for: git+ssh://git@mygitserver/myproject (MissingEggInfo)
DEBUG Preparing metadata for: git+ssh://git@mygitserver/myproject
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.4
DEBUG Solving with target Python version: >=3.12.4
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: versioningit*
DEBUG No cache entry for: https://pypi.org/simple/hatchling/
DEBUG No cache entry for: https://pypi.org/simple/versioningit/
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/56/50784a34941e6a77cb068289c851d35c8b9af6a4d266fdb85d4d4828fe21/versioningit-3.1.2-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Searching for a compatible version of versioningit (*)
DEBUG Selecting: versioningit==3.1.2 [compatible] (versioningit-3.1.2-py3-none-any.whl)
DEBUG No cache entry for: https://pypi.org/simple/pathspec/
DEBUG No cache entry for: https://pypi.org/simple/pluggy/
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/trove-classifiers/
DEBUG Adding transitive dependency for versioningit==3.1.2: packaging>=17.1
DEBUG No cache entry for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b6/7a/e0edec9c8905e851d52076bbc41890603e2ba97cf64966bc1498f2244fd2/trove_classifiers-2024.9.12-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.9.12 [compatible] (trove_classifiers-2024.9.12-py3-none-any.whl)
DEBUG Tried 6 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1, versioningit 1
DEBUG Split specific environment resolution took 0.221s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12, versioningit==3.1.2 in /Users/me/.cache/uv/builds-v0/.tmpYsIRmx
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Must revalidate requirement: versioningit
DEBUG Downloading and building requirements for build: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12, versioningit==3.1.2
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/7f/56/50784a34941e6a77cb068289c851d35c8b9af6a4d266fdb85d4d4828fe21/versioningit-3.1.2-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/b6/7a/e0edec9c8905e851d52076bbc41890603e2ba97cf64966bc1498f2244fd2/trove_classifiers-2024.9.12-py3-none-any.whl
DEBUG Installing build requirements: hatchling==1.25.0, versioningit==3.1.2, packaging==24.1, pluggy==1.5.0, pathspec==0.12.1, trove-classifiers==2024.9.12
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.prepare_metadata_for_build_wheel()`
DEBUG Prepared metadata for: git+ssh://git@mygitserver/myproject
DEBUG No workspace root found, using project root
DEBUG Released lock at `/Users/me/.cache/uv/sdists-v4/git/0f5be1768a6ffc48/114ab0c03a727caf/.lock`
DEBUG Caching credentials for: ssh://git@mygitserver/myproject
DEBUG No changes to dependencies; skipping update
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-project @ file:///Users/.........../test-project
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 16 packages in 0.30ms
DEBUG Using request timeout of 30s
.
.
DEBUG Must revalidate requirement: myproject
.
.
DEBUG Fetching source distribution from Git: ssh://git@mygitserver/myproject
DEBUG Acquired lock for `ssh://mygitserver/myproject`
.
.
.
DEBUG Using existing Git source `ssh://git@mygitserver/myproject`
.
DEBUG Released lock at `/Users/me/.cache/uv/git-v0/locks/9f1202babc7ca148`
DEBUG Acquired lock for `/Users/me/.cache/uv/sdists-v4/git/d98613b14b70cff7/114ab0c03a727caf`
DEBUG Released lock at `/Users/me/.cache/uv/sdists-v4/git/d98613b14b70cff7/114ab0c03a727caf/.lock`
Prepared 12 packages in 92ms
.
.
DEBUG Uninstalled myproject (47 files, 13 directories)
.
.
</pre>
</details>

```
Uninstalled 12 packages in 189ms
Installed 12 packages in 25ms
.
.
~ myproject==0.3.0.post6+g5cf4ddf (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
.
```
The only way I've found to fix this is to run `uv cache clean`.
```
$ uv -v cache clean
DEBUG uv 0.4.18
Clearing cache at: /Users/me/.cache/uv
Removed 309 files (2.3MiB)
```
```
$ uv add -v git+ssh://git@mygitserver/myproject --tag v0.3.1 --reinstall
```
<details>
<summary>Expand verbose logs</summary>
<pre>
DEBUG uv 0.4.18
DEBUG Found project root: `/Users/........./test-project`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `/Users/........./test-project/.python-version`
DEBUG The virtual environment's Python version satisfies `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Fetching source distribution from Git: ssh://git@mygitserver/myproject
DEBUG Acquired lock for `ssh://mygitserver/myproject`
DEBUG Updating Git source `ssh://git@mygitserver/myproject`
DEBUG Performing a Git fetch for: ssh://git@mygitserver/myproject
DEBUG reset /Users/me/.cache/uv/git-v0/checkouts/9f1202babc7ca148/114ab0c to 5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9
DEBUG Released lock at `/Users/me/.cache/uv/git-v0/locks/9f1202babc7ca148`
DEBUG Acquired lock for `/Users/me/.cache/uv/sdists-v4/git/0f5be1768a6ffc48/114ab0c03a727caf`
DEBUG No static `pyproject.toml` available for: git+ssh://git@mygitserver/myproject (PyprojectToml(DynamicField("version")))
DEBUG No static `PKG-INFO` available for: git+ssh://git@mygitserver/myproject (MissingPkgInfo)
DEBUG No static `egg-info` available for: git+ssh://git@mygitserver/myproject (MissingEggInfo)
DEBUG Preparing metadata for: git+ssh://git@mygitserver/myproject
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.4
DEBUG Solving with target Python version: >=3.12.4
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: versioningit*
DEBUG No cache entry for: https://pypi.org/simple/hatchling/
DEBUG No cache entry for: https://pypi.org/simple/versioningit/
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/56/50784a34941e6a77cb068289c851d35c8b9af6a4d266fdb85d4d4828fe21/versioningit-3.1.2-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Searching for a compatible version of versioningit (*)
DEBUG Selecting: versioningit==3.1.2 [compatible] (versioningit-3.1.2-py3-none-any.whl)
DEBUG Adding transitive dependency for versioningit==3.1.2: packaging>=17.1
DEBUG No cache entry for: https://pypi.org/simple/packaging/
DEBUG No cache entry for: https://pypi.org/simple/pathspec/
DEBUG No cache entry for: https://pypi.org/simple/pluggy/
DEBUG No cache entry for: https://pypi.org/simple/trove-classifiers/
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl.metadata
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b6/7a/e0edec9c8905e851d52076bbc41890603e2ba97cf64966bc1498f2244fd2/trove_classifiers-2024.9.12-py3-none-any.whl.metadata
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.9.12 [compatible] (trove_classifiers-2024.9.12-py3-none-any.whl)
DEBUG Tried 6 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1, versioningit 1
DEBUG Split specific environment resolution took 0.199s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12, versioningit==3.1.2 in /Users/me/.cache/uv/builds-v0/.tmpgQcR3M
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Must revalidate requirement: versioningit
DEBUG Downloading and building requirements for build: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12, versioningit==3.1.2
DEBUG No cache entry for: https://files.pythonhosted.org/packages/0c/8b/90e80904fdc24ce33f6fc6f35ebd2232fe731a8528a22008458cf197bc4d/hatchling-1.25.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/08/aa/cc0199a5f0ad350994d660967a8efb233fe0416e4639146c089643407ce6/packaging-24.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/7f/56/50784a34941e6a77cb068289c851d35c8b9af6a4d266fdb85d4d4828fe21/versioningit-3.1.2-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/88/5f/e351af9a41f866ac3f1fac4ca0613908d9a41741cfcf2228f4ad853b697d/pluggy-1.5.0-py3-none-any.whl
DEBUG No cache entry for: https://files.pythonhosted.org/packages/b6/7a/e0edec9c8905e851d52076bbc41890603e2ba97cf64966bc1498f2244fd2/trove_classifiers-2024.9.12-py3-none-any.whl
DEBUG Installing build requirements: packaging==24.1, hatchling==1.25.0, versioningit==3.1.2, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.prepare_metadata_for_build_wheel()`
DEBUG Prepared metadata for: git+ssh://git@mygitserver/myproject
DEBUG No workspace root found, using project root
DEBUG Released lock at `/Users/me/.cache/uv/sdists-v4/git/0f5be1768a6ffc48/114ab0c03a727caf/.lock`
DEBUG Caching credentials for: ssh://git@mygitserver/myproject
DEBUG No changes to dependencies; skipping update
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: test-project @ file:///Users/me/........./test-project
DEBUG No workspace root found, using project root
DEBUG Existing `uv.lock` satisfies workspace requirements
Resolved 16 packages in 0.34ms
DEBUG Using request timeout of 30s
.
.
DEBUG Must revalidate requirement: myproject
.
.
DEBUG Fetching source distribution from Git: ssh://git@mygitserver/myproject
DEBUG No cache entry for: ...
.
.
DEBUG Using existing Git source `ssh://git@mygitserver/myproject`
DEBUG Released lock at `/Users/me/.cache/uv/git-v0/locks/9f1202babc7ca148`
DEBUG Acquired lock for `/Users/me/.cache/uv/sdists-v4/git/d98613b14b70cff7/114ab0c03a727caf`
DEBUG Building: myproject @ git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.12.4
DEBUG Solving with target Python version: >=3.12.4
DEBUG Adding direct dependency: hatchling*
DEBUG Adding direct dependency: versioningit*
DEBUG Searching for a compatible version of hatchling (*)
DEBUG Selecting: hatchling==1.25.0 [compatible] (hatchling-1.25.0-py3-none-any.whl)
DEBUG Adding transitive dependency for hatchling==1.25.0: packaging>=23.2
DEBUG Adding transitive dependency for hatchling==1.25.0: pathspec>=0.10.1
DEBUG Adding transitive dependency for hatchling==1.25.0: pluggy>=1.0.0
DEBUG Adding transitive dependency for hatchling==1.25.0: trove-classifiers*
DEBUG Searching for a compatible version of versioningit (*)
DEBUG Selecting: versioningit==3.1.2 [compatible] (versioningit-3.1.2-py3-none-any.whl)
DEBUG Adding transitive dependency for versioningit==3.1.2: packaging>=17.1
DEBUG Searching for a compatible version of packaging (>=23.2)
DEBUG Selecting: packaging==24.1 [compatible] (packaging-24.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pathspec (>=0.10.1)
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
DEBUG Searching for a compatible version of pluggy (>=1.0.0)
DEBUG Selecting: pluggy==1.5.0 [compatible] (pluggy-1.5.0-py3-none-any.whl)
DEBUG Searching for a compatible version of trove-classifiers (*)
DEBUG Selecting: trove-classifiers==2024.9.12 [compatible] (trove_classifiers-2024.9.12-py3-none-any.whl)
DEBUG Tried 6 versions: hatchling 1, packaging 1, pathspec 1, pluggy 1, trove-classifiers 1, versioningit 1
DEBUG Split specific environment resolution took 0.000s
DEBUG Installing in hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12, versioningit==3.1.2 in /Users/me/.cache/uv/builds-v0/.tmpx66bCo
DEBUG Must revalidate requirement: hatchling
DEBUG Must revalidate requirement: packaging
DEBUG Must revalidate requirement: pathspec
DEBUG Must revalidate requirement: pluggy
DEBUG Must revalidate requirement: trove-classifiers
DEBUG Must revalidate requirement: versioningit
DEBUG Downloading and building requirements for build: hatchling==1.25.0, packaging==24.1, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12, versioningit==3.1.2
DEBUG Installing build requirements: hatchling==1.25.0, packaging==24.1, versioningit==3.1.2, pathspec==0.12.1, pluggy==1.5.0, trove-classifiers==2024.9.12
DEBUG Creating PEP 517 build environment
DEBUG Calling `hatchling.build.get_requires_for_build_wheel()`
DEBUG Calling `hatchling.build.build_wheel("/Users/me/.cache/uv/sdists-v4/git/d98613b14b70cff7/114ab0c03a727caf/.tmpnqHogz", {}, None)`
DEBUG Finished building: myproject @ git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9
DEBUG Released lock at `/Users/me/.cache/uv/sdists-v4/git/d98613b14b70cff7/114ab0c03a727caf/.lock`
Prepared 12 packages in 2.04s
.
.
DEBUG Uninstalled myproject (47 files, 13 directories)
.
.
</pre>
</details>

```
Uninstalled 12 packages in 231ms
Installed 12 packages in 24ms
.
.
 - myproject==0.3.0.post6+g5cf4ddf (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
 + myproject==0.3.1 (from git+ssh://git@mygitserver/myproject@5cf4ddf3a85c705daa92ab96c7db96a64d77cbb9)
.
.
```
Now uv has correctly renamed the build as `myproject==0.3.1`.

---

_Comment by @charliermarsh on 2024-10-03 13:18_

Can you find a way for me to reproduce this with a public repository? My guess is the thinking is that we treat Git commits as immutable, but this plugin is now breaking that assumption, since the version is based on other state.

---

_Label `needs-mre` added by @charliermarsh on 2024-10-03 13:18_

---

_Comment by @my1e5 on 2024-10-03 14:14_

To reproduce this you need to `uv add` the repository first and then go and create a new release. I can outline the most simple way to reproduce it.

### 1. Create a Python library with dynamic versioning (and push to Github)

```
$ uv init --lib test-versioning
```
Modify the `pyproject.toml` to use a dynamic version and the `versioningit` plugin for Hatch.
```
[project]
name = "test-versioning"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
dynamic = ["version"]

[build-system]
requires = ["hatchling", "versioningit"]
build-backend = "hatchling.build"

[tool.hatch.version]
source = "versioningit"

[tool.versioningit]
default-version = "0.0.0"
```
This should build successfully and use the default-version of `0.0.0` (as there exist no git tags at this point)
```
$ uv sync
Using CPython 3.12.4
Creating virtual environment at: .venv
Resolved 1 package in 1.10s
   Built test-versioning @ file:///C:/Users/User/projects/test-versioning
Prepared 1 package in 1.17s
Installed 1 package in 12ms
 + test-versioning==0.0.0 (from file:///C:/Users/User/projects/test-versioning)
```
Create a new repo on Github - e.g. `https://github.com/<username>/test-versioning`
```
$ git add .
$ git commit -m "initial commit"
$ git branch -M main
$ git remote add origin https://github.com/<username>/test-versioning.git
$ git push -u origin main
```
### 2. Create a new Python project and add this Git repo as a dependency
```
$ uv init test-project
$ cd test-project/
$ uv add git+https://github.com/<username>/test-versioning
Using CPython 3.12.4
Creating virtual environment at: .venv
 Updated https://github.com/<username>/test-versioning (76e5c0c)
 Updated https://github.com/<username>/test-versioning (76e5c0c)
Resolved 2 packages in 922ms
   Built test-versioning @ git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245
Prepared 1 package in 1.26s
Installed 1 package in 10ms
 + test-versioning==0.0.0 (from git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245)
```
As you can see, it's correctly built as version `0.0.0`

### 3. Create a release on GitHub

For example, create a new tag called `v0.1.0` with 'Target: main'.

Back in `test-project`, when we use this tag, it finds it alright but still calls the build `0.0.0`
```
$ uv add git+https://github.com/<username>/test-versioning --tag=v0.1.0 --reinstall
 Updated https://github.com/<username>/test-versioning (76e5c0c)
 Updated https://github.com/<username>/test-versioning (76e5c0c)
Resolved 2 packages in 2.25s
Prepared 1 package in 150ms
Uninstalled 1 package in 2ms
Installed 1 package in 13ms
 ~ test-versioning==0.0.0 (from git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245)
```

### 4. Clearing the cache for the specific package doesn't work.
```
$ uv cache clean test-versioning
Removed 13 files for test-versioning (1.9KiB)

$ uv add git+https://github.com/<username>/test-versioning --tag=v0.1.0 --reinstall
 Updated https://github.com/<username>/test-versioning (76e5c0c)
Resolved 2 packages in 0.81ms
   Built test-versioning @ git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245
Prepared 1 package in 1.24s
Uninstalled 1 package in 1ms
Installed 1 package in 10ms
 ~ test-versioning==0.0.0 (from git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245)
 ```
 
 ### 5. But clearing the entire cache does work
 ```
 $ uv cache clean
Clearing cache at: C:\Users\User\AppData\Local\uv\cache
Removed 9772 files (569.0MiB)

$ uv add git+https://github.com/<username>/test-versioning --tag=v0.1.0 --reinstall
 Updated https://github.com/<username>/test-versioning (76e5c0c)
Resolved 2 packages in 0.83ms
   Built test-versioning @ git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245
Prepared 1 package in 1.30s
Uninstalled 1 package in 1ms
Installed 1 package in 11ms
 - test-versioning==0.0.0 (from git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245)
 + test-versioning==0.1.0 (from git+https://github.com/<username>/test-versioning@76e5c0cca634a2a1b8624eb5ae00af3660e94245)
```
Only when you run `uv cache clean` does it reinstall as version `0.1.0`.

I've tested this MRE myself at https://github.com/my1e5/test-versioning - but to reproduce it yourself you need to be able to add the Git repo before the release tag is created. So you will need to make your own repo. Hope this is helpful.
 

---

_Comment by @my1e5 on 2024-10-04 12:53_

Actually, it can also be reproduced completely locally. Here is a full example involving two workspaces:

```
user@user1 MINGW64 ~/Projects/Test
$ mkdir testing-space

user@user1 MINGW64 ~/Projects/Test
$ cd testing-space

user@user1 MINGW64 ~/Projects/Test/testing-space
$ uv init --lib dynamic-version-lib
Initialized project `dynamic-version-lib` at `C:\Users\user\Projects\Test\testing-space\dynamic-version-lib`

user@user1 MINGW64 ~/Projects/Test/testing-space
$ uv init test-project
Initialized project `test-project` at `C:\Users\user\Projects\Test\testing-space\test-project`

user@user1 MINGW64 ~/Projects/Test/testing-space
$ cd dynamic-version-lib/

user@user1 MINGW64 ~/Projects/Test/testing-space/dynamic-version-lib (main)
$ code .

* edit the pyproject.toml file to:

    [project]
    name = "dynamic-version-lib"
    description = "Add your description here"
    readme = "README.md"
    requires-python = ">=3.12"
    dependencies = []
    dynamic = ["version"]

    [build-system]
    requires = ["hatchling", "versioningit"]
    build-backend = "hatchling.build"

    [tool.hatch.version]
    source = "versioningit"
    default-version = "0.0.0"


user@user1 MINGW64 ~/Projects/Test/testing-space/dynamic-version-lib (main)
$ git add .
$ git commit -m "test"
[main (root-commit) c7d497c] test
 6 files changed, 28 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .python-version
 create mode 100644 README.md
 create mode 100644 pyproject.toml
 create mode 100644 src/dynamic_version_lib/__init__.py
 create mode 100644 src/dynamic_version_lib/py.typed

user@user1 MINGW64 ~/Projects/Test/testing-space/dynamic-version-lib (main)
$ uv sync
Using CPython 3.12.4
Creating virtual environment at: .venv
Resolved 1 package in 1.47s
   Built dynamic-version-lib @ file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib
Prepared 1 package in 1.15s
Installed 1 package in 14ms
 + dynamic-version-lib==0.0.0 (from file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib)

user@user1 MINGW64 ~/Projects/Test/testing-space/dynamic-version-lib (main)
$ cd ..

user@user1 MINGW64 ~/Projects/Test/testing-space
$ ls
dynamic-version-lib/  test-project/

user@user1 MINGW64 ~/Projects/Test/testing-space
$ cd test-project/

user@user1 MINGW64 ~/Projects/Test/testing-space/test-project (main)
$ uv add git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib
Using CPython 3.12.4
Creating virtual environment at: .venv
 Updated file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib (c7d497c)
 Updated file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib (c7d497c)
Resolved 2 packages in 293ms
   Built dynamic-version-lib @ git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib@c7d497c99f355b6314478a3304a13bcff11394cd
Prepared 1 package in 1.24s
Installed 1 package in 10ms
 + dynamic-version-lib==0.0.0 (from git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib@c7d497c99f355b6314478a3304a13bcff11394cd)

user@user1 MINGW64 ~/Projects/Test/testing-space/test-project (main)
$ cd ..

user@user1 MINGW64 ~/Projects/Test/testing-space
$ cd dynamic-version-lib/

user@user1 MINGW64 ~/Projects/Test/testing-space/dynamic-version-lib (main)
$ git tag v0.1.0

user@user1 MINGW64 ~/Projects/Test/testing-space/dynamic-version-lib (main)
$ cd ..

user@user1 MINGW64 ~/Projects/Test/testing-space
$ cd test-project/

user@user1 MINGW64 ~/Projects/Test/testing-space/test-project (main)
$ uv add git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib --reinstall
 Updated file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib (c7d497c)
Resolved 2 packages in 0.84ms
Prepared 1 package in 136ms
Uninstalled 1 package in 1ms
Installed 1 package in 11ms
 ~ dynamic-version-lib==0.0.0 (from git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib@c7d497c99f355b6314478a3304a13bcff11394cd)

user@user1 MINGW64 ~/Projects/Test/testing-space/test-project (main)
$ uv cache clean
Clearing cache at: C:\Users\user\AppData\Local\uv\cache
Removed 367 files (1.3MiB)

user@user1 MINGW64 ~/Projects/Test/testing-space/test-project (main)
$ uv add git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib --reinstall
 Updated file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib (c7d497c)
Resolved 2 packages in 0.81ms
   Built dynamic-version-lib @ git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib@c7d497c99f355b6314478a3304a13bcff11394cd
Prepared 1 package in 1.30s
Uninstalled 1 package in 1ms
Installed 1 package in 11ms
 - dynamic-version-lib==0.0.0 (from git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib@c7d497c99f355b6314478a3304a13bcff11394cd)
 + dynamic-version-lib==0.1.0 (from git+file:///C:/Users/user/Projects/Test/testing-space/dynamic-version-lib@c7d497c99f355b6314478a3304a13bcff11394cd)
 ```

---

_Comment by @charliermarsh on 2024-10-04 12:54_

Thanks.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-16 13:56_

---

_Closed by @charliermarsh on 2024-10-16 15:13_

---

_Closed by @charliermarsh on 2024-10-16 15:13_

---

_Comment by @my1e5 on 2024-10-17 22:06_

@charliermarsh - PR #8259 did fix issue #7997, but I've just tested the above MRE with uv 0.4.23 - and this bug still remains.

The point is when you add a dynamic version library from git to another project, then no matter what you try uv will always remember the git commit hash is associated with the first version number it built. So it can never get new tag updates. Even if you `uv sync --reinstall` or you try add it again but use `--reinstall` it won't retrieve any new tag updates.

The only way to fix it is to clear your entire cache with `uv cache clean`

Adding `cache-keys = [{ git = { commit = true, tags = true } }]` in the project has no effect in this case. Because it's not the Git tags of the current project we want. We want to get updates for the dependencies.

---

_Reopened by @charliermarsh on 2024-10-17 22:09_

---

_Comment by @charliermarsh on 2024-10-17 22:58_

Ah thanks -- I re-opened! Though I'll admit that I'm tempted to mark this as "won't fix"... I just find it challenging that a Git commit SHA would not be indicative of a reproducible build, and that the build instead relies on what is effectively ambient global state.

I guess `--reinstall` or `--refresh` should arguably re-pull the list of refs... maybe? I don't think it should work without some explicit opt-in like a flag though, at the very least.

---

_Label `needs-mre` removed by @charliermarsh on 2024-10-17 22:58_

---

_Label `wish` added by @charliermarsh on 2024-10-17 22:58_

---

_Comment by @my1e5 on 2024-10-17 23:44_

> I just find it challenging that a Git commit SHA would not be indicative of a reproducible build, and that the build instead relies on what is effectively ambient global state.

I understand what you mean. I guess this is simply the nature of a project with dynamic metadata.

All I think is neccessary is a way to properly force a Git dependency to rebuild itself. In the same way I could (even before PR #8259) force my project to rebuild (using `sync --reinstall`) and refresh its dynamic metadata. If I add a dependency to my project I want to be able to force reinstall it and refresh all its dynamic metadata too.

`--reinstall` seems like the natural flag a user might reach for. Maybe it needs to be more explicit like a `--force-reinstall`?

At the moment it just feels very unsatisfactory that the only way to refresh the build is to do an entire `uv cache clean`. If there was a way to clean the cache just for the specific dependency in question then that would be great.

---

_Comment by @my1e5 on 2024-10-17 23:56_

To clarify that last point - at the moment, if I run 
```
$ uv cache clean dynamic-version-lib
Removed 11 files for dynamic-version-lib (2.1KiB)
```
it doesn't result in the metadata getting refreshed when I add it to project again. Only `uv cache clean` works.

---
