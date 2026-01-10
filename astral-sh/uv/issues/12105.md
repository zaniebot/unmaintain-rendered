```yaml
number: 12105
title: "`uv add` gives ambiguous error"
type: issue
state: closed
author: iloveitaly
labels:
  - error messages
assignees: []
created_at: 2025-03-10T22:20:55Z
updated_at: 2025-03-11T03:00:10Z
url: https://github.com/astral-sh/uv/issues/12105
synced_at: 2026-01-10T03:50:31Z
```

# `uv add` gives ambiguous error

---

_Issue opened by @iloveitaly on 2025-03-10 22:20_

### Summary

I attempted to add a group of debugging tools using:

```
uv add -vvv --group debugging-extras ipython pudb debugpy git+https://github.com/iloveitaly/ipdb@support-executables 'pdbr[ipython]' rich git+https://github.com/iloveitaly/ipython-autoimport@ipython-9.x IPythonClipboard ipython_ctrlr_fzf docrepr pyfzf jedi pretty-traceback pre-commit sqlparse git+https://github.com/iloveitaly/ipython-suggestions.git@ipython-9.x rpdb datamodel-code-generator funcy-pipe colorama pipdeptree icecream httpdbg
```

And got the following error:

```
error: Cannot perform ambiguous update; found multiple entries with matching package names
```

However, I have no idea which package caused the issue. It would be great if more context was given in the error messag.e

A verbose log didn't help me:

