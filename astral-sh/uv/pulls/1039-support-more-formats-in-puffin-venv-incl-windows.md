```yaml
number: 1039
title: "Support more formats in `puffin venv`, incl. windows support"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: konsti/support-more-python-specs
created_at: 2024-01-22T18:14:52Z
updated_at: 2024-01-23T15:35:08Z
url: https://github.com/astral-sh/uv/pull/1039
synced_at: 2026-01-10T15:39:03Z
```

# Support more formats in `puffin venv`, incl. windows support

---

_Pull request opened by @konstin on 2024-01-22 18:14_

Mirroring `virtualenv -p` and driven by the lack of `pythonx.y` in `PATH` on windows, this PR adds `-p x.y` support to `puffin venv` (first commit).

Supported formats:
* NEW: `-p 3.10` searches for an installed Python 3.10 (Looking for `python3.10` on linux/mac).
  Specifying a patch version is not supported
* `-p python3.10` or `-p python.exe` looks for a binary in `PATH`
* `-p /home/ferris/.local/bin/python3.10` uses this exact Python

In the second commit, we add python interpreter search on windows using `py --list-paths`. On windows, all python are called `python.exe` so the unix trick of looking for `python{}.{}` in `PATH` doesn't work. Instead, we ask the python launcher for windows to tell us about all installed packages. We should eventually migrate this to [PEP 514](https://peps.python.org/pep-0514/) by reading the registry entries ourselves.

---

_Label `enhancement` added by @konstin on 2024-01-22 18:14_

---

_Label `windows` added by @konstin on 2024-01-22 18:14_

---

_@konstin reviewed on 2024-01-22 18:20_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:56 on 2024-01-22 18:20_

I'm not clear about how we want to struct platform dependent symbols. I expect there will be two or three platforms generally (windows, unix and maybe wasm) and atm it looks like there won't be too much platform specific code.

I chose to not `cfg` gate them so it's easier to keep the code in sync with all platforms when working on one platform.

---

_Comment by @zanieb on 2024-01-22 19:15_

Minor conflict with #1040 

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/lib.rs`:11 on 2024-01-23 00:12_

Nit: mind adding `crate`?

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/python_query.rs`:42 on 2024-01-23 00:13_

Should we try to canonicalize first, and then fallback to `which`? Otherwise, this would be wrong if `python3.10` were a binary in the current directory...

---

_@charliermarsh approved on 2024-01-23 00:13_

Looks good to me!

---

_@charliermarsh reviewed on 2024-01-23 00:14_

---

_Review comment by @charliermarsh on `crates/puffin-interpreter/src/python_query.rs`:56 on 2024-01-23 00:14_

It seems okay to me for now. Harder choice if it only _compiles_ on one platform.

Should we add `installed_pythons_unix` for consistency, and just inline the `which` code?

---

_@konstin reviewed on 2024-01-23 15:31_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:42 on 2024-01-23 15:31_

To use a binary in the current directory, you'd need to use `./python3.10`.

---

_Merged by @konstin on 2024-01-23 15:35_

---

_Closed by @konstin on 2024-01-23 15:35_

---

_Branch deleted on 2024-01-23 15:35_

---