```
    0.000804s DEBUG uv uv 0.6.3 (a0b9f22a2 2025-02-24)
    0.006197s DEBUG uv_workspace::workspace Found project root: `/Users/mike/Projects/cosma/cosma`
    0.006517s DEBUG uv_workspace::workspace No workspace root found, using project root
    0.007520s DEBUG uv_fs Acquired lock for `/Users/mike/Projects/cosma/cosma`
    0.009602s DEBUG uv::commands::project Using Python request `>=3.13` from `requires-python` metadata
    0.010063s DEBUG uv_python::environment Checking for Python environment at `.venv`
    0.011774s DEBUG uv::commands::project The virtual environment's Python version satisfies `>=3.13`
    0.011945s DEBUG uv_fs Released lock at `/var/folders/xs/kzl6z1xs3cz8trwrgmmfhxbc0000gn/T/uv-40a83a6b58fe5aaa.lock`
 uv_requirements::specification::from_source source=ipython
 uv_requirements::specification::from_source source=pudb
 uv_requirements::specification::from_source source=debugpy
 uv_requirements::specification::from_source source=git+https://github.com/iloveitaly/ipdb@support-executables
 uv_requirements::specification::from_source source=pdbr[ipython]
 uv_requirements::specification::from_source source=rich
 uv_requirements::specification::from_source source=git+https://github.com/iloveitaly/ipython-autoimport@ipython-9.x
 uv_requirements::specification::from_source source=IPythonClipboard
 uv_requirements::specification::from_source source=ipython_ctrlr_fzf
 uv_requirements::specification::from_source source=docrepr
 uv_requirements::specification::from_source source=pyfzf
 uv_requirements::specification::from_source source=jedi
 uv_requirements::specification::from_source source=pretty-traceback
 uv_requirements::specification::from_source source=pre-commit
 uv_requirements::specification::from_source source=sqlparse
 uv_requirements::specification::from_source source=git+https://github.com/iloveitaly/ipython-suggestions.git@ipython-9.x
 uv_requirements::specification::from_source source=rpdb
 uv_requirements::specification::from_source source=datamodel-code-generator
 uv_requirements::specification::from_source source=funcy-pipe
 uv_requirements::specification::from_source source=colorama
 uv_requirements::specification::from_source source=pipdeptree
 uv_requirements::specification::from_source source=icecream
 uv_requirements::specification::from_source source=httpdbg
 uv_client::linehaul::linehaul
    0.017564s DEBUG uv_client::base_client Using request timeout of 30s
 uv_resolver::flat_index::from_entries
    0.023510s DEBUG uv_git::resolver Attempting GitHub fast path for: https://api.github.com/repos/iloveitaly/ipdb/commits/support-executables
    0.031204s DEBUG uv_git::resolver Attempting GitHub fast path for: https://api.github.com/repos/iloveitaly/ipython-autoimport/commits/ipython-9.x
    0.031302s DEBUG uv_git::resolver Attempting GitHub fast path for: https://api.github.com/repos/iloveitaly/ipython-suggestions/commits/ipython-9.x
    0.217780s DEBUG uv_distribution::source Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/iloveitaly/ipdb/ae526fb6b3f5b13f1c69649a3bee711ded5a435d/pyproject.toml
    0.220727s DEBUG uv_distribution::source Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/iloveitaly/ipython-suggestions/99efb0017a2d851f58531b65a73e27c26185a146/pyproject.toml
    0.237357s DEBUG uv_distribution::source Failed to extract static metadata from GitHub API for: https://raw.githubusercontent.com/iloveitaly/ipdb/ae526fb6b3f5b13f1c69649a3bee711ded5a435d/pyproject.toml
    0.237736s DEBUG uv_git::resolver Fetching source distribution from Git: https://github.com/iloveitaly/ipdb
    0.238109s DEBUG uv_fs Acquired lock for `https://github.com/iloveitaly/ipdb`
 uv_git::source::fetch repository=https://github.com/iloveitaly/ipdb, rev=Some(GitOid { len: 40, bytes: [97, 101, 53, 50, 54, 102, 98, 54, 98, 51, 102, 53, 98, 49, 51, 102, 49, 99, 54, 57, 54, 52, 57, 97, 51, 98, 101, 101, 55, 49, 49, 100, 101, 100, 53, 97, 52, 51, 53, 100] })
    0.259472s  21ms DEBUG uv_git::source Using existing Git source `https://github.com/iloveitaly/ipdb`
    0.263196s DEBUG uv_distribution::source Attempting to fetch `pyproject.toml` from: https://raw.githubusercontent.com/iloveitaly/ipython-autoimport/4f39222ad693549cdc7b7f58c4818edee84a84cf/pyproject.toml
    0.268543s DEBUG uv_distribution::source Failed to extract static metadata from GitHub API for: https://raw.githubusercontent.com/iloveitaly/ipython-autoimport/4f39222ad693549cdc7b7f58c4818edee84a84cf/pyproject.toml
    0.268556s DEBUG uv_git::resolver Fetching source distribution from Git: https://github.com/iloveitaly/ipython-autoimport
    0.268702s DEBUG uv_fs Acquired lock for `https://github.com/iloveitaly/ipython-autoimport`
 uv_git::source::fetch repository=https://github.com/iloveitaly/ipython-autoimport, rev=Some(GitOid { len: 40, bytes: [52, 102, 51, 57, 50, 50, 50, 97, 100, 54, 57, 51, 53, 52, 57, 99, 100, 99, 55, 98, 55, 102, 53, 56, 99, 52, 56, 49, 56, 101, 100, 101, 101, 56, 52, 97, 56, 52, 99, 102] })
    0.273662s DEBUG uv_fs Released lock at `/Users/mike/.cache/uv/git-v0/locks/9d7b212698957267`
    0.273977s DEBUG uv_fs Acquired lock for `/Users/mike/.cache/uv/sdists-v8/git/fc917440db5f731f/ae526fb6b3f5b13f`
    0.274482s DEBUG uv_distribution::source No static `pyproject.toml` available for: git+https://github.com/iloveitaly/ipdb@support-executables (FieldNotFound("project"))
    0.275460s DEBUG uv_distribution::source No static `PKG-INFO` available for: git+https://github.com/iloveitaly/ipdb@support-executables (MissingPkgInfo)
    0.277344s DEBUG uv_distribution::source Using cached metadata for: git+https://github.com/iloveitaly/ipdb@support-executables
    0.277430s DEBUG uv_fs Released lock at `/Users/mike/.cache/uv/sdists-v8/git/fc917440db5f731f/ae526fb6b3f5b13f/.lock`
    0.278942s  10ms DEBUG uv_git::source Using existing Git source `https://github.com/iloveitaly/ipython-autoimport`
    0.292017s DEBUG uv_fs Released lock at `/Users/mike/.cache/uv/git-v0/locks/3650d239963831dc`
    0.292078s DEBUG uv_fs Acquired lock for `/Users/mike/.cache/uv/sdists-v8/git/cd2980dd86b60fd6/4f39222ad693549c`
```

### Platform

macOS 15.3 (24D60)

### Version

uv 0.6.3 (a0b9f22a2 2025-02-24)

### Python version

3.13.1

---

_Label `bug` added by @iloveitaly on 2025-03-10 22:20_

---

_Label `bug` removed by @konstin on 2025-03-10 22:29_

---

_Label `error messages` added by @konstin on 2025-03-10 22:29_

---

_Closed by @charliermarsh on 2025-03-11 03:00_

---
